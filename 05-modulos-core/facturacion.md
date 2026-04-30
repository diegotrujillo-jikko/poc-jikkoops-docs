# Módulo de Facturación

## Propósito

Generar, revisar, enviar y contabilizar facturas según modelo de revenue de cada contrato.

## Flujo de Facturación

```
Recolectar datos del período
    ↓
Calcular según modelo de revenue
    ↓
Validar no haya errores
    ↓
Generar documento PDF
    ↓
Revisar antes de enviar
    ↓
Enviar a cliente
    ↓
Registrar en contabilidad (SILIN)
    ↓
Cobro (si aplica)
```

## 1. Generación Automática

**Scheduled Job** (mensual, 1er día):

```
Para cada tenant activo:
1. Leer período (mes anterior)
2. Recolectar datos:
   ├─ Expedientes procesados (desde SILIN)
   ├─ Usuarios activos (desde IAM)
   ├─ Recaudos procesados (desde Pagos)
   └─ Vigencia de contrato (validar no vencido)
3. Validar integridad de datos
4. Calcular según modelo:
   ├─ Si CAUTE: 0
   ├─ Si PERCENTAGE: expedientes × %
   ├─ Si PER_USER: usuarios × precio
   ├─ Si PER_EXPEDIENT: expedientes × precio
   └─ Si HÍBRIDO: suma de aplicables
5. Aplicar mínimos y máximos
6. Generar factura (estado: BORRADOR)
7. Enviar a revisión (si está configurado)
8. Una vez aprobada: Emitir
9. Enviar a cliente
10. Registrar en SILIN para cobro
```

**Validaciones críticas**:
```
✓ Contrato activo en el período
✓ Datos de facturación completos
✓ Modelo de revenue válido
✓ Sin cambios retroactivos en período
✓ Comparación con mes anterior (alerta si >50% diferencia)
```

## 2. Vista de Facturas (Admin)

**Listar facturas**:
```
Filtros:
├── Tenant / Cliente
├── Estado: [Borrador, Emitida, Pagada, Vencida, Anulada]
├── Período: Fecha desde/hasta
├── Rango de monto
└── Forma de pago

Tabla:
| Número       | Cliente    | Período      | Total    | Estado   | Acción     |
|--------------|------------|--------------|----------|----------|------------|
| FACT-2026-05 | CALI       | 2026-04-01   | $110,900 | Emitida  | Ver        |
| FACT-2026-04 | PEREIRA    | 2026-03-01   | $25,000  | Pagada   | Descarga   |
| FACT-2026-03 | MANIZALES  | 2026-02-01   | $150,000 | Vencida  | Cobro coac |

Acciones por estado:
├─ Borrador: Editar, Emitir, Anular, Resincronizar datos
├─ Emitida: Enviar email, Enviar WhatsApp, Cambiar fecha vencimiento
├─ Vencida: Generar Cobro coactivo, Descuento, Renegociar
└─ Pagada: Descargar PDF, Ver comprobante de pago
```

**Resincronizar datos de borrador**:
```
Caso: Manager nota que factura en borrador tiene datos incorrectos

Click "Resincronizar"
    ↓
Sistema:
├─ Recolecta datos nuevamente de período
├─ Recalcula según modelo revenue
├─ Actualiza líneas de factura
└─ Mantiene estado: Borrador
    ↓
Manager puede revisar cambios y emitir
```

## 3. Detalle de Factura

```
Factura: FACT-2026-05-CALI
═════════════════════════════════════════

Cliente: Municipio de Cali
NIT: 123.456.789-0
Dirección: Calle X, No Y
Email: tesoreria@cali.gov.co

Período: 2026-04-01 a 2026-04-30

LÍNEAS DE FACTURACIÓN:
────────────────────────────────────
Descripción                Cantidad   Precio Unit  Subtotal
Expedientes procesados        450        $200      $90,000
  (Modelo: Por Expediente, modelo-ingresos)

─────────────────────────────────────
Subtotal:                                           $90,000
Ajuste (descuento/recargo):                        +$5,000
Subtotal ajustado:                                 $95,000
IVA (19%):                                         $18,050
────────────────────────────────────
TOTAL:                                            $113,050

TÉRMINOS DE PAGO:
────────────────────────────────────
Fecha emisión: 2026-05-01
Fecha vencimiento: 2026-05-15
Forma de pago: Transferencia bancaria
Cuenta: [Datos bancarios]

NOTAS:
────────────────────────────────────
- Próximo escalado si excede 1000 expedientes: 10% del recaudo
- Cambios de modelo de pricing requieren enmienda de contrato
```

**Editar líneas (modo borrador)**:
```
Si hay error en cálculo, manager puede:

Línea: "Expedientes procesados"
├─ Cantidad: 450 → Cambiar a 480
├─ Precio: $200 → Cambiar a $195
├─ Subtotal: Recalcula a $93,600
├─ Razon: "Validación manual: conteo fue 480 no 450"
└─ Guardar

Sistema:
├─ Actualiza total de factura
├─ Registra cambio en audit log
├─ Mantiene estado: Borrador
└─ Requiere re-aprobación si está configurado
```

## 4. Emisión de Factura

**De Borrador a Emitida**:
```
Click "Emitir factura"
    ↓
Validaciones finales:
├─ ✓ Todos los campos obligatorios completos
├─ ✓ Total > $0
├─ ✓ Datos del cliente válidos
├─ ✓ Contrato activo en el período
│
└─ Si todo OK:
   ├─ Generar número secuencial (FACT-YYYY-MM-CLIENTE)
   ├─ Generar documento PDF
   ├─ Cambiar estado: Emitida
   ├─ Emitir evento: invoice.created
   └─ Enviar email a cliente automáticamente
       Asunto: "Factura de servicios JikkoOps"
       Adjunto: PDF
```

## 5. Cobro de Facturas

**Integración con SILIN**:
```
Cuando factura se emite:
    ├─→ SILIN: Registra cobro
    ├─→ Sistema de Pagos: Crea recibo de pago
    └─→ Contabilidad: Registra ingreso

Formas de pago soportadas:
├── Transferencia bancaria (manual)
├── Tarjeta de crédito (procesador)
├── Débito automático (ACH)
└── Efectivo (manual)

Proceso de cobro:
1. Cliente realiza pago (cualquier forma)
2. Sistema detecta pago (automático o manual)
3. Registra fecha y referencia de transacción
4. Cambia estado factura: Pagada
5. Emite evento: invoice.paid
6. Envía recibido al cliente
7. Actualiza balance de cliente
```

**Factura vencida**:
```
Si fecha_vencimiento <= hoy y estado != Pagada:
    ├─ Estado: Vencida
    ├─ Alerta: Enviada a manager
    └─ Opciones:
       ├─ Enviar recordatorio (email automático)
       ├─ Generar Cobro coactivo (si aplica)
       ├─ Ofrecer descuento (requiere aprobación)
       └─ Renegociar condiciones
```

## 6. Anulación de Factura

**Caso: Facturaerror después de emitida**:
```
Click "Anular"
    ↓
Razón: [Desplegable]
├── Emitida por error
├── Corrección de datos
├── Solicitud del cliente
└── Otro: [texto libre]
    ↓
Confirmación: "¿Anular factura? No se puede deshacer"
    ↓
Si confirma:
├─ Estado: Anulada
├─ Generar factura de ajuste (si necesario)
├─ Registrar en auditoría
└─ Emitir evento: invoice.voided
```

## 7. Reportes

```
Reporte de Facturación (mensual):
├── Total emitido: $X
├── Total pagado: $Y
├── Vencidas sin pagar: $Z
├── Tasa de cobranza: X%
├── Promedio de días para pago: N
├── Por modelo revenue:
│   ├─ Caute: $X (Y%)
│   ├─ Porcentaje: $X (Y%)
│   └─ ...
└── Por cliente top 10

Exportar: Excel, PDF
```

## 8. Auditoría de Facturación

```
Cambios auditados:
├─ Creación de factura (datos iniciales)
├─ Edición de líneas (borrador)
├─ Emisión de factura (cambio de estado)
├─ Envío a cliente (timestamp)
├─ Registro de pago
├─ Anulación (razón)
└─ Cualquier otra modificación

Cada cambio registra:
├─ Usuario que lo hizo
├─ Timestamp
├─ Cambio específico (before/after)
└─ Razon (si aplica)
```

## 9. Permisos

```
Accountant:
├── Ver facturas ✓
├── Emitir facturas ✓
├── Editar en borrador ✓
├── Registrar pagos ✓
└── Exportar reportes ✓

CFO:
├── Ver todo ✓
├── Anular facturas ✓
├── Descontos/recargas ✓
└── Análisis de cobranza ✓

Admin:
├── Todo ✓
```

## Integración con Otros Sistemas

```
JikkoOps Facturación
        ├─→ SILIN: Registra cobro
        ├─→ Sistema de Pagos: Procesa pago
        ├─→ Email: Envía factura al cliente
        ├─→ Contabilidad ERP: Registra ingreso
        └─→ Auditoría: Log de cambios
```
