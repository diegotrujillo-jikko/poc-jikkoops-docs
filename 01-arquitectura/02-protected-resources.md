# Protected Resources (Recursos Protegidos)

## Definición

Los **Protected Resources** son el inventario exhaustivo de elementos interactivos y funcionales que pueden ser habilitados/deshabilitados en JikkoOps:

- **Botones y Acciones**: Liquidar, Crear expediente, Enviar notificación, Descargar reporte
- **Endpoints API**: GET /clientes, POST /liquidaciones, PUT /expedientes/{id}
- **Vistas/Pantallas**: Dashboard, Módulo de contratos, Módulo de facturación
- **Características Especiales**: Búsqueda avanzada, Reportes personalizados, Integraciones

## Propósito

Permitir:
- **Modularización**: Vender funcionalidades de forma granular
- **Control de Acceso**: Activar/desactivar por tenant y plan
- **Revenue Modeling**: Monetizar cada recurso o grupo de recursos
- **Auditoría**: Inventariar exactamente qué está disponible en cada ambiente

## Cómo Inventariar un Recurso

Cada Protected Resource debe tener:

| Campo | Ejemplo | Descripción |
|-------|---------|-------------|
| **Código** | `LIQ-001` | Identificador único (prefijo módulo + número) |
| **Nombre** | `Botón Liquidar` | Descripción breve |
| **Tipo** | `button` / `endpoint` / `view` | Categoría del recurso |
| **Módulo** | `SILIN` | Módulo que contiene el recurso |
| **Descripción** | "Permite al usuario... " | Qué hace el recurso |
| **Criticidad** | `crítica` / `alta` / `media` / `baja` | Importancia para el negocio |
| **Dependencias** | `[DB-001, API-002]` | Otros recursos que requiere |

## Inventario por Módulo

### SILIN (Liquidación)
```
LIQ-001: Botón Liquidar (button, crítica)
LIQ-002: Endpoint POST /liquidaciones (endpoint, crítica)
LIQ-003: Endpoint GET /liquidaciones/{id} (endpoint, crítica)
LIQ-004: Vista Dashboard de Liquidación (view, alta)
LIQ-005: Reporte de Liquidaciones (action, media)
```

### DOS (Documentos)
```
DOC-001: Crear documento (button, alta)
DOC-002: Asignar TRD (button, alta)
DOC-003: Firma digital (action, crítica)
DOC-004: Vista Búsqueda de documentos (view, alta)
```

### SOCIA (Servicio al Ciudadano)
```
SOC-001: Enviar notificación manual (button, alta)
SOC-002: Enviar notificación digital (action, media)
SOC-003: Registrar entrega física (action, media)
SOC-004: Buzón de correspondencia (view, alta)
```

## Recursos Huérfanos

Los **Recursos Huérfanos** son Protected Resources que:
- Fueron creados en código (nuevo botón, nuevo endpoint)
- AÚN NO están agrupados en ninguna funcionalidad
- No están disponibles para ningún plan/tenant

**Acción**: Cada deploy debe:
1. Inventariar nuevos recursos automáticamente
2. Marcarlos como huérfanos en la matriz
3. Un manager los agrupa a una funcionalidad
4. Se vuelven disponibles en planes

## Matriz de Protección

Se mantiene en el sistema (base de datos, archivo JSON, o Google Sheets):

```
Código | Nombre | Tipo | Módulo | Funcionalidad | Plan Básico | Plan Std | Plan Prem | Estado
-------|--------|------|--------|---------------|-------------|----------|-----------|--------
LIQ-001| Liquidar| btn  | SILIN  | Liquidación   | ✓           | ✓        | ✓         | active
SOC-001| Notif  | btn  | SOCIA  | Notificaciones| ✗           | ✓        | ✓         | active
DOC-005| Nuevo  | btn  | DOCIA    | (ninguno)     | -           | -        | -         | huérfano
```

## Almacenamiento

La matriz de Protected Resources se almacena en:
- **Opción 1**: Base de datos (tabla `protected_resources`)
- **Opción 2**: Archivo JSON `knowledge/protected-resources.json` (versionable en git)
- **Opción 3**: Google Sheets compartida (más accesible para negocio)

**Recomendación**: Mantener en DB + sincronizar copia en JSON para auditoría.

## Ciclo de Vida

```
Desarrollo (código nuevo)
    ↓
Inventariar (scan automático)
    ↓
Recurso Huérfano (sin funcionalidad)
    ↓
Agrupar a Funcionalidad (manual/approval)
    ↓
Activar en Plans (feature flag)
    ↓
Runtime: Verificar entitlements
    ↓
Mostrar/Ocultar en UI
```

## Scripts de Inventario

### Generar Inventario Automático

```bash
# Script que scanea código y genera recursos huérfanos
npm run inventory:scan

# Output: nuevos recursos detectados en inventory/huérfanos.json
```

### Sincronizar Matriz

```bash
# Sincronizar BD con copia JSON
npm run inventory:sync-matrix

# Generar reporte de cobertura
npm run inventory:report
```

## Validación

No se permite cambiar:
- Código de recurso existente
- Criticidad sin revisión
- Estado a "activo" sin aprobación

Estos cambios requieren **Code Review** y validación de impacto en revenue/compliance.
