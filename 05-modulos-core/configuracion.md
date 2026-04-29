# Módulo de Configuración

## Propósito

Configurar parámetros de JikkoOps a nivel global, por producto, plan y tenant.

## Secciones

### 1. Configuración Global (Admin)

**Parámetros del sistema**:
```
Empresa:
├── Nombre legal
├── NIT
├── Email de contacto
├── Logo/Branding
└── Dirección

Sistema:
├── Ambiente: [Desarrollo | Producción | Staging]
├── URL base de API
├── Timezone
└── Idiomas soportados: [Español, English]

Integración:
├── CILIN
│  ├─ URL API
│  ├─ API Key
│  └─ Secret
├── DOS
├── SOCIA
└── Sistema de Pagos

Email:
├── SMTP Server
├── Sender email
├── Plantillas de email (editable)
└── Test de conexión

Auditoría:
├── Retención de logs (años)
├── Eventos para registrar
└── Encriptación de datos sensibles
```

### 2. Configuración de Productos

**Para cada producto**:
```
Producto: "Liquidación Estándar"

Básico:
├── Nombre
├── Descripción
├── Funcionalidades incluidas (checkboxes)
├── Protected Resources (calculado)
├── Estado: [Activo | Deprecated]
└── Fecha de creación

Versioning:
├── Versión actual: 2.1
├── Cambios desde última versión
├── Historial de versiones
└── Notas de compatibilidad

Descripción para clientes:
├── Pitch corto (marketing)
├── Funcionalidades detalladas
├── Límites y restricciones
└── Precondiciones de uso
```

**Editar funcionalidades del producto**:
```
Producto: Liquidación Estándar
Funcionalidades actuales:
├─ Liquidación básica
├─ Reportes estándar
├─ Auditoría

Agregar funcionalidad:
    ├─ Buscar funcionalidad disponible
    ├─ Seleccionar
    └─ Guardar
        ↓
    Sistema:
    ├─ Agrega recursos automáticamente
    ├─ Recalcula qué planes usan este producto
    └─ Valida no haya conflictos
```

### 3. Configuración de Plans

**Para cada plan**:
```
Plan: "Plan Estándar"

Identidad:
├── Nombre
├── Descripción
├── Producto padre
├── Icono
├── Color (para UI)
└── Estado: [Activo | Deprecated]

Funcionalidades:
├── Agregar/quitar funcionalidades
├── Reordenar (para mostrar en UI)
└── Total de recursos: X

Límites:
├── Usuarios máximo: 50 (NULL = ilimitado)
├── Expedientes/mes: 1000 (NULL = ilimitado)
├── Espacio en BD: 10 GB (NULL = ilimitado)
└── Features especiales:
   ├─ Reportes personalizados: Sí/No
   ├─ Integración custom: Sí/No
   └─ Soporte 24/7: Sí/No

Pricing:
├── Tipo: [Caute | Porcentaje | Por Usuario | Por Expediente | Híbrido]
├── Valores según tipo (dinámicos)
├── Mínimo mensual: $X
├── Máximo mensual: $X (si aplica)
└── Moneda: COP

Marketing:
├── Descripción breve (pitch)
├── Principales beneficios (top 5)
└── Casos de uso ideales
```

**Cambios en plans**:
```
Cambio: Aumentar límite de usuarios de 50 a 100

Sistema valida:
├─ ¿Tenants activos en este plan? Sí, 7 tenants
├─ Impacto: 7 clientes tendrán new benefit automáticamente
├─ ¿Cambiar precio? [Sí | No]
│  └─ Si Sí: Nuevo precio se aplica a próximas renovaciones
│
Cambio registrado en:
├─ Audit log
├─ Changelog de plan
└─ Notificación a manager comercial
```

### 4. Configuración de Protected Resources

**Panel de inventario de recursos**:
```
Total de recursos: 42 (39 asignados + 3 huérfanos)

Recursos Huérfanos (sin funcionalidad):
├─ DOC-005: Búsqueda avanzada
├─ SOC-004: Nuevo tipo de notificación
└─ LIQ-007: Exportación a formato XML

Acción: Asignar a funcionalidad
    ├─ Seleccionar recurso
    ├─ Búscar funcionalidad
    ├─ Asignar
    └─ Confirmar impacto en plans/tenants
```

**Editar recurso**:
```
Recurso: LIQ-001 (Botón Liquidar)

Información:
├── Código: LIQ-001
├── Nombre: Botón Liquidar
├── Tipo: button
├── Módulo: CILIN
├── Criticidad: crítica
└── Estado: activo

Descripción:
├── Qué es: Permite al usuario ejecutar liquidación
├── Cuándo se usa: En dashboard de liquidación
├── Requisitos: Usuario admin o operador con permiso
└── Impacto si falta: Imposible liquidar

Dependencias:
├── Requiere: [LIQ-002, API-Liquidación]
└── Usado por: [Feature: Liquidación, Plan: Básico/Estándar/Premium]

Cambios recientes:
├── 2026-04-01: Actualizado descripción
├── 2026-03-15: Cambio de tipo: action → button
└── ... (historial)
```

### 5. Configuración de Revenue Models

**Definir modelos soportados**:
```
Modelos activos:
├─ CAUTE
├─ PERCENTAGE_REVENUE
├─ PER_USER
├─ PER_EXPEDIENT
└─ CAUTE_THEN_PERCENTAGE

Para cada modelo:
├── Descripción
├── Parámetros configurables
├── Fórmula de cálculo
├── Ejemplos (3 escenarios)
└── Validaciones (qué limites aplican)
```

**Agregar modelo personalizado** (rara vez):
```
Nuevo Modelo: "TIERED_PERCENTAGE"

Fórmula:
├─ Expedientes 1-1000: 5% recaudo
├─ Expedientes 1001-5000: 7% recaudo
├─ Expedientes >5000: 10% recaudo

Parámetros:
├─ Nombre: TIERED_PERCENTAGE
├─ Tiers: [
│    {"rango_min": 0, "rango_max": 1000, "porcentaje": 0.05},
│    {"rango_min": 1001, "rango_max": 5000, "porcentaje": 0.07},
│    {"rango_min": 5001, "rango_max": null, "porcentaje": 0.10}
│  ]
├─ Mínimo mensual: $0
└─ Máximo mensual: null

Implementación:
├─ Actualizar fórmula en código
├─ Validar en pruebas
├─ Disponibilizar en propuestas comerciales
└─ Documentar para equipo
```

### 6. Configuración de Módulos (CILIN, DOS, SOCIA)

**Para cada módulo integrado**:
```
Módulo: CILIN (Liquidación)

Integración:
├── URL API: https://cilin.internal/api/v1
├── API Key: [encriptado]
├── Secret: [encriptado]
├── Timeout: 30 segundos
└── Reintentos: 3

Sincronización:
├── Cada: 15 minutos (configurable)
├── Datos a sincronizar:
│  ├─ Expedientes procesados
│  ├─ Recaudos
│  └─ Estados de liquidación
└─ Validar integridad: Sí

Webhooks:
├── Eventos que CILIN envía: [liquidacion_completada, error_liquidacion]
├── Endpoint para recibir: /webhooks/cilin
├── Secret para validar: [encriptado]
└── Test de webhook: [botón]

Health Check:
├── Verificar cada: 5 minutos
├── Marcar como DOWN si no responde: 2 intentos
└── Alertas: Sí (a manager operación)
```

### 7. Plantillas de Email

**Editar plantillas de comunicación**:
```
Plantilla: "Bienvenida a JikkoOps"

Asunto: Personalizable
├── [Cliente]: Municipio de Cali
├── [Plan]: Plan Estándar
└── [Fecha]: 2026-05-01

Cuerpo:
```
Estimado [ContactoNombre],

Nos complace informar que su contrato ha sido activado exitosamente.

Servicios incluidos:
[FuncionalidadesList]

Acceso:
URL: [DashboardURL]
Email: [ContactoEmail]
Teléfono: [SoporteTeléfono]

Próximos pasos:
1. Acceder al sistema con credenciales enviadas
2. Crear usuarios adicionales si es necesario
3. Contactar soporte si tiene dudas

¡Bienvenido a JikkoOps!
```

Campos disponibles:
├── [Cliente], [ContactoNombre], [Plan]
├── [FuncionalidadesList], [DashboardURL]
├── [SoporteTeléfono], etc.
└── Preview en tiempo real
```

### 8. Configuración de Alertas y Notificaciones

**Qué alertas enviar y a quién**:
```
Alerta: "Contrato próximo a vencer"

Configuración:
├── Habilitada: Sí
├── Días antes: [30, 15, 7] (checkboxes)
├── Destinatarios: [Manager Comercial, Cliente]
├── Canales: [Email, SMS, Dashboard]
├── Plantilla email: "Recordatorio vencimiento"
└── Frecuencia máx: 1x/día

Alerta: "Factura vencida sin pago"

Configuración:
├── Habilitada: Sí
├── Días vencida: 0, 7, 15, 30
├── Destinatarios: [Cliente, CFO]
├── Canales: [Email]
├── Escalar a cobro coactivo después: 30 días
└── Plantilla email: "Recordatorio factura vencida"
```

### 9. Roles y Permisos

**Definir roles y qué pueden hacer**:
```
Rol: "Operations Manager"

Permisos:
├─ Ver tenants activos ✓
├─ Cambiar feature flags ✓
├─ Gestionar usuarios de tenant ✓
├─ Resincronizar datos ✓
├─ Resolver alertas ✓
├─ Crear contratos ✗
├─ Emitir facturas ✗
└─ Acceso a reportes financieros ✗

Módulos acceso:
├─ Dashboard Operación: Lectura + Escritura
├─ Gestión de Contratos: Lectura
├─ Facturas: Lectura
└─ Configuración: No
```

### 10. Backup y Recuperación

**Configuración de backups**:
```
Backup automático:
├── Frecuencia: Diario (00:00 UTC)
├── Retención: Últimos 30 backups + archivos mensual
├── Replicación: 3 copias (HA)
├── Ubicación: [Cloud storage principal + secondary]
└── Encriptación: AES-256

Test de restore:
├── Automático: Semanalmente
├── Validar: Integridad de BD
├── Reporte: Log de test
└── Alertas: Si test falla
```

## Auditoría de Cambios de Configuración

Todos los cambios en configuración se registran:

```
Cambio: Aumentar límite de usuarios en Plan Básico
├── Usuario: Diego Trujillo
├── Timestamp: 2026-04-29 10:30:00
├── Cambio: usuarios_limite [50 → 100]
├── Impacto: 5 tenants activos
├── Aprobado: Sí (automático)
└── Motivo: Solicitud comercial para CALI
```

## Permisos

```
Admin Global:
├── Todo ✓

Product Manager:
├── Configurar productos ✓
├── Configurar plans ✓
├── Editar Protected Resources ✓
├── Configurar modelos revenue ✓
└── Configuración global ✗

Operations Manager:
├── Configurar alertas ✓
├── Ver backup status ✓
└── Modificar otra config ✗
```
