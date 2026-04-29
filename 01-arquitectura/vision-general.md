# Visión General de Arquitectura JikkoOps

## Overview

**JikkoOps** es el back office comercial y operativo de un ecosistema modular de gobierno inteligente (SIGIA). Se trata de un sistema integrado que centraliza gestión de clientes, operaciones, facturación, y la activación de servicios a través de Protected Resources y Feature Flags.

## Propósito

JikkoOps permite:
- Vender, activar, medir y facturar servicios modulares a diferentes entidades (municipios, gobernaciones, DIAN, etc.)
- Gestionar contratos y operaciones de múltiples sistemas (CILIN, DOS, SOCIA)
- Controlar acceso a funcionalidades mediante Protected Resources y Tenant Entitlements
- Monetizar servicios con modelos de revenue flexibles (por expediente, por porcentaje de recaudo, por usuario, híbridos)

## Usuarios Principales

- **Jikkosoft Users**: Administradores internos que gestionan productos, planes y configuración
- **Entity Admins**: Administradores de municipios/gobernaciones que usan JikkoOps
- **Managers**: Usuarios finales que usan módulos específicos del sistema

## Entidades Soportadas

- Municipios (alcaldías)
- Gobernaciones
- DIAN
- Cualquier entidad del sector público

## Integración con SIGIA

JikkoOps es la capa comercial y operativa de **SIGIA** (Sistema de Gestión Integral Administrativo), que incluye:

- **CILIN**: Módulo de liquidación y facturación
- **DOS**: Gestión documental y archivo
- **SOCIA**: Servicio al ciudadano y notificaciones
- **IAM**: Identity and Access Management (gestión transversal de usuarios)

Cada uno de estos módulos tiene su propio portal interno alojado en JikkoOps.

## Arquitectura de Capas

```
┌─────────────────────────────────────────────────────────┐
│  Portal / UX (Dashboard, Formularios, Reportes)         │
├─────────────────────────────────────────────────────────┤
│  API REST / GraphQL                                      │
├─────────────────────────────────────────────────────────┤
│  Módulos Core (Dashboard, Contratos, Op, Facturación)   │
├─────────────────────────────────────────────────────────┤
│  Protected Resources → Feature Catalog → Plans           │
├─────────────────────────────────────────────────────────┤
│  Tenant Entitlements → Runtime Flags                     │
├─────────────────────────────────────────────────────────┤
│  Data Layer (SQL, Cache, Feature Flags Store)           │
└─────────────────────────────────────────────────────────┘
```

## Flujo de Comercialización

1. **Definir Producto**: Agrupar Protected Resources por funcionalidad
2. **Crear Plans**: Definir combinaciones de funcionalidades y modelos de pricing
3. **Comercializar**: Crear propuestas personalizadas por entidad
4. **Ejecutar Contrato**: Activar servicios y planes en el tenant
5. **Operar**: Monitorear uso, aplicar feature flags, facturar según modelo

## Conceptos Clave

### Protected Resources
Inventario de botones, endpoints, vistas y acciones que pueden ser activadas/desactivadas. Cada recurso:
- Tiene código único
- Pertenece a una funcionalidad
- Puede estar en uno o varios planes
- Se controla mediante feature flags en runtime

### Feature Catalog
Agrupación de Protected Resources por funcionalidad. Ejemplos:
- Liquidación (botón liquidar, endpoint /liquidar, vistas, acciones)
- Expedientes (CRUD, búsqueda, filtros)
- Notificaciones (enviar manual, digital, registrar entrega)

### Plans
Combinación de funcionalidades con modelo de pricing. Ejemplo:
- **Plan Básico**: Liquidación + Expedientes básicos, 50 usuarios, $0
- **Plan Estándar**: Liquidación + Expedientes avanzados + Reportes, ilimitado, 10% recaudo
- **Plan Premium**: Todos los módulos, 20% recaudo

### Tenant Entitlements
Define qué planes y funcionalidades tiene una entidad contratante.

### Runtime Flags
Feature flags que controlan qué Protected Resources están activos para un tenant en tiempo de ejecución.

## Siguientes Pasos

Ver `proteccted-resources.md` para detalles sobre inventariar y agrupar recursos.
