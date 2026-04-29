# Estrategia de Caché en JikkoOps

## Overview

JikkoOps sirve múltiples tenants concurrentemente. El caché es crítico para:
- Performance (reducir latencia)
- Escalabilidad (reducir carga en BD)
- Experiencia del usuario (respuestas rápidas)

## Niveles de Caché

```
┌─────────────────────────────────────────────┐
│  L1: Browser / Client Cache (5 min)         │
├─────────────────────────────────────────────┤
│  L2: API Gateway Cache (15 min)             │
├─────────────────────────────────────────────┤
│  L3: Application Cache (Redis) (30 min)     │
├─────────────────────────────────────────────┤
│  L4: Database (Source of Truth)             │
└─────────────────────────────────────────────┘
```

## L1: Browser Cache

Datos que NO cambian frecuentemente:

### Configuración (Estática)

```
GET /api/v1/config/products
Cache-Control: public, max-age=3600  // 1 hora

Datos:
├── Lista de productos
├── Planes disponibles
├── Funcionalidades por plan
└── Protected resources metadatos
```

### Metadatos de Módulos

```
GET /api/v1/modules/metadata
Cache-Control: public, max-age=1800  // 30 min

Datos:
├── Versión de CILIN, DOS, SOCIA
├── Información de integración
└── URLs de endpoints
```

## L2: API Gateway Cache

Cache en la capa de API (nginx, API Gateway):

### Rules de Invalidación

```
Cache por ruta:

GET /api/v1/products/*           → max-age=3600
GET /api/v1/plans/*              → max-age=1800
GET /api/v1/protected-resources/*→ max-age=900
GET /api/v1/tenants/{id}/config  → max-age=600
GET /api/v1/tenants/{id}/flags   → max-age=300
POST/PUT/DELETE /*               → no-cache (invalidar siempre)
```

### Invalidación por Evento

```
Cuando ocurre:           Invalidar:
─────────────────────    ──────────────────────
Cambio en plan           /api/*/products/*
                         /api/*/plans/*

Cambio en contrato       /api/v1/tenants/{id}/*
                         /api/*/flags/*

Cambio en flags          /api/*/tenants/{id}/flags
                         /api/*/protected-resources/*

Cambio en config         /api/v1/config/*
```

## L3: Application Cache (Redis)

Cache distribuido para datos calientes:

### Estructura de Keys

```
jikkoops:products:{product_id}
├── TTL: 3600 segundos (1 hora)
└── Invalidar: Cuando se modifica producto

jikkoops:plans:{plan_id}
├── TTL: 1800 segundos (30 min)
└── Invalidar: Cuando se modifica plan

jikkoops:tenant:{tenant_id}:config
├── TTL: 600 segundos (10 min)
└── Invalidar: Cuando se activa contrato o cambian entitlements

jikkoops:tenant:{tenant_id}:flags
├── TTL: 300 segundos (5 min)
└── Invalidar: Cuando se activa/desactiva recurso

jikkoops:protected-resources:{codigo}
├── TTL: 900 segundos (15 min)
└── Invalidar: Cuando se modifica recurso o cambia estado

jikkoops:feature-catalog:{modulo}
├── TTL: 1800 segundos (30 min)
└── Invalidar: Cuando se modifica feature o recursos
```

### Invalidación Manual

```javascript
// JavaScript / Node.js

async function invalidateCache(pattern) {
    const keys = await redis.keys(pattern);
    if (keys.length > 0) {
        await redis.del(...keys);
        console.log(`Invalidated ${keys.length} keys`);
    }
}

// Ejemplos:
await invalidateCache('jikkoops:tenant:cali-2026:*');
await invalidateCache('jikkoops:plans:*');
await invalidateCache('jikkoops:protected-resources:*');
```

### Warm-up de Caché

Al iniciar aplicación o deploy:

```javascript
async function warmupCache() {
    const products = await db.query("SELECT * FROM products WHERE estado = 'activo'");
    for (const product of products) {
        await redis.setex(
            `jikkoops:products:${product.id}`,
            3600,
            JSON.stringify(product)
        );
    }
    
    const plans = await db.query("SELECT * FROM plans WHERE estado = 'activo'");
    for (const plan of plans) {
        await redis.setex(
            `jikkoops:plans:${plan.id}`,
            1800,
            JSON.stringify(plan)
        );
    }
    
    console.log('Cache warmed up');
}
```

## L4: Database

Estructura optimizada para consultas frecuentes:

### Índices Críticos (Redundancia controlada)

```sql
-- Denormalizaciones para performance
ALTER TABLE tenants ADD COLUMN plan_id_cached UUID; -- duplicado para rápido acceso
ALTER TABLE feature_flags ADD COLUMN codigo_recurso TEXT; -- denorm para WHERE rápido
ALTER TABLE contracts ADD COLUMN modelo_revenue_cached JSONB; -- para lectura rápida

-- Triggers para mantener caché sincronizado
CREATE TRIGGER sync_plan_cache 
AFTER UPDATE ON plans 
FOR EACH ROW EXECUTE sync_tenant_cache();

CREATE TRIGGER sync_flag_cache 
AFTER UPDATE ON feature_flags 
FOR EACH ROW EXECUTE invalidate_redis();
```

## Estrategia por Módulo

### Productos y Planes (LOW FREQUENCY)

```
Cambio: Raro (máx 1-2/mes)
Cache: L1 + L2 + L3
TTL: 
├── L1 (Browser): 1 hora
├── L2 (API Gateway): 30 min
└── L3 (Redis): 1 hora

Invalidación: Manual por manager
┌─ UI: Admin → Productos → Editar
└─ Sistema invalida todos los niveles

Fallback: Si Redis no disponible, ir a BD
```

### Configuración de Tenant (MEDIUM FREQUENCY)

```
Cambio: Ocasional (cuando se activa contrato, escala de volumen)
Cache: L2 + L3
TTL:
├── L2 (API Gateway): 10 min
└── L3 (Redis): 10 min

Invalidación: Automática cuando:
├── Se activa/vence contrato
├── Se cambian entitlements
├── Se escalado de volumen detectado
└── Manager invalidate manual

Fallback: Si Redis no disponible, ir a BD (sin L1)
```

### Feature Flags (HIGH FREQUENCY)

```
Cambio: Frecuente (diario o más)
Cache: L2 + L3
TTL:
├── L2 (API Gateway): 5 min
└── L3 (Redis): 5 min

Invalidación: Casi inmediata
├── Cambio automático (escalado): invalida inmediatamente
├── Cambio manual: invalida en <1 segundo
└── Cron de sincronización: cada 5 minutos

Fallback: Si Redis no disponible, SIEMPRE ir a BD
         (flags deben estar actualizados siempre)
```

## Invalidación Inteligente

### On-Demand Invalidation

```javascript
// Cuando se actualiza un contrato
async function updateContract(contractId, newData) {
    // 1. Actualizar BD
    await db.contracts.update(contractId, newData);
    
    // 2. Invalidar caché relacionado
    const contract = await db.contracts.findOne(contractId);
    await redis.del(`jikkoops:tenant:${contract.tenant_id}:*`);
    
    // 3. Notificar invalidación a otros servidores (pub/sub)
    await pubSub.publish('cache:invalidate', {
        pattern: `jikkoops:tenant:${contract.tenant_id}:*`,
        reason: 'contract_updated'
    });
    
    // 4. Invalidar nivel 2 (API Gateway)
    await invalidateAPIGateway(`/tenants/${contract.tenant_id}/*`);
}
```

### Broadcast Invalidation (multi-servidor)

```javascript
// Redis Pub/Sub para sincronización entre servidores

const pubSub = redis.createClient();

// Subscriber (todos los servidores)
pubSub.subscribe('cache:invalidate', (pattern, reason) => {
    console.log(`Invalidating ${pattern} (reason: ${reason})`);
    redis.keys(pattern).then(keys => {
        if (keys.length) redis.del(...keys);
    });
});

// Publisher (cuando ocurre cambio)
redis.publish('cache:invalidate', {
    pattern: 'jikkoops:tenant:cali-2026:*',
    reason: 'plan_activated'
});
```

## Monitoreo de Caché

### Métricas Clave

```
Hit Rate por Nivel:
├── L1 (Browser): > 90%
├── L2 (API Gateway): > 80%
└── L3 (Redis): > 70%

Size de Caché:
├── Redis memory: < 2GB (con política eviction LRU)
├── API Gateway cache: < 500MB
└── Browser cache: límite del navegador

Staleness:
├── Máximo acceptable: 5 minutos para datos no críticos
├── Máximo acceptable: 30 segundos para flags
└── Máximo acceptable: inmediato para transacciones
```

### Dashboard de Monitoreo

```
JikkoOps Caché Status:

Redis:
├── Memory used: 1.2 / 2 GB (60%)
├── Keys: 15,234
├── Hit rate: 85%
├── TTL promedio: 12 min
└── Evictions (24h): 42

API Gateway:
├── Cache size: 350 / 500 MB (70%)
├── Hit rate: 88%
├── Purges (24h): 3
└── Slowest purge: 200ms

Tenant Flags (más crítico):
├── Tiempo de actualización: < 100ms
├── Desincronizaciones detectadas: 0
└── Last bulk invalidation: 2h ago

Invalidations (24h):
├── Manual: 5
├── Automáticas: 187
└── Por escalado de volumen: 12
```

## Troubleshooting

### Problema: Datos Stale en Cliente

```
Síntoma: Usuario ve plan antiguo después de upgrade

Diagnóstico:
1. Verificar flag en BD: SELECT * FROM feature_flags WHERE tenant_id = 'cali-2026'
2. Verificar Redis: redis-cli GET 'jikkoops:tenant:cali-2026:flags'
3. Verificar API Gateway: nginx cache keys

Solución:
# Invalidar todos los niveles
npm run cache:invalidate --tenant=cali-2026

# O manual en Redis
redis-cli DEL 'jikkoops:tenant:cali-2026:*'

# O forzar refresh en cliente
# Cliente: Ctrl+Shift+R (hard refresh)
```

### Problema: Redis Down

```
Fallback Automático:
├── Nivel 3 (Redis): ✗ DOWN
├── Sistema: Usa BD directamente
├── Performance: Degradada (sin L3)
├── Usable: SÍ (pero lento)

Acción:
1. Alertar al equipo de DevOps
2. Usuarios ven performance degradada pero funciona
3. Una vez Redis se recupera:
   └── Warm-up automático de caché

Código:
try {
    return redis.get(key);
} catch (error) {
    console.error('Redis error, fallback to DB', error);
    return db.query(key);
}
```

## Best Practices

1. **TTL adecuado**: No cachear datos que cambian a menudo
2. **Invalidación activa**: No esperar a que expire el TTL
3. **Fallbacks**: Siempre tener estrategia si caché no está disponible
4. **Monitoreo**: Alertar si hit rate cae <70%
5. **Documentación**: Cada cache key debe estar documentado
6. **Testing**: Validar que invalidación funciona correctamente
7. **Seguridad**: No cachear datos sensibles sin cifrado
