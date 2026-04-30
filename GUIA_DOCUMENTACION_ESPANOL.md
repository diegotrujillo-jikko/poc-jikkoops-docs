# Guía de Documentación JikkoOps en Español

Esta guía presenta la estructura y enfoque para la documentación técnica de **JikkoOps** traducida y mejorada con los aprendizajes de la reunión del 2026-04-30.

## 📋 Estructura de Directorios

```
jikkoops-docs/
├── 00-general/
│   ├── 2026-04-30-transcripcion-reunion-jikkoops.md
│   └── README.md
│
├── 01-arquitectura/
│   ├── vision-general.md
│   ├── recursos-protegidos.md
│   └── principios-base-datos.md (NUEVO)
│
├── 02-procesos/
│   ├── flujo-contratos.md
│   └── modelos-ingresos.md
│
├── 03-datos/
│   ├── modelo-datos.md
│   ├── sincronizacion-flags.md
│   ├── estrategia-cache.md
│   └── gestion-migraciones-alembic.md (NUEVO)
│
├── 04-apis/
│   ├── endpoints-principales.md
│   ├── eventos-integracion.md
│   └── metricas-sdk.md (NUEVO)
│
├── 05-modulos-nucleares/
│   ├── dashboard-comercial.md
│   ├── gestion-contratos.md
│   ├── operaciones.md
│   ├── facturacion.md
│   └── configuracion.md
│
├── 06-riesgos-y-decisiones/
│   ├── riesgos-identificados.md
│   ├── decisiones-arquitecturales.md
│   └── seguridad-autenticacion-mfa.md (NUEVO)
│
└── GUIA_DOCUMENTACION_ESPANOL.md (ESTE ARCHIVO)
```

## 🎯 Cambio de Paradigma: Enfoque Base-Datos-Primero

El descubrimiento clave de la reunión 2026-04-30 es un **cambio fundamental** en cómo diseñamos sistemas:

### Enfoque Antiguo (Incremental)
```
Inicio: BD pequeña (10-20% definida)
Mes 1-6: Agregamos tablas
Mes 12: Agregamos más tablas  
Año 2+: Refactorización costosa y riesgosa
```

**Problema**: La BD se vuelve un "monstruo" que requiere reescritura.

### Enfoque Nuevo (Planificación Futura)
```
Inicio: BD fuerte (90% definida) usando IA
Mes 1-12: Refinamos, no refactorizamos
Año 2+: La BD aguanta casos de uso imprevistos
```

**Ventaja**: Diseñamos para 5 años en adelante desde el inicio.

## 🔧 Herramientas y Conceptos Clave

### 1. **Alembic** - Versionamiento Automático de BD
- Sistema de migrations para PostgreSQL/Python
- Cada cambio a la BD genera una versión
- Rollbacks automáticos si algo falla
- Historico completo para auditoría (requisito fiscal: 7 años)

### 2. **Iteración con IA**
- Usar ChatGPT como "experto en BD"
- Validar cada tabla con preguntas de caso de uso
- Ejemplo: "¿Esta BD soporta MFA en liquidación?" → No → Agregamos tabla
- Resultado: BD mejorada en minutos, no semanas

### 3. **MFA (Autenticación Multi-Factor)**
- Endpoints críticos (liquidación, cambios de flags) requieren MFA
- Implementación: Google Authenticator (TOTP)
- Validación: Backend verifica código + credenciales

### 4. **SDK Métricas**
- Cada botón/endpoint tiene ID único
- Registra: tiempo ejecución, tokens, costo
- Objetivo: Entender exactamente qué consume el sistema
- Permite facturación granular y optimización

### 5. **Playwright** - Testing Automatizado
- Abre navegador real y prueba cada botón
- Verifica que API ↔ UI funcionan correctamente
- Evita que UI solicite datos que BD no tiene

## 📊 Proceso Recomendado para Diseño

```
1. RECOPILAR CASOS DE USO
   └─ Qué quiere hacer cada usuario (operador, gerente, CFO)
   
2. DISEÑAR BD ITERATIVAMENTE
   └─ Usar ChatGPT/Claude para validar tablas
   └─ Preguntar: "¿Esta BD aguanta 50M liquidaciones/mes?"
   └─ Agregar índices, particionar si es necesario
   
3. DEFINIR ALEMBIC MIGRATIONS
   └─ Cloud Code genera migrations automáticas
   └─ Cada cambio se versionan
   
4. CONSTRUIR APIs ALREDEDOR DE BD
   └─ Las APIs exponen lo que la BD puede hacer
   └─ No agregas BD para cambios de API, cambias API
   
5. DESARROLLAR UI EN PARALELO
   └─ UI valida contra casos de uso
   └─ Playwright prueba que UI ↔ BD funcionan
   
6. VALIDAR COMPLETITUD
   └─ Todos los casos de uso funcionan
   └─ MFA en endpoints críticos
   └─ Métricas registran uso de cada función
```

## 💡 Casos de Uso Críticos (Usar para Validación de BD)

### Ejemplos de Casos de Uso

**CU-001: Crear Contrato con Modelo Caute + Porcentaje**
```
1. Manager crea contrato para municipio nuevo
2. Modelo: 0-1000 expedientes = $0, después = 10% recaudo
3. Sistema crea registros en: contracts, tenants, tenant_entitlements
4. Feature flags se activan/desactivan según modelo
5. Factura se genera automáticamente mes siguiente
```

**CU-002: Escalado Automático de Volumen**
```
1. Municipio supera 1000 expedientes en mes X
2. Sistema detecta limite y notifica a Manager
3. Feature flags de "% recaudo" se activan automáticamente
4. Próxima factura incluye % de recaudo
5. Cliente recibe notificación de cambio
```

**CU-003: Acceso Crítico Requiere MFA**
```
1. Operador intenta liquidar $50M
2. Sistema pide MFA: "Abre Google Authenticator y ingresa código"
3. Operador ingresa código TOTP
4. Sistema valida en backend contra tabla de MFA
5. Si válido: liquidación se procesa. Si inválido: se rechaza
```

**CU-004: Reporte Predictivo para Alcalde**
```
1. Alcalde pregunta al chatbot: "¿Cuánto dinero falta para pagar contratos?"
2. Sistema consulta ingresos proyectados, contratos, modelo-ingresos pendiente
3. IA genera: "Faltan $X, déficit en mes Z si no mejora recaudo"
4. Opciones: "¿Qué contratos están en riesgo?"
5. Sistema lista contratos + probabilidad de no pago
```

**CU-005: Cambiar Protected Resource sin Afectar Producción**
```
1. Dev agrega nuevo botón "Revisar firma digital"
2. Sistema marca como "recurso huérfano" (sin funcionalidad)
3. Manager agrupa a funcionalidad "Firma Digital"
4. Solo se activa en feature flags cuando es necesario
5. Clientes sin plan con firma → no ven el botón
```

## 🔐 Requisitos de Seguridad

### Endpoints que Requieren MFA
- `POST /liquidaciones` - Crear liquidación
- `PUT /tenant-flags/{id}` - Cambiar feature flags
- `POST /contracts/{id}/cambiar-modelo` - Cambiar modelo de pricing
- `DELETE /invoice/{id}` - Anular factura

### Tabla de MFA en BD
```sql
TABLE mfa_credentials
├── id: UUID
├── user_id: UUID (FK)
├── secret: STRING (TOTP secret, cifrado)
├── habilitado: BOOLEAN
├── intentos_fallidos: INT (para lockout)
├── ultimo_uso: TIMESTAMP
└── backup_codes: ARRAY<STRING> (para recuperación)
```

## 📈 Métricas y Observabilidad

### SDK Metrics - Estructura
```
Para cada Protected Resource:
├── resource_id: STRING (ej: "LIQ-001")
├── resource_name: STRING (ej: "Botón Liquidar")
├── tenant_id: UUID
├── execution_time_ms: INT
├── tokens_used: INT
├── cost_usd: DECIMAL
├── timestamp: TIMESTAMP
└── success: BOOLEAN
```

## 🎓 Ciclo de Aprendizaje de Desarrolladores

1. **Principiante**: Entiende qué es una BD
2. **Practicante**: Usa IA para diseñar tablas
3. **Líder de IA**: Refina código generado
4. **Especialista**: Lidera diseño de nuevos módulos

## 🚀 Próximos Pasos Inmediatos

1. ✅ Leer principios-base-datos.md
2. ⏳ Documentar casos de uso detallados
3. ⏳ Iterar BD en ChatGPT hasta 90% completitud
4. ⏳ Definir Alembic migrations
5. ⏳ Validar BD contra casos de uso
6. ⏳ Generar ERD/MER
7. ⏳ Implementar MFA
8. ⏳ Configurar SDK Métricas

---

**Creado**: 2026-04-30  
**Integración de**: Transcripción reunión JikkoOps (2026-04-30, 127 min)  
**Versión**: 1.0-DRAFT
