# Módulo de Operación

## Propósito

Monitorear la salud operacional de tenants activos, gestionar feature flags, usuario y detectar/resolver problemas.

## Secciones Principales

### 1. Health Check por Tenant

**Dashboard de estado**:
```
Municipio de Cali (municipio_cali):
├── Plan: Estándar
├── Estado: ACTIVO ✓
├── Última actividad: hace 2 horas
├── Usuarios activos: 42/50 (84%)
├── Expedientes este mes: 450/1000 (45%)
│
├─ CILIN: OPERATIVO ✓
│  ├─ Última liquidación: hace 1h
│  ├─ Expedientes pendientes: 0
│  └─ Performance: Normal
│
├─ DOS: OPERATIVO ✓
│  ├─ Última actualización: hace 4h
│  ├─ Documentos en BD: 1,234
│  └─ Espacio usado: 2.3 GB / 10 GB
│
└─ SOCIA: OPERATIVO ✓
   ├─ Notificaciones hoy: 23
   ├─ Tasa de entrega: 98%
   └─ Performance: Normal
```

**Indicadores de salud**:
```
Verde ✓:   Todo normal
Naranja ⚠: Alertas menores (uso >80%, respuesta lenta)
Rojo ✗:    Error crítico (módulo caído, acceso bloqueado)
```

### 2. Gestión de Feature Flags

**Ver flags de un tenant**:
```
GET Municipio de Cali → Feature Flags

Liquidación (CILIN):
├─ LIQ-001 (Botón Liquidar): ON ✓ (Plan Estándar)
├─ LIQ-004 (Dashboard): ON ✓ (Plan Estándar)
├─ LIQ-005 (Reportes): OFF ✗ (Plan Premium)
└─ LIQ-006 (Descarga masiva): OFF ✗ (Escalado a resolver)

Documentos (DOS):
├─ DOC-001 (Crear doc): ON ✓
├─ DOC-003 (Firma digital): ON ✓
└─ DOC-005 (Búsqueda avanzada): OFF ✗

Acciones disponibles:
├─ Cambiar flags manualmente (con confirmación)
├─ Ver historial de cambios
├─ Resincronizar con plan
└─ Generar reporte de cobertura
```

**Resincronizar con plan**:
```
Si flag se desincroniza (ej: plan dice ON pero flag dice OFF)

Click "Resincronizar"
    ↓
Sistema:
├─ Lee plan contratado
├─ Calcula flags esperados
├─ Sincroniza con base de datos
├─ Invalida caché de aplicación
└─ Envía notificación a tenant
    ↓
Confirmación: "Resincronización completada"
```

### 3. Gestión de Usuarios

**Listar usuarios del tenant**:
```
Municipio de Cali - Usuarios:

| Email          | Nombre        | Rol      | Activo | Último login | Acción |
|----------------|---------------|----------|--------|--------------|--------|
| admin@cali...  | Carlos López  | Admin    | Sí     | hace 2h      | Editar |
| op1@cali...    | María García  | Operador | Sí     | hace 1d      | Editar |
| op2@cali...    | Juan Pérez    | Operador | Sí     | hace 3d      | Editar |
| read@cali...   | Ana Martínez  | Lectura  | No     | nunca        | Activar|

Acciones:
├─ Crear nuevo usuario
├─ Editar usuario (email, rol, módulos acceso)
├─ Desactivar usuario (reversible)
├─ Eliminar usuario (irreversible, con confirmación)
└─ Resetear contraseña
```

**Roles disponibles**:
```
Admin:
├── Acceso a todo
├── Puede crear/editar usuarios
└── Cambiar plan/configuración

Operador:
├── Acceso a módulos contratados
└── Operaciones básicas (no cambios de config)

Lectura:
├── Acceso a reportes y dashboards
└── Sin capacidad de modificación
```

### 4. Alertas y Problemas

**Panel de alertas activas**:
```
Últimas 24 horas:

🔴 CRÍTICA (0):
   Ninguna

🟡 ADVERTENCIA (3):
   ├─ Municipio PEREIRA: Módulo DOS sin respuesta por 10 min
   ├─ Municipio MANIZALES: Uso de espacio DOS > 80%
   └─ Municipio ARMENIA: 3 fallos de liquidación en últimas 2h

🟢 INFO (5):
   ├─ Municipio IBAGUÉ: Contrato activado
   ├─ Municipio CALI: Escalado de volumen detectado
   └─ ... (2 más)

Click en alerta → Detalles + Acciones recomendadas
```

**Acción sobre alerta**:
```
Alerta: "PEREIRA - DOS sin respuesta"
    ↓
Detalles:
├── Duración: 10 minutos
├── Última respuesta: 14:32
├── Status code: 503
├── Causa probable: BD lenta o servicio caído
├── Impacto: Usuarios no pueden crear documentos
│
└─ Acciones:
   ├── [ ] Reintentar conexión (fuerza refresh)
   ├── [ ] Verificar BD de DOS
   ├── [ ] Contactar administrador DOS
   ├── [ ] Notificar al cliente
   └── [ ] Escalar a equipo de infraestructura
```

### 5. Monitoreo de Escalados Automáticos

**Detectar si tenant supera límites**:
```
Municipio Cali (mes actual):
├── Expedientes procesados: 1,050
├── Límite caute: 1,000
├── Estado: ⚠️ SUPERADO
│
Acciones automáticas:
├── Activar feature flags de "% recaudo"
├── Generar notificación a cliente
├── Preparar factura con nuevo modelo
└── Marcar para revisión manual

Revisión manual:
├── Validar datos de expedientes
├── Confirmar cambio de modelo
├── Generar enmienda de contrato
└── Obtener firma cliente para escalado
```

### 6. Sincronización de Módulos

**Estado de sincronización con CILIN, DOS, SOCIA**:
```
Municipio Cali:

CILIN (Liquidación):
├── Última sincronización: hace 5 min
├── Expedientes BD Jikkoops: 1,050
├── Expedientes BD CILIN: 1,050
├── Diferencia: 0 ✓
└── Status: SINCRONIZADO

DOS (Documentos):
├── Última sincronización: hace 15 min
├── Usuarios BD Jikkoops: 42
├── Usuarios BD DOS: 42
├── Diferencia: 0 ✓
└── Status: SINCRONIZADO

SOCIA (Ciudadano):
├── Última sincronización: hace 2h
├── Eventos generados: 234
├── Eventos procesados: 234
├── Diferencia: 0 ✓
└── Status: SINCRONIZADO

Si hay desincronización:
    ├─ Alertar automáticamente
    └─ Ofrecer opción de resincronizar
```

### 7. Logs y Auditoría

**Ver logs de un tenant**:
```
Filtros:
├── Fecha y hora
├── Tipo de evento: [login, liquidacion, error, cambio_flag, etc.]
├── Usuario
├── Nivel: [error, warning, info, debug]
└── Palabra clave de búsqueda

Ejemplo:
2026-04-29 14:30:00 | INFO    | Carlos López | Activar flag LIQ-001 | Usuario: CALI-01
2026-04-29 14:32:15 | ERROR   | Sistema      | Fallo BD DOS timeout  | Resolvió: Auto-retry
2026-04-29 15:00:00 | WARNING | Sistema      | Uso espacio DOS > 80% | Notificación: Enviada
```

## Reportes de Operación

```
Reporte diario:
├── Tenants en línea: X
├── Problemas resueltos: Y
├── Escalados nuevos: Z
├── Performance promedio: W ms
└── Uptime: 99.95%

Reporte semanal/mensual:
├── KPIs de estabilidad
├── Incidentes y resoluciones
├── Tendencias de uso
└── Recomendaciones de optimización
```

## Permisos

```
Operations Manager:
├── Ver todo ✓
├── Cambiar flags ✓
├── Gestionar usuarios del tenant ✓
├── Resincronizar ✓
└── Resolver alertas ✓

Admin:
├── Todo ✓
```
