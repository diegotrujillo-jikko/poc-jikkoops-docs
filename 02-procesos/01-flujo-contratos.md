# Flujo de Contratos en JikkoOps

## Descripción General

El flujo de contratos en JikkoOps define cómo una entidad (municipio, gobernación, DIAN) se convierte en cliente y cómo se activan sus servicios de forma ordenada y auditable.

## Etapas del Proceso

```
1. Prospección (CRM)
   ↓
2. Propuesta Comercial
   ↓
3. Firma de Contrato
   ↓
4. Activación en JikkoOps
   ↓
5. Operación y Facturación
   ↓
6. Renovación / Ajustes
```

## 1. Prospección (CRM)

**Responsable**: Equipo Comercial  
**Sistema**: G-COPS (Revenue Contract Orchestrator) - módulo CRM

### Actividades

- Crear **Oportunidad de Negocio** con:
  - Entidad (nombre, ubicación, sector)
  - Necesidades identificadas
  - Presupuesto estimado
  - Timeline
  - Contactos (decisores, influenciadores)

- Ingresar **Leads**: Información de contactos, decisores
- Seguimiento: Visitas, llamadas, ajustes de propuesta

## 2. Propuesta Comercial

**Responsable**: Equipo de Producto/Comercial  
**Sistema**: G-COPS - módulo Propuestas

### Elementos de la Propuesta

Basada en los **Productos** definidos (combinaciones de funcionalidades):

```
Propuesta-2026-MUN-CALI:
├── Servicios Incluidos
│   ├── SILIN (Liquidación)
│   │   └── Funcionalidades: Liquidación básica, Reportes, Ajustes
│   ├── DOS (Documentos)
│   │   └── Funcionalidades: Gestión básica, TRD, Firma digital
│   └── SOCIA (Servicio al Ciudadano)
│       └── Funcionalidades: Notificaciones, Seguimiento
│
├── Modelo de Pricing
│   ├── Cantidad de usuarios: 50
│   ├── Cantidad de expedientes: 1000/mes (modelo-ingresos, pague después)
│   ├── Revenue share: 10% de recaudo (después de 1000 expedientes)
│   ├── Período: 12 meses
│   └── Valor total: $XXX.XXX COP
│
└── Términos y Condiciones
    ├── SLA de disponibilidad
    ├── Horario de soporte
    ├── Cláusulas de escalado de volumen
    └── Opciones de renovación
```

### Aprobación de Propuesta

- Manager interno valida modelo de pricing
- Propuesta enviada a entidad
- Entidad revisa y solicita ajustes (iteración)
- Aprobación final

## 3. Firma de Contrato

**Responsable**: Legal/Comercial  
**Sistema**: Sistema externo de firma digital

### Documentos

- Contrato marco
- Anexo de servicios y precios
- Acuerdos de SLA
- Política de datos

### Registro en JikkoOps

Una vez firmado, se crea **Contrato** en JikkoOps:

```
Contrato-2026-MUN-CALI:
├── Datos Generales
│   ├── Entidad: Municipio de Cali
│   ├── Tipo: Principal / Renovación / Enmienda
│   ├── Fecha firma: YYYY-MM-DD
│   ├── Vigencia: 12 meses
│   └── Estado: Activo / Cancelado / En revisión
│
├── Servicios Incluidos
│   ├── Plan: "Plan Estándar + Módulo DOS"
│   ├── Período: YYYY-MM-DD a YYYY-MM-DD
│   └── Funcionalidades: [LIQ-*, DOC-*, SOC-*]
│
├── Términos Financieros
│   ├── Modelo: Por expediente + % recaudo
│   ├── Valor fijo: $0 (período modelo-ingresos)
│   ├── Variables: 10% de cada expediente procesado
│   └── Forma de pago: [Transferencia, TDC, Débito automático]
│
└── Documentos
    ├── Contrato escaneado (PDF)
    ├── Anexos
    └── Addendums
```

## 4. Activación en JikkoOps

**Responsable**: Manager de JikkoOps / Operaciones  
**Sistema**: G-COPS - módulo Contratos + Configuración

### Proceso de Activación

#### 4.1 Crear Cliente
```
Cliente:
├── Razón Social: Municipio de Cali
├── NIT: XXXX
├── Email contacto: admin@cali.gov.co
├── Teléfono: +57...
├── Dirección: Calle X, No Y
└── Contactos por rol: [Admin, Operador, Reportes]
```

#### 4.2 Configurar Tenant
```
Tenant (cali-2026):
├── Nombre: municipio_cali
├── Región: Colombia - Valle del Cauca
├── Plan: Plan Estándar + DOS
├── Usuarios permitidos: 50
├── Expedientes/mes: 1000 (modelo-ingresos)
├── Datos de facturación: [Dirección, RUT, Email]
└── Configuración técnica: [BD, API keys, Webhooks]
```

#### 4.3 Activar Protected Resources
```
Feature Flags:
├── LIQ-001 (Liquidar): ON ✓
├── LIQ-004 (Dashboard): ON ✓
├── DOC-001 (Crear doc): ON ✓
├── SOC-001 (Notif manual): OFF ✗ (requiere upgrade)
└── [Todos los de Plan Estándar + DOS]: ON/OFF según plan
```

#### 4.4 Configurar Integraciones
- Webhook para notificaciones
- Sincronización de datos con SIGIA
- Credenciales de API
- Certificados SSL/TLS

#### 4.5 Capacitación y Go-Live
- Entrega de manuales
- Sesión de capacitación
- Período de prueba
- Confirmación de go-live

## 5. Operación y Facturación

**Responsable**: Equipo de Operaciones / Finanzas  
**Sistema**: G-COPS + SILIN (facturación integrada)

### Monitoreo

```
KPIs por contrato:
├── Usuarios activos / límite
├── Expedientes procesados / mes
├── Cumplimiento de SLA
├── Incidentes / tickets abiertos
└── NPS / Satisfacción
```

### Facturación

**Mensual o según modelo**:

```
Factura (MES-YYYY-CALI):
├── Período: YYYY-MM-01 a YYYY-MM-30
├── Cantidad de expedientes: 450
├── Costo unitario (modelo-ingresos): $0
├── Total variable (10%): $X.XXX
├── Subtotal: $X.XXX
├── IVA: $X.XXX
└── Total: $X.XXX

Envío a SILIN para:
└── Generación de cobro coactivo si es necesario
```

### Escalados de Volumen

Si el cliente supera límites:

```
Caso: Cali supera 1000 expedientes/mes (límite modelo-ingresos)

Acción automática:
1. Sistema notifica a Manager
2. Se activa feature flag de 10% recaudo (SOC-002, SOC-003)
3. Próxima factura incluye % de recaudo
4. Se avisa al cliente
5. Opcionalmente: upgrade a plan superior
```

## 6. Renovación / Ajustes

**Responsable**: Comercial / Operaciones  
**Sistema**: G-COPS

### Renovación

- 60 días antes de vencimiento: Recordatorio a cliente
- Manager propone renovación o cambio de plan
- Firma de nueva propuesta/contrato
- Actualizar datos en JikkoOps

### Ajustes

En cualquier momento, el cliente puede:
- Solicitar más usuarios (sin costo o con ajuste)
- Solicitar módulos adicionales
- Cambiar modelo de pricing
- Escalado temporal para campaña especial

**Proceso**: Propuesta → Validación Manager → Actualizar Contrato → Aplicar feature flags

## Estados de Contrato

```
Borrador → Propuesta → En revisión → Activo → Cancelado
                                       ↑
                                    Renewing
                                       ↑
                                    Amended
```

## Seguridad

**Safe Change Rule**: 
- ✗ NO permitir cambios en modelo de pricing/revenue share sin aprobación
- ✓ SÍ permitir cambios en funcionalidades activadas
- ✗ NO permitir cambios en términos de facturación sin auditoría

Todos los cambios requieren:
- Justificación
- Aprobación de Manager
- Registro en audit log
- Notificación al cliente

## Integraciones Externas

JikkoOps debe integrarse con:
- **SIGIA**: Para sincronización de usuarios y expedientes
- **SILIN**: Para facturación y cobro
- **Sistema de Pagos**: Para recaudos
- **CRM Externo**: Si el cliente lo requiere

---

**Versión**: 1.0 | **Actualizado**: 2026-04-30
