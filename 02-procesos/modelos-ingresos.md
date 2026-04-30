# Modelos de Revenue Share en JikkoOps

## Descripción General

JikkoOps soporta múltiples modelos de monetización para que cada contrato se ajuste a la realidad financiera y operativa de cada entidad.

**CRÍTICO**: Los cambios en cálculos de revenue share requieren validación y aprobación. No se permiten modificaciones sin revisión.

## Modelos Soportados

### 1. Por Expediente (Caute, Pague Después)

**Caso de uso**: Entidades nuevas que no pueden pagar upfront

```
Modelo: "CAUTE"
├── Período caute: Primeros 1000 expedientes (o X días) = $0
├── Límite: 1000 expedientes/mes
├── Activación: Cuando se supera límite
│   ├── Feature flags: [10% recaudo] activan
│   ├── Próxima factura: Incluye % de recaudo
│   └── Notificación al cliente: "Ha superado límite, nuevo modelo aplica"
└── Transición: Automática al superar límite
```

**Ejemplo**:
```
Mes 1 (350 expedientes):
└── Factura: $0

Mes 2 (1050 expedientes):
├── Primeros 1000: $0
├── Restantes 50: 50 × $X (precio unitario) = $XXX
└── Factura: $XXX
```

### 2. Por Porcentaje de Recaudo

**Caso de uso**: Municipios con capacidad de pago desde el inicio

```
Modelo: "PERCENTAGE_REVENUE"
├── Porcentaje: 10% (configurable)
├── Base: Total facturado/recaudado en el período
├── Cálculo: Recaudado × 10% = JikkoOps Revenue
├── Pago: Mensual (Mes N+1)
└── Validación: Auditoría independiente de recaudos
```

**Ejemplo**:
```
Mes 1:
├── Recaudos procesados: $1.000.000
├── % JikkoOps: 10%
└── Factura a Jikkosoft: $100.000

Mes 2:
├── Recaudos procesados: $1.500.000
├── % JikkoOps: 10%
└── Factura a Jikkosoft: $150.000
```

**Validación**: 
- ✓ Revisar estado de recaudos en CILIN
- ✓ Comparar con reportes de tesorería del municipio
- ✓ Auditar diferencias >5%

### 3. Por Cantidad de Usuarios

**Caso de uso**: Servicios de gestión documental, usuario/permisos centralizados

```
Modelo: "PER_USER"
├── Usuarios contratados: 50
├── Precio/usuario/mes: $XX
├── Cálculo: 50 × $XX = $XXX
├── Ajuste: Si excedens, pagar adicional al prorata
└── Reducción: Si cancelan usuarios, prorratear devolución
```

**Ejemplo**:
```
Contrato: 50 usuarios × $2000 = $100.000/mes

Mes 1 (50 activos): $100.000
Mes 2 (40 activos): $80.000
Mes 3 (55 activos): $110.000 (5 excedentes × $2000)
```

### 4. Por Expedientes Procesados

**Caso de uso**: Servicios de liquidación, CILIN integrado

```
Modelo: "PER_EXPEDIENT"
├── Precio/expediente: $XXX (fijo)
├── Cantidad: Expedientes procesados en período
├── Cálculo: Cantidad × Precio = Factura
├── Mínimo: Puede haber mínimo mensual
└── Máximo: Puede haber tope máximo/mes (rara vez)
```

**Ejemplo**:
```
Precio/expediente: $50

Mes 1 (200 expedientes): 200 × $50 = $10.000
Mes 2 (350 expedientes): 350 × $50 = $17.500
```

### 5. Modelos Híbridos

Combinación de los anteriores según la entidad.

#### Híbrido A: Caute + Recaudo

```
Modelo: "CAUTE_THEN_PERCENTAGE"
├── Fase 1 (Caute):
│   ├── Duración: 6 meses o 5000 expedientes (lo que primero ocurra)
│   ├── Costo: $0
│   └── Límite: 1000 expedientes/mes
│
└── Fase 2 (Revenue Share):
    ├── Después de Fase 1
    ├── Modelo: 10% de recaudo
    └── Mínimo: $50.000/mes (piso)
```

**Timeline**:
```
M1-M6: Caute ($0)
M7+: 10% recaudo (mínimo $50.000)
```

#### Híbrido B: Usuarios + Expedientes

```
Modelo: "USERS_AND_EXPEDIENTS"
├── Usuarios: 50 × $1000 = $50.000 (fijo)
├── Expedientes: > 500/mes → $100/expediente
│   └── Ej: 600 expedientes → 100 extra × $100 = $10.000
└── Total mes 1: $60.000
```

#### Híbrido C: Tiered (Escalonado)

```
Modelo: "TIERED"
├── Expedientes 0-500: $0
├── Expedientes 501-1000: $50/expediente
├── Expedientes 1001-5000: $30/expediente
└── Expedientes >5000: $20/expediente
```

**Ejemplo** (1200 expedientes):
```
Primeros 500: $0
Siguientes 500: 500 × $50 = $25.000
Siguientes 200: 200 × $30 = $6.000
Total: $31.000
```

## Decisión Comercial

### Qué Modelo Elegir

| Entidad | Situación | Modelo Recomendado |
|---------|-----------|-------------------|
| Municipio pequeño, bajo presupuesto | Primera vez | Caute + Porcentaje |
| Municipio grande, recaudo importante | Capacidad de pago | % Recaudo puro |
| Contratación de servicios (DOS, IAM) | Usuarios centralizados | Por Usuario |
| Liquidación integrada (CILIN) | Alto volumen expedientes | Por Expediente |
| Multi-servicio complejo | Mezcla de servicios | Híbrido personalizado |

### Plantilla de Propuesta

Toda propuesta debe especificar:

```markdown
## Modelo de Monetización

**Tipo**: [Caute | Porcentaje | Por Usuario | Por Expediente | Híbrido]

**Detalles**:
- Período caute (si aplica): X meses o Y expedientes
- Precio/unidad: $XXX
- Mínimo mensual: $XXX (si aplica)
- Máximo mensual: $XXX (si aplica)
- Escalados: Si supera X, se aplica Y

**Ejemplo de facturación**:
- Escenario 1 (uso bajo): $XXX
- Escenario 2 (uso esperado): $XXX
- Escenario 3 (uso alto): $XXX

**Validación**: 
- ✓ Manager de producto
- ✓ CFO / Finanzas
- ✓ Legal (si es modelo nuevo)
```

## Cálculo y Facturación

### Flujo de Facturación Mensual

```
1. Recolectar datos del período (M-1)
   ├── Expedientes procesados (de CILIN)
   ├── Usuarios activos (de IAM)
   ├── Recaudos procesados (de Sistema de Pagos)
   └── Feature flags activados

2. Aplicar modelo de pricing
   ├── Calcular cantidad/base de facturación
   ├── Aplicar descuentos/escalados si aplican
   ├── Ajustar por mínimos/máximos
   └── Generar líneas de factura

3. Generar factura
   ├── Documento XML/PDF
   ├── Líneas detalladas
   ├── Términos de pago
   └── Fecha de vencimiento

4. Enviar a CILIN
   ├── Para generación de cobro coactivo
   ├── Para registro contable
   └── Para auditoría

5. Enviar al cliente
   ├── Email con factura
   ├── Portal de facturas
   └── Período de reclamación: 10 días
```

### Auditoría y Validación

**Mensual** (antes de generar factura):

```
Validación Modelo "Caute + %":
├── Expedientes: Comparar BD con reporte CILIN
├── Recaudos: Comparar con Sistema de Pagos
├── Limite caute (1000): ¿Se superó? → Activar % recaudo
├── Cálculo: Verificar fórmula aplicada correctamente
└── Comparación: vs. mes anterior (verificar anomalías >10%)
```

**Trimestral**: Auditoría independiente de recaudos vs. facturas

**Anual**: Revisión completa de contabilidad y acuerdos

## Excepciones y Ajustes

### Ajustes Permitidos
- ✓ Descuento por volumen (5-10%)
- ✓ Período extendido de caute (por acuerdo comercial)
- ✓ Promoción temporal (máx 2 meses)

### Ajustes NO Permitidos (requieren aprobación)
- ✗ Cambio de modelo sin notificación
- ✗ Descuentos >15%
- ✗ Cambios retroactivos
- ✗ Eliminación de líneas de factura

**Proceso de aprobación**:
1. Manager propone ajuste + justificación
2. CFO valida impacto financiero
3. Legal revisa si afecta contrato
4. CEO aprueba si impacto > 5% valor anual
5. Notificar al cliente
6. Documentar en audit log

## Reporte de Revenue por Modelo

**Dashboard G-COPS**:

```
Revenue por Modelo (últimos 12 meses):
├── Caute: $500.000 (10 clientes)
├── Porcentaje: $2.500.000 (8 clientes)
├── Por Usuario: $300.000 (4 clientes)
├── Por Expediente: $1.200.000 (6 clientes)
└── Híbrido: $400.000 (3 clientes)

Total: $4.900.000

Tasa de escalado (M/M): +15%
Clientes en riesgo (> 30 días vencido): 2
Próximas renovaciones: 5 (en próximos 90 días)
```

## Próximos Pasos

1. Documentar matriz de modelos soportados
2. Crear templates de propuesta por modelo
3. Automatizar cálculos en sistema
4. Entrenar equipo comercial en modelos
5. Implementar validaciones en código

---

**Versión**: 1.0 | **Actualizado**: 2026-04-30
