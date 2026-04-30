# Visión General de la Arquitectura JikkoOps

## Resumen Ejecutivo

**JikkoOps** es el back-office comercial y operativo de **SIGIA**, un ecosistema modular para gobierno inteligente. Vende, activa, mide y factura servicios modulares a entidades públicas de forma granular, flexible y auditable.

## Propósito

- ✅ Comercializar servicios modulares (CILIN, DOS, SOCIA)
- ✅ Controlar acceso granular mediante Protected Resources + Feature Flags
- ✅ Monetizar flexiblemente (Caute, % recaudo, por usuario, expediente, híbridos)
- ✅ Integrar ecosistema SIGIA (sincronización bidireccional)
- ✅ Auditar exhaustivamente (7 años de logs inmutables)
- ✅ Medir granularmente (métricas por función)

## Usuarios Principales

| Rol | Descripción |
|-----|-------------|
| **Jikkosoft Admins** | Gestión de productos, planes, recursos |
| **Entity Admins** | Administradores municipio/gobernación |
| **Managers** | Operadores, reportería, facturación |
| **Alcaldes/Directores** | Dashboards predictivos, alertas |

## Arquitectura de Capas

```
Portal/UX (Dashboards, Formularios, Reportes, Chatbot)
    ↓
API REST/GraphQL (Operaciones CRUD)
    ↓
Módulos Core
├─ Dashboard Comercial (KPIs, renovaciones)
├─ Gestión Contratos (crear, activar, monitorear)
├─ Operaciones (health, flags, usuarios)
├─ Facturación (generar, revisar, cobrar)
└─ Configuración (productos, planes, integraciones)
    ↓
Layer de Control
├─ Protected Resources (inventario)
├─ Feature Flags (activación runtime)
├─ Plans (funcionalidades + pricing)
├─ Tenant Entitlements (qué tiene cada cliente)
└─ Métricas SDK (costo por función)
    ↓
Data Layer
├─ PostgreSQL (BD por tenant)
├─ Redis Cache (multi-nivel)
├─ Message Queue (eventos)
├─ Alembic (versionamiento BD)
└─ Audit Log (logs inmutables)
```

## Flujo de Comercialización

```
Prospección → Propuesta → Firma → Activación → Operación → Renovación
```

## Conceptos Clave

### Protected Resources
Inventario de funcionalidades (botones, endpoints, vistas):
- **Código único**: LIQ-001, DOC-003, SOC-001
- **Criticidad**: crítica, alta, media, baja
- **Control**: Feature flags en runtime

### Plans (Ofertas Comerciales)
Combinación de funcionalidades + modelo de pricing:
- Plan Básico: CILIN, 50 usuarios, $0
- Plan Estándar: CILIN + DOS, 100 usuarios, 10% recaudo
- Plan Premium: Todos, ilimitado, 20% recaudo

### Tenant Entitlements
Define qué Protected Resources están activos para cada tenant en tiempo de ejecución.

### Feature Flags
Control en runtime de cada recurso:
- ON: Botón visible, endpoint funcional
- OFF: Botón oculto, endpoint rechaza
- Automático: Activa al superar límite (escalado)

## Cambio de Paradigma (Reunión 2026-04-30)

### Antiguo Enfoque
- BD pequeña (10-20% definida) al inicio
- Iterar agregando tablas cada 6 meses
- Resultado: Refactorización costosa en año 2-3

### Nuevo Enfoque
- BD fuerte (90% definida) desde el inicio usando IA
- Validar iterativamente con ChatGPT
- Usar Alembic para migrations automáticas
- Resultado: BD estable y escalable

**Ventaja**: Diseñar para 5 años en adelante desde el inicio.

## Requisitos de Seguridad

### MFA (Multi-Factor Authentication)
Endpoints críticos requieren Google Authenticator:
- Liquidaciones
- Cambios de feature flags
- Modificaciones de contratos
- Anulación de facturas

### Auditoría Fiscal
- Logs inmutables por 7 años
- Quién, qué, cuándo, dónde, por qué
- Especialmente: cambios de precios e ingresos

## Métricas y Observabilidad

Cada función registra:
- **Resource ID**: Qué se ejecutó
- **Execution Time**: Milisegundos
- **Tokens Used**: Consumo IA
- **Cost USD**: Tokens × tarifa
- **Timestamp**: Cuándo

## Próximos Pasos

1. Leer `recursos-protegidos.md` - Inventario de funciones
2. Leer `principios-base-datos.md` - Diseño de BD
3. Crear casos de uso por módulo
4. Validar BD con IA
5. Definir Alembic migrations
6. Implementar MFA
7. Configurar métricas
8. Generar ERD/MER

---
**Versión**: 1.0 | **Actualizado**: 2026-04-30
