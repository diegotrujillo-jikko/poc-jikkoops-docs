# Endpoints Principales - API REST de JikkoOps

## Autenticación

Todos los endpoints requieren:
- **Header**: `Authorization: Bearer {JWT_TOKEN}`
- **Header**: `X-Tenant-ID: {TENANT_UUID}`
- Endpoints críticos: Requieren **MFA** adicional

## Clientes (Entidades)

### Listar Clientes
```
GET /v1/clientes
Parámetros: page, limit, estado, region
Respuesta: [{ id, nombre, nit, tipo, estado }]
```

### Obtener Cliente
```
GET /v1/clientes/{cliente_id}
Respuesta: { id, nombre, nit, tipo, region, email, estado, fecha_creacion }
```

### Crear Cliente
```
POST /v1/clientes
Body: { nombre, nit, tipo, region, email, telefono, direccion }
Respuesta: { id, ... }
```

## Tenants

### Listar Tenants
```
GET /v1/tenants
Parámetros: entity_id, estado
Respuesta: [{ id, entity_id, nombre_tecnico, plan_id, estado }]
```

### Crear Tenant
```
POST /v1/tenants
Body: { entity_id, nombre_tecnico, plan_id, usuarios_limite, expedientes_limite_mes }
Respuesta: { id, ... }
```

### Configurar Tenant
```
PUT /v1/tenants/{tenant_id}
Body: { plan_id, usuarios_limite, estado }
Respuesta: { id, ... actualizado ... }
```

## Contratos

### Listar Contratos
```
GET /v1/contratos
Parámetros: tenant_id, estado, fecha_inicio, fecha_fin
Respuesta: [{ id, tenant_id, numero, estado, valor_total_cop, fecha_vencimiento }]
```

### Obtener Contrato
```
GET /v1/contratos/{contrato_id}
Respuesta: { id, tenant_id, numero, modelo_revenue, servicios, estado, ... }
```

### Crear Contrato
```
POST /v1/contratos
Body: { tenant_id, numero, fecha_firma, servicios, modelo_revenue, valor_total_cop }
Respuesta: { id, ... }
```

### Actualizar Modelo de Pricing ⭐ REQUIERE MFA
```
PUT /v1/contratos/{contrato_id}/modelo-ingresos
Requiere: MFA code + credentials
Body: { modelo_revenue, valor_total_cop, justificacion }
Respuesta: { id, modelo_revenue, audit_log_id }
```

## Feature Flags

### Listar Flags
```
GET /v1/feature-flags
Parámetros: tenant_id, recurso_protegido_id, activo
Respuesta: [{ id, tenant_id, recurso_codigo, activo, fecha_activacion }]
```

### Cambiar Flag ⭐ REQUIERE MFA
```
PUT /v1/feature-flags/{flag_id}
Requiere: MFA code + credentials
Body: { activo, razon }
Respuesta: { id, activo, fecha_actualizacion }
```

### Activar Flag en Bulk
```
POST /v1/feature-flags/bulk
Requiere: MFA code
Body: { tenant_id, flags: [{ recurso_id, activo }], razon }
Respuesta: { count, results: [...] }
```

## Facturas

### Listar Facturas
```
GET /v1/facturas
Parámetros: tenant_id, estado, periodo_inicio, periodo_fin
Respuesta: [{ id, numero_factura, estado, total, fecha_emision }]
```

### Obtener Factura
```
GET /v1/facturas/{factura_id}
Respuesta: { id, numero_factura, lineas, subtotal, iva, total, estado }
```

### Generar Factura
```
POST /v1/facturas
Body: { tenant_id, periodo_inicio, periodo_fin, modelo_revenue }
Respuesta: { id, numero_factura, total, estado: 'borrador' }
```

### Emitir Factura
```
POST /v1/facturas/{factura_id}/emitir
Requiere: MFA code (factura >$100K)
Respuesta: { id, numero_factura, estado: 'emitida' }
```

### Anular Factura ⭐ REQUIERE MFA
```
DELETE /v1/facturas/{factura_id}
Requiere: MFA code + justificacion
Body: { motivo }
Respuesta: { id, estado: 'anulada', audit_log_id }
```

## Recursos Protegidos

### Listar Recursos
```
GET /v1/recursos-protegidos
Parámetros: modulo, criticidad, estado
Respuesta: [{ id, codigo, nombre, tipo, modulo, criticidad }]
```

### Obtener Recurso
```
GET /v1/recursos-protegidos/{recurso_id}
Respuesta: { id, codigo, nombre, dependencias, estado, funcionalidad_id }
```

### Crear Recurso
```
POST /v1/recursos-protegidos
Body: { codigo, nombre, tipo, modulo, descripcion, criticidad }
Respuesta: { id, codigo, estado: 'huerfano' }
```

### Agrupar Recurso a Funcionalidad
```
PUT /v1/recursos-protegidos/{recurso_id}/funcionalidad
Body: { funcionalidad_id }
Respuesta: { id, estado: 'activo', funcionalidad_id }
```

## Plans

### Listar Plans
```
GET /v1/planes
Parámetros: producto_id, estado
Respuesta: [{ id, nombre, producto_id, usuario_limite, modelo_revenue }]
```

### Obtener Plan
```
GET /v1/planes/{plan_id}
Respuesta: { id, nombre, producto_id, funcionalidades, recursos_protegidos, modelo_revenue }
```

### Crear Plan
```
POST /v1/planes
Body: { nombre, producto_id, funcionalidades, modelo_revenue, usuario_limite }
Respuesta: { id, ... }
```

## Métricas (SDK Metrics)

### Registrar Métrica
```
POST /v1/metricas
Body: { 
  resource_id: "LIQ-001",
  execution_time_ms: 245,
  tokens_used: 150,
  cost_usd: 0.0045,
  success: true
}
Respuesta: { id, timestamp }
```

### Obtener Métricas
```
GET /v1/metricas
Parámetros: resource_id, tenant_id, fecha_inicio, fecha_fin
Respuesta: [{ resource_id, execution_time_ms, tokens_used, cost_usd }]
```

### Dashboard de Costos
```
GET /v1/metricas/dashboard?periodo=mes
Respuesta: {
  total_cost_usd: 1234.56,
  por_recurso: [{ resource_id, cost_usd, count }],
  promedio_tiempo_ms: 125
}
```

## Auditoría

### Obtener Audit Log
```
GET /v1/audit-log
Parámetros: tenant_id, tabla_afectada, fecha_inicio, fecha_fin
Respuesta: [{ 
  id, tabla, registro_id, operacion, 
  valores_anterior, valores_nuevo, 
  usuario_id, timestamp 
}]
```

## Códigos de Estado

| Código | Significado |
|--------|-------------|
| 200 | OK - Solicitud exitosa |
| 201 | Created - Recurso creado |
| 400 | Bad Request - Parámetros inválidos |
| 401 | Unauthorized - Token ausente o inválido |
| 403 | Forbidden - Acceso denegado / MFA requerida |
| 404 | Not Found - Recurso no existe |
| 409 | Conflict - Recurso duplicado |
| 422 | Unprocessable Entity - Validación fallida |
| 429 | Too Many Requests - Rate limit excedido |
| 500 | Internal Server Error - Error del servidor |

## Rate Limiting

- **Límite estándar**: 1000 requests/hora por tenant
- **Límite crítico**: 100 requests/hora para DELETE y PUT con MFA
- **Header**: `X-RateLimit-Remaining: 999`

## Errores Estándar

```json
{
  "status": "error",
  "code": "VALIDATION_ERROR",
  "message": "Descripción del error en español",
  "details": {
    "field": "nombre",
    "reason": "requerido"
  },
  "timestamp": "2026-04-30T12:00:00Z"
}
```

---

**Versión**: 1.0 | **Actualizado**: 2026-04-30
