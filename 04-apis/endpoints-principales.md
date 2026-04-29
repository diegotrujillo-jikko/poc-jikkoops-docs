# Endpoints Principales de JikkoOps

## Base URL

```
https://api.jikkoops.internal/v1
```

## Autenticación

Todos los endpoints requieren:
```
Authorization: Bearer {JWT_TOKEN}
X-Tenant-ID: {tenant_id}
```

## 1. Gestión de Productos

### Listar Productos

```
GET /products
Query params:
  ├── estado: ['activo', 'deprecated'] (opcional)
  ├── limit: 20 (default)
  └── offset: 0 (default)

Response:
{
    "productos": [
        {
            "id": "uuid",
            "nombre": "Liquidación Básica",
            "descripcion": "Módulo de liquidación con funcionalidades básicas",
            "funcionalidades": ["liquidacion", "reportes_basicos"],
            "protected_resources": ["LIQ-001", "LIQ-004"],
            "estado": "activo",
            "fecha_creacion": "2026-01-01T00:00:00Z"
        }
    ],
    "total": 5,
    "limit": 20,
    "offset": 0
}
```

### Obtener Producto

```
GET /products/{product_id}

Response:
{
    "id": "uuid",
    "nombre": "Liquidación Básica",
    ... (full product data)
}
```

## 2. Gestión de Plans

### Listar Plans

```
GET /plans
Query params:
  ├── producto_id: uuid (opcional - filtrar por producto)
  ├── estado: ['activo', 'deprecated']
  └── limit: 20

Response:
{
    "planes": [
        {
            "id": "uuid",
            "nombre": "Plan Básico",
            "producto_id": "uuid",
            "funcionalidades": ["liquidacion"],
            "protected_resources": ["LIQ-001", "LIQ-004"],
            "usuario_limite": 50,
            "expediente_limite_mes": 1000,
            "modelo_revenue": {
                "tipo": "CAUTE",
                "periodo_caute_dias": 180
            },
            "estado": "activo"
        }
    ],
    "total": 3
}
```

### Obtener Plan

```
GET /plans/{plan_id}

Response: (plan completo con detalles)
```

## 3. Gestión de Tenants

### Listar Tenants (admin)

```
GET /admin/tenants
Headers: Authorization: Bearer {ADMIN_TOKEN}

Query:
  ├── entity_id: uuid (opcional)
  ├── estado: ['activo', 'inactivo']
  ├── limit: 20
  └── search: "cali" (busca en nombre técnico)

Response:
{
    "tenants": [
        {
            "id": "uuid",
            "entity_id": "uuid",
            "nombre_tecnico": "municipio_cali",
            "plan_id": "uuid",
            "usuarios_limite": 50,
            "expedientes_mes_limite": 1000,
            "estado": "activo",
            "fecha_activacion": "2026-01-15",
            "fecha_vencimiento": "2027-01-15"
        }
    ],
    "total": 15
}
```

### Obtener Configuración del Tenant (self)

```
GET /tenant/config

Response:
{
    "tenant_id": "cali-2026",
    "nombre": "Municipio de Cali",
    "plan": {
        "id": "uuid",
        "nombre": "Plan Estándar"
    },
    "usuarios_limite": 50,
    "usuarios_activos": 42,
    "expedientes_mes_limite": 1000,
    "expedientes_mes_actual": 450,
    "modulo_revenue": {
        "tipo": "CAUTE",
        "estado": "activo",
        "limite_caute_expedientes": 1000,
        "limite_caute_fecha_vencimiento": "2026-06-15"
    },
    "fecha_vencimiento_contrato": "2027-01-15"
}
```

## 4. Gestión de Feature Flags

### Obtener Flags del Tenant

```
GET /tenant/flags
Query:
  ├── modulo: ['CILIN', 'DOS', 'SOCIA'] (opcional)
  └── estado: ['activo', 'inactivo'] (opcional)

Response:
{
    "flags": [
        {
            "codigo": "LIQ-001",
            "nombre": "Botón Liquidar",
            "tipo": "button",
            "modulo": "CILIN",
            "activo": true,
            "fecha_activacion": "2026-01-15T10:30:00Z",
            "razon": "Plan Estándar"
        },
        {
            "codigo": "SOC-001",
            "nombre": "Notificación Manual",
            "tipo": "action",
            "modulo": "SOCIA",
            "activo": false,
            "razon": "No incluido en plan"
        }
    ],
    "total": 42
}
```

### Obtener Flag Específico

```
GET /tenant/flags/{codigo_recurso}

Response:
{
    "codigo": "LIQ-001",
    "nombre": "Botón Liquidar",
    "activo": true,
    "tipo": "button",
    "modulo": "CILIN",
    "criticidad": "critica",
    "fecha_activacion": "2026-01-15T10:30:00Z",
    "feature": "Liquidación",
    "en_plan": true
}
```

## 5. Sincronización con Módulos

### Reportar Expediente Procesado (desde CILIN)

```
POST /sync/expedient-processed

Headers:
  └── X-Module-Secret: {secret_cilin}

Body:
{
    "tenant_id": "cali-2026",
    "expediente_id": "EXP-2026-001234",
    "fecha_procesamiento": "2026-04-29T14:30:00Z",
    "tipo": "liquidacion",
    "monto_liquidado": 150000,
    "estado": "completado"
}

Response:
{
    "expediente_registrado": true,
    "contador_mes": 451,
    "limite_mes": 1000,
    "superado_limite": false,
    "cambio_de_modelo": false
}
```

### Reportar Escalado de Volumen

```
POST /sync/escalado-volumen

Headers:
  └── X-Module-Secret: {secret}

Body:
{
    "tenant_id": "cali-2026",
    "evento": "caute_completado",
    "expedientes_procesados": 1050,
    "limite_caute": 1000,
    "nuevo_modelo": "CAUTE_THEN_PERCENTAGE",
    "porcentaje_recaudo": 0.10
}

Response:
{
    "escalado_aplicado": true,
    "flags_actualizados": ["SOC-002", "REP-002"],
    "notificacion_enviada": true,
    "fecha_efectiva": "2026-04-30T00:00:00Z"
}
```

## 6. Facturas

### Listar Facturas

```
GET /invoices
Query:
  ├── estado: ['emitida', 'pagada', 'vencida']
  ├── fecha_desde: YYYY-MM-DD
  ├── fecha_hasta: YYYY-MM-DD
  ├── limit: 20
  └── offset: 0

Response:
{
    "facturas": [
        {
            "id": "uuid",
            "numero_factura": "FACT-2026-04-CALI",
            "periodo_inicio": "2026-04-01",
            "periodo_fin": "2026-04-30",
            "fecha_emision": "2026-05-01",
            "fecha_vencimiento": "2026-05-15",
            "estado": "emitida",
            "total": 125000,
            "iva": 23750,
            "subtotal": 101250,
            "lineas": [
                {
                    "descripcion": "Expedientes procesados (450 x $200)",
                    "cantidad": 450,
                    "precio_unitario": 200,
                    "subtotal": 90000,
                    "origen": "expediente"
                }
            ]
        }
    ],
    "total": 5
}
```

### Obtener Factura

```
GET /invoices/{invoice_id}

Response: (factura completa)
```

### Descargar PDF de Factura

```
GET /invoices/{invoice_id}/pdf

Response: [PDF binary]
```

## 7. Contratos

### Listar Contratos

```
GET /admin/contracts
Query:
  ├── tenant_id: uuid
  ├── estado: ['activo', 'vencido', 'cancelado']
  ├── limit: 20
  └── offset: 0

Response:
{
    "contratos": [
        {
            "id": "uuid",
            "numero": "CONTRATO-2026-CALI-001",
            "tenant_id": "uuid",
            "fecha_firma": "2026-01-15",
            "fecha_inicio": "2026-01-15",
            "fecha_vencimiento": "2027-01-15",
            "estado": "activo",
            "servicios": ["CILIN", "DOS"],
            "modelo_revenue": {
                "tipo": "CAUTE",
                "periodo_caute_dias": 180
            },
            "valor_total_cop": 0,
            "forma_pago": "transferencia"
        }
    ],
    "total": 3
}
```

### Crear Contrato (admin)

```
POST /admin/contracts

Body:
{
    "tenant_id": "uuid",
    "numero": "CONTRATO-2026-CALI-002",
    "fecha_firma": "2026-04-29",
    "fecha_inicio": "2026-05-01",
    "fecha_vencimiento": "2027-05-01",
    "plan_id": "uuid",
    "servicios": ["CILIN", "DOS", "SOCIA"],
    "modelo_revenue": {
        "tipo": "PERCENTAGE_REVENUE",
        "porcentaje": 0.10,
        "minimo_mensual": 50000
    },
    "forma_pago": "transferencia"
}

Response:
{
    "id": "uuid",
    "numero": "CONTRATO-2026-CALI-002",
    "estado": "borrador",
    "mensaje": "Contrato creado. Siguiente: firma y activación"
}
```

### Activar Contrato

```
POST /admin/contracts/{contract_id}/activate

Body:
{
    "razon": "Firma completada y cliente aprobó"
}

Response:
{
    "contrato_id": "uuid",
    "estado": "activo",
    "flags_actualizados": 18,
    "tenant_notificado": true,
    "fecha_efectiva": "2026-04-29T15:30:00Z"
}
```

## 8. Protected Resources (Admin)

### Listar Recursos Protegidos

```
GET /admin/protected-resources
Query:
  ├── modulo: ['CILIN', 'DOS', 'SOCIA']
  ├── tipo: ['button', 'endpoint', 'view', 'action']
  ├── estado: ['activo', 'huerfano', 'deprecated']
  ├── feature_id: uuid (opcional)
  └── limit: 50

Response:
{
    "recursos": [
        {
            "id": "uuid",
            "codigo": "LIQ-001",
            "nombre": "Botón Liquidar",
            "tipo": "button",
            "modulo": "CILIN",
            "feature_id": "uuid",
            "feature_nombre": "Liquidación",
            "criticidad": "critica",
            "estado": "activo",
            "dependencias": ["LIQ-002"],
            "fecha_creacion": "2026-01-01"
        }
    ],
    "total": 42,
    "huerfanos": 2
}
```

### Agrupar Recursos Huérfanos a Funcionalidad

```
POST /admin/protected-resources/{codigo}/assign-feature

Body:
{
    "feature_id": "uuid",
    "razon": "Nuevo recurso agregado en deploy"
}

Response:
{
    "codigo": "DOC-005",
    "feature_asignada": "Gestión de Documentos",
    "estado": "activo"
}
```

## Error Responses

Todos los endpoints usan formato estándar de error:

```json
{
    "error": {
        "codigo": "INVALID_TENANT",
        "mensaje": "Tenant 'xyz' no encontrado",
        "detalles": {
            "tenant_id": "xyz",
            "causa": "El tenant ha sido cancelado"
        },
        "timestamp": "2026-04-29T14:30:00Z",
        "request_id": "req-12345"
    }
}
```

## Rate Limiting

```
Límites:
├── Por IP: 1000 requests/minuto
├── Por tenant: 500 requests/minuto
└── Por usuario: 100 requests/minuto

Response headers:
├── X-RateLimit-Limit: 1000
├── X-RateLimit-Remaining: 876
└── X-RateLimit-Reset: 1619685600

Si se excede:
└── HTTP 429 Too Many Requests
```

## Webhooks

JikkoOps puede enviar eventos a webhooks configurados:

```
Eventos:
├── contract.activated
├── escalado_volumen.detectado
├── flags.changed
├── invoice.created
├── invoice.paid
└── tenant.vencimiento_cercano (30 días antes)

Configurar en: /admin/tenants/{id}/webhooks
```
