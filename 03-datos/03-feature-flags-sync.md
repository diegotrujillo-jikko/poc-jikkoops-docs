# Sincronización de Feature Flags

## Overview

Los Feature Flags en JikkoOps controlan qué Protected Resources están disponibles para cada tenant en runtime. Deben estar siempre sincronizados entre:

- **Plan del Tenant**: Qué funcionalidades tiene contratadas
- **Entitlements**: Qué tiene activado en BD
- **Runtime Flags**: Qué ve el usuario en aplicación

## Tipos de Feature Flags

### 1. Flag por Contrato (Plan-based)

Automático cuando se activa un contrato:

```
Contrato: "Plan Estándar + DOS"
  ├─→ Activar automáticamente:
  │   ├─ LIQ-001 (Botón Liquidar)
  │   ├─ LIQ-004 (Dashboard Liquidación)
  │   ├─ DOC-001 (Crear Documento)
  │   └─ ... (todos en el plan)
  │
  └─→ Mantener desactivado:
      ├─ SOC-001 (Notificación - Plan Premium)
      └─ ... (recursos fuera del plan)
```

### 2. Flag por Límite (Escalado automático)

Cuando se supera un límite, se activan/desactivan recursos:

```
Tenant: Caute limit = 1000 expedientes/mes

Mes 1 (350 expedientes):
  └─ Flag: SOC-002 (% de recaudo) = OFF

Mes 2 (1050 expedientes):
  ├─ Límite superado detectado
  ├─ Sistema automático:
  │  ├─ Activa: SOC-002 (% de recaudo)
  │  ├─ Genera nota en tenant
  │  └─ Envía email al cliente
  └─ Flag: SOC-002 = ON (próxima factura aplica %)
```

### 3. Flag Manual (Manager override)

Manager puede activar/desactivar manualmente con aprobación:

```
Caso: Cliente solicita módulo DOS en mitad del mes

Proceso:
1. Cliente solicita upgrade
2. Manager genera propuesta
3. Cliente aprueba y firma enmienda
4. Manager va a JikkoOps → Tenant → Feature Flags
5. Activa manualmente: DOC-* (todos los recursos de DOS)
6. Entrada en audit: "Activados recursos DOS - Enmienda 2026-004"
7. Próxima factura incluye ajuste de precio
```

### 4. Flag Temporal (Time-bounded)

Activado por período limitado:

```
Caso: Promoción especial por 30 días

Flag: REP-001 (Reportes avanzados)
├── Activo desde: 2026-05-01
├── Activo hasta: 2026-05-31
├── Automaticamente se desactiva al 2026-06-01
└── Notificación al cliente: "Promoción vence en 7 días"
```

## Sincronización Automática

### Flujo de Activación de Contrato

```
1. Contrato se aprueba y firma
   ↓
2. Manager activa contrato en JikkoOps
   └─ Estado: "Activo"
   ↓
3. Sistema automático (scheduled job cada 5 minutos):
   ├─ Lee plan del contrato
   ├─ Lee Protected Resources del plan
   ├─ Crea/actualiza feature_flags para cada recurso
   └─ Estado flags: ON/OFF según plan
   ↓
4. Flags se replican a BD del tenant
   ├─ Tenant puede consultar: GET /api/flags/mi-tenant
   └─ Caché invalidado para evitar stale data
   ↓
5. UI consume flags en cada request
   ├─ Muestra/oculta botones según flags
   ├─ Habilita/desabilita endpoints según flags
   └─ Usuario ve solo lo contratado
```

### Detección de Escalado Automático

```
Scheduled Job: "escalado_de_volumen" (cada 1 hora)

1. Por cada tenant activo:
   ├─ Contar expedientes del mes
   ├─ Comparar con limite_expedientes_mes
   ├─ SI expedientes > limite ENTONCES:
   │  ├─ Cambiar modelo_revenue a "CAUTE_COMPLETE"
   │  ├─ Activar flags de "% de recaudo"
   │  ├─ Crear entrada en audit_log
   │  └─ Enviar notificación al tenant
   │
   └─ SI escalado ocurrió ENTONCES:
      └─ Marcar contrato para revisión (próxima facturación)
```

### Sincronización con SILIN

Cuando SILIN (módulo de liquidación) genera liquidaciones, debe confirmar:

```
POST /jikkoops/api/v1/sync-expedient
{
    "tenant_id": "cali-2026",
    "expediente_id": "EXP-2026-001234",
    "fecha": "2026-04-29",
    "estado": "liquidado"
}

JikkoOps:
├─ Registra expediente
├─ Incrementa contador del mes
├─ Valida si supera límite modelo-ingresos
├─ Actualiza flags si es necesario
└─ Response: { "expediente_registrado": true, "flags_updated": false }
```

## Matriz de Sincronización

### Estados Esperados por Escenario

```
Escenario 1: Plan Básico (SILIN only)
├── LIQ-001 (Botón Liquidar): ON ✓
├── LIQ-004 (Dashboard): ON ✓
├── DOC-001 (Crear Documento): OFF ✗
├── SOC-001 (Notificación): OFF ✗
└── REP-001 (Reportes): OFF ✗

Escenario 2: Plan Estándar (SILIN + DOS)
├── LIQ-001 (Botón Liquidar): ON ✓
├── LIQ-004 (Dashboard): ON ✓
├── DOC-001 (Crear Documento): ON ✓
├── DOC-003 (Firma digital): ON ✓
├── SOC-001 (Notificación): OFF ✗ (requiere Plan Premium)
└── REP-001 (Reportes): OFF ✗ (requiere Plan Premium)

Escenario 3: Caute Completo (después de superar límite)
├── LIQ-001 (Botón Liquidar): ON ✓
├── SOC-002 (% de recaudo): ON ✓ (activado automáticamente)
├── REP-002 (Reportes de Recaudo): ON ✓ (activado automáticamente)
└── Otros según plan: ON/OFF
```

## Validación de Sincronización

### Health Check (cada 30 minutos)

```bash
Script: check_flag_sync.sh

Para cada tenant activo:
1. Leer plan de contrato
2. Leer flags actuales en feature_flags table
3. Calcular flags esperados
4. SI esperados != actuales ENTONCES:
   ├─ Log WARNING: "Flag desync detectado"
   ├─ Alertar a DevOps
   ├─ Intentar resincronización automática
   └─ Si falla, manual: manager debe revisar
```

### Auditoría de Cambios

```sql
TABLE feature_flag_audit
├── id: UUID
├── flag_id: UUID
├── accion: ENUM ['activado', 'desactivado', 'actualizado']
├── timestamp: TIMESTAMP
├── razon: STRING
├── activado_por: UUID (usuario o 'system')
├── cambios: JSON (before/after)
└── tenant_id: UUID
```

**Cada cambio debe registrarse** incluso si es automático. Servicio de auditoría debe poder rastrear:
- ¿Cuándo se activó cada recurso?
- ¿Por quién (usuario o sistema)?
- ¿Por qué razón?

## Resincronización Manual

### Caso: Desincronización Detectada

```
Problema: DB dice LIQ-001=OFF pero plan dice =ON

Solución:
1. Manager va a Tenant → Feature Flags
2. Click: "Resincronizar con Plan"
3. Sistema:
   ├─ Lee plan actual
   ├─ Lee flags actuales
   ├─ Calcula diferencias
   ├─ Sincroniza todo
   └─ Envía notificación al tenant
4. Confirmation: "Resincronización completada"
```

### API de Resincronización

```
POST /jikkoops/api/v1/admin/tenants/{tenant_id}/resync-flags

Response:
{
    "tenant_id": "cali-2026",
    "flags_updated": 15,
    "flags_activated": 5,
    "flags_deactivated": 2,
    "timestamp": "2026-04-29T10:30:00Z",
    "resync_reason": "manual_request"
}
```

## Sincronización con Terceros

### Replicar Flags a SIGIA

Si el tenant usa SIGIA (SILIN, DOS, SOCIA), deben saber qué está activo:

```
POST https://sigia.internal/api/v1/sync-flags
{
    "tenant_id": "cali-2026",
    "flags": [
        { "codigo": "LIQ-001", "activo": true },
        { "codigo": "DOC-001", "activo": true },
        { "codigo": "SOC-001", "activo": false }
    ]
}

SIGIA responde:
{
    "tenant_id": "cali-2026",
    "flags_aplicados": 3,
    "timestamp": "2026-04-29T10:31:00Z"
}
```

## Rollback de Flags

Si algo falla después de cambiar flags:

```
Caso: Activar DOS para tenant, pero SILIN + DOS = timeout

Acción manual:
1. Revertir flags a estado anterior
2. Logging: "Rollback de flags - DOS causes timeout with SILIN"
3. Investigar: ¿Incompatibilidad?
4. Notificar al cliente
5. Escalar a arquitectura

Script:
npm run flags:rollback --tenant=cali-2026 --restore-point=2026-04-29-09:00
```

## Monitoreo de Flags

### Dashboard de Flags (G-COPS)

```
Resumen:
├── Total de tenants: 15
├── Total de recursos inventariados: 42
├── Flags activos en total: 287
├── Flags inactivos en total: 198
│
Por estado:
├── Sincronizados correctamente: 14 tenants ✓
├── Desincronizados: 1 tenant (cali-2026) ⚠
└── Necesita revisión manual: 0

Cambios últimas 24h:
├── Flags activados: 3
├── Flags desactivados: 2
├── Resincronizaciones: 0
└── Errores: 0
```

## Best Practices

1. **Nunca** editar flags directamente en BD
2. **Siempre** usar UI de JikkoOps o APIs documentadas
3. **Validar** plan antes de cambiar flags
4. **Auditar** cada cambio - sin excepciones
5. **Resincronizar** si hay duda
6. **Notificar** al tenant si hay cambios
7. **Documentar** razón de cambios manuales
