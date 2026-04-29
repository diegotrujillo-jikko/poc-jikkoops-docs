# Dashboard Comercial

## Propósito

Vista consolidada para managers y vendedores que permite monitorear:
- Clientes activos y su estado
- Contratos vigentes y próximas renovaciones
- Ingresos proyectados y reales
- Performance de planes y servicios
- Alertas y problemas operacionales

## Usuarios

- **Sales Manager**: Seguimiento de oportunidades y clientes
- **Account Manager**: Renovaciones y escalados
- **CFO**: Proyecciones de revenue
- **Admin Manager**: Monitoreo de operación general

## Secciones Principales

### 1. KPIs en Línea

```
┌────────────────────────────────────────────────────┐
│ Clientes Activos: 15    │ Ingresos Este Mes: $2.5M │
│ Contratos Vencidos: 2   │ Ingresos Proyectados: $3M│
└────────────────────────────────────────────────────┘
```

**Métricas**:
- Clientes activos (estado = "activo")
- Contratos vencidos (fecha_vencimiento <= hoy)
- Clientes en riesgo (>30 días vencido sin pagar)
- Ingresos facturados este mes
- Ingresos proyectados (rest of quarter)

### 2. Pipeline de Renovaciones

```
Próximas 90 días:
├─ 30 días: 2 contratos venciendo (CALI, PEREIRA)
├─ 60 días: 3 contratos (MANIZALES, ARMENIA, CARTAGO)
└─ 90 días: 1 contrato (MEDELLIN)
```

**Acción**: Manager puede:
- Click en contrato → Ver detalles y contactos
- Click "Renovar" → Crear nueva propuesta
- Click "Alertar" → Enviar email de recordatorio

### 3. Rendimiento por Plan

```
Plan Básico:
├── Clientes: 5
├── Ingresos: $500K
├── Churn rate: 0%
└── NPS: 8.2/10

Plan Estándar:
├── Clientes: 7
├── Ingresos: $1.8M
├── Churn rate: 5% (1 cliente)
└── NPS: 7.9/10

Plan Premium:
├── Clientes: 3
├── Ingresos: $1.2M
├── Churn rate: 0%
└── NPS: 8.8/10
```

### 4. Alertas Operacionales

```
Alertas (últimas 24h):
├─ 🔴 CRÍTICA: CALI - Factura 90 días vencida
├─ 🟡 ADVERTENCIA: PEREIRA - Acceso deshabilitado por 3 días
├─ 🟡 ADVERTENCIA: Escalado detectado en MANIZALES
└─ 🟢 INFO: Nuevo contrato activo: IBAGUÉ

Click en alerta → Detalles y acciones recomendadas
```

### 5. Proyección de Ingresos

```
Gráfico de línea: Últimos 12 meses

                    Ingresos Reales vs Proyectados
$3.5M ├─────────────────────────────────
      │      .___
$3.0M ├─────/     \______________
      │    /                      \___
$2.5M ├───/________________________
      │
$2.0M ├─
      │  A M J J A S O N D E F M
```

- Línea azul: Ingresos reales
- Línea punteada: Ingresos proyectados
- Sombra: Margen de error ±10%

### 6. Matriz de Clientes

```
Tabla filterable y sorteable:

| Cliente    | Plan      | Estado   | Ingresos  | Vence     | Acción       |
|------------|-----------|----------|-----------|-----------|--------------|
| CALI       | Estándar  | Activo   | $200K     | 2027-05-01| Renovar      |
| PEREIRA    | Básico    | Riesgo   | $50K      | 2026-05-15| Contactar    |
| MANIZALES  | Premium   | Activo   | $400K     | 2027-08-01| Seguimiento  |
| ARMENIA    | Estándar  | Activo   | $150K     | 2026-07-20| Escalado +30d|

Filtros disponibles:
├── Por estado: [Activo, Riesgo, Vencido, Cancelado]
├── Por plan: [Básico, Estándar, Premium]
├── Por región: [Dropdown de departamentos]
└── Por ingresos: [Slider rango]
```

## Interacciones Clave

### Flujo: Renovar Contrato

```
Manager abre Dashboard
    ↓
Ve "CALI vence en 30 días"
    ↓
Click "Renovar"
    ↓
Sistema abre modal:
├─ Cliente: Municipio de Cali
├─ Contrato actual: Plan Estándar
├─ Ingresos últimos 12m: $2.4M
├─ Propuesta recomendada: Plan Premium (+30% precio)
├─ Acción: "Crear propuesta" o "Personalizar"
    ↓
Manager personaliza plan
    ↓
Click "Enviar propuesta"
    ↓
Sistema crea propuesta en G-COPS
    ↓
Email automático a cliente
```

### Flujo: Alertas de Riesgo

```
Sistema detecta (automático):
├─ Cliente PEREIRA: 30 días vencido
├─ Expedientes superan límite caute
└─ Acceso bloqueado por 3+ días

Dashboard muestra alerta
    ↓
Manager click en alerta
    ↓
Abre detalle:
├─ Causa de riesgo
├─ Historial de comunicación
├─ Acciones recomendadas:
│  ├─ [ ] Enviar recordatorio pago
│  ├─ [ ] Contactar al cliente
│  ├─ [ ] Generar cobro coactivo
│  └─ [ ] Escalar a CFO
└─ Contactos del cliente (directo/email/teléfono)
```

## Datos que Consulta

```
SELECT
    e.nombre,
    t.id,
    c.numero,
    c.fecha_vencimiento,
    c.modelo_revenue,
    SUM(inv.total) as ingresos_12m,
    COUNT(CASE WHEN inv.estado='pagada') as facturas_pagadas,
    MAX(inv.fecha_pago) as ultimo_pago,
    (SELECT SUM(cantidad) FROM expedientes 
     WHERE tenant_id=t.id AND fecha >= DATE_TRUNC('month', NOW())) as expedientes_mes
FROM
    entities e
    JOIN tenants t ON e.id = t.entity_id
    JOIN contracts c ON t.id = c.tenant_id
    LEFT JOIN invoices inv ON t.id = inv.tenant_id
GROUP BY
    e.id, t.id, c.id
ORDER BY
    c.fecha_vencimiento ASC
```

## Cache y Performance

```
Caché:
├── KPIs en línea: 5 min (bajo TTL, datos cambian)
├── Pipeline de renovaciones: 1 día (estable)
├── Matriz de clientes: 30 min (datos de contratos)
└── Gráficos históricos: 1 día (datos históricos)

Load: < 1 seg (con caché)
```

## Acciones Disponibles

| Acción | Requisito | Efecto |
|--------|-----------|--------|
| Renovar | Contrato a <60 días vencimiento | Abre modal de propuesta |
| Escalado | Contrato activo | Abre wizard de upgrade de plan |
| Contactar | Cliente activo | Abre formulario de email |
| Ver detalles | Cualquier cliente | Abre page de cliente |
| Exportar | Role: admin | Descarga Excel con datos |
| Alertar | Role: admin | Envía notificación al cliente |

## Exportación

```
Botón "Exportar"
    ↓
Descarga Excel con:
├─ Clientes activos
├─ Contratos vigentes
├─ Ingresos por plan
├─ KPIs de operación
└─ Fecha de generación
```

## Notificaciones en Tiempo Real

```
- Nuevo contrato activado: notificación visual + ding
- Cliente alcanza riesgo: notificación visual + email
- Factura pagada: notificación visual
- Escalado detectado: notificación visual + email
```

## Roles y Permisos

```
Sales Manager:
├── Ver todo ✓
├── Crear propuesta ✓
├── Contactar cliente ✓
└── Exportar ✓

Account Manager:
├── Ver asignados ✓
├── Crear propuesta ✓
└── Contactar cliente ✓

CFO:
├── Ver todo ✓
├── Análisis de revenue ✓
└── Ver proyecciones ✓

Admin:
├── Ver todo ✓
├── Todas las acciones ✓
└── Configurar dashboard ✓
```
