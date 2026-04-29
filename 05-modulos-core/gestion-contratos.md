# Gestión de Contratos

## Propósito

Módulo para crear, revisar, activar y monitorear contratos de servicio con clientes.

## Funcionalidades

### 1. Crear Contrato

**Flujo**:
```
Nuevo Contrato
    ↓
Seleccionar cliente/tenant
    ↓
Elegir plan y servicios
    ↓
Configurar modelo de revenue
    ↓
Cargar documento PDF
    ↓
Revisar antes de enviar
    ↓
Estado: Borrador
```

**Campos**:
```
Básicos:
├── Cliente (búsqueda)
├── Número de contrato (auto-generado)
├── Tipo: Principal / Renovación / Enmienda
└── Período: fecha inicio y fin

Servicios:
├── Plan (dropdown)
├── Módulos adicionales (checkboxes)
├── Funcionalidades específicas
└── Protected Resources (calculado)

Financiero:
├── Modelo de revenue (selector)
├── Valores según modelo (dinámicos)
├── Forma de pago
├── Términos de pago
└── Cláusulas especiales

Documentación:
├── PDF del contrato
├── Anexos (múltiples)
└── Notas internas
```

### 2. Revisar Contrato

**Workflow de aprobación**:
```
Borrador
    ↓
Click "Enviar a revisión"
    ↓
Estado: En revisión
    ├── CFO: Valida términos financieros
    ├── Legal: Valida términos legales
    └── Product Manager: Valida servicios
    ↓
Si todas aprueban → Estado: Listo para firma
Si alguien rechaza → Vuelve a Borrador con comentarios
```

**Vista de revisor**:
```
Contrato: CONTRATO-2026-CALI-002
Cliente: Municipio de Cali
Revisores:
├─ CFO: (avatar) "Revisando..."
├─ Legal: (avatar) "Aprobado ✓"
└─ Product: (avatar) (no revisó)

Cambios desde última revisión:
├─ Modelo revenue: CAUTE → CAUTE_THEN_PERCENTAGE
├─ Porcentaje recaudo: N/A → 10%
└─ Mínimo mensual: N/A → $50.000

Comentarios:
├─ CFO (1h): "Validar que 10% sea competitivo"
└─ Legal (2h): "Términos OK, pendiente firma de contador"
```

### 3. Activar Contrato

**Pre-requisitos**:
```
✓ Estado: Firmado o Listo
✓ Fecha de inicio <= hoy
✓ Todas las revisiones completadas
✓ Documento PDF adjunto
```

**Activación**:
```
Click "Activar"
    ↓
Sistema:
├─ Crea tenant en BD
├─ Activa feature flags
├─ Emite evento contract.activated
├─ Envía email de bienvenida al cliente
└─ Abre espacios en módulos (CILIN, DOS, SOCIA)
    ↓
Estado: Activo
```

### 4. Monitorear Contratos Activos

**Vista de monitoreo**:
```
Tabla de contratos activos:

| Cliente    | Plan      | Inicio    | Vencimiento | Estado    | Días |
|------------|-----------|-----------|-------------|-----------|------|
| CALI       | Estándar  | 2026-01-15| 2027-01-15  | Activo    | 256  |
| PEREIRA    | Básico    | 2026-03-01| 2026-12-31  | Activo    | 240  |
| MANIZALES  | Premium   | 2026-02-01| 2027-02-01  | Activo    | 305  |

Filtros:
├── Por estado: [Activo, Vencido, Cancelado]
├── Por plan
├── Vencimiento en próximos: X días
└── Por ingresos

Acciones:
├── Ver detalles
├── Renovar (si vence <90 días)
├── Cancelar (con confirmación)
└── Descargar PDF
```

## Notificaciones de Contrato

```
30 días antes de vencimiento:
├─ Email a manager: "Contrato CALI vence en 30 días"
├─ Email a cliente: "Su contrato vence en 30 días, contacte gerencia"
└─ Dashboard: Marca contrato en naranja

7 días antes de vencimiento:
├─ Email a manager: "URGENTE: Contrato CALI vence en 7 días"
└─ Dashboard: Marca contrato en rojo

Día del vencimiento:
├─ Sistema: Bloquea acceso si no se renueva
├─ Email a cliente: "Su contrato venció, servicio suspendido"
└─ Dashboard: Marca como Vencido
```

## Escalados (Enmiendas)

**Caso**: Cliente solicita módulo adicional en mitad del contrato

```
Crear Enmienda
    ↓
Seleccionar contrato base
    ↓
Modificar servicios/plan
    ↓
Sistema calcula ajuste de precio (prorrateo)
    ↓
Enviar para revisión y firma
    ↓
Una vez firmada, activar enmienda
    ↓
Actualizar feature flags y contrato
    ↓
Ajustar facturación próximo mes
```

## Integración con Otros Módulos

```
Contrato Activo
    ├─→ CILIN: Inicializa liquidación del cliente
    ├─→ DOS: Crea espacios documentales
    ├─→ SOCIA: Crea usuarios iniciales
    ├─→ Facturación: Comienza a facturar según modelo
    └─→ Feature Flags: Activa recursos según plan
```

## Reportes

```
Reporte de Contratos:
├── Resumen ejecutivo
│   ├─ Total activos: X
│   ├─ Ingresos mensuales: $Y
│   └─ Próximas renovaciones: Z
├── Contratos por estado
├── Contratos por plan
├── Proyección de vencimientos
└── Histórico de cambios

Exportar: Excel / PDF
```

## Auditoría

```
Cambios auditados:
├── Creación de contrato
├── Cambios en servicios/precios
├── Activación
├── Cambios de estado
├── Cancelación
└── Quién hizo qué y cuándo

Registro completo en audit_log con:
├─ Usuario que hizo cambio
├─ Timestamp
├─ Cambio específico (before/after)
└─ Razon (si aplica)
```

## Permisos

```
Sales Manager:
├── Crear contrato ✓
├── Enviar a revisión ✓
└── Ver todos ✓

Legal:
├── Revisar contratos ✓
├── Solicitar cambios ✓
└── Aprobar/rechazar ✓

CFO:
├── Revisar términos financieros ✓
├── Solicitar cambios ✓
└── Aprobar/rechazar ✓

Admin:
├── Todo ✓
```
