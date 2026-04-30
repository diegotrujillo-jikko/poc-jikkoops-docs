# JikkoOps - Sistema Integral de Back-Office Comercial

**JikkoOps** es el back-office comercial y operativo del ecosistema **SIGIA** (Sistema de Gestión Integral Administrativo) para municipios, gobernaciones y entidades públicas colombianas.

## 🎯 ¿Qué es JikkoOps?

Un sistema modular que permite:
- **Comercializar servicios digitales** de forma granular (liquidación, documentos, servicio al ciudadano)
- **Controlar acceso** mediante Protected Resources y Feature Flags
- **Monetizar flexiblemente** (modelo modelo-ingresos, porcentaje recaudo, por usuario, por expediente, híbrido)
- **Integrar ecosistema SIGIA** (sincronización bidireccional con SILIN, DOS, SOCIA, IAM)
- **Auditar exhaustivamente** (logs inmutables por 7 años - requisito fiscal)
- **Observabilidad completa** (métricas de costo por cada función)

## 📁 Estructura de Documentación

```
jikkoops-docs/
├── 00-general/                    # Documentos generales y transcripciones
├── 01-arquitectura/               # Diseño del sistema y conceptos
├── 02-procesos/                   # Procesos de negocio
├── 03-datos/                      # Capa de datos y modelos
├── 04-apis/                       # Integración y endpoints REST
├── 05-modulos-nucleares/          # Módulos core del sistema
├── 06-riesgos-y-decisiones/       # Riesgos e hitos arquitectónicos
└── DOCUMENTACION/                 # Archivos maestros de referencia
```

## 📖 Documentación Principal

### Para Comenzar
1. **RESUMEN_EJECUTIVO_ES.md** - Visión general en 5 minutos
2. **INDICE_DOCUMENTACION_ESPANOL.md** - Mapa completo de documentación
3. **GUIA_DOCUMENTACION_ESPANOL.md** - Conceptos y paradigma BD-first

### Arquitectura
- **01-arquitectura/01-vision-general.md** - Visión arquitectónica completa
- **01-arquitectura/recursos-protegidos.md** - Inventario de funcionalidades

### Procesos de Negocio
- **02-procesos/01-flujo-contratos.md** - Ciclo de vida del cliente (prospección → renovación)
- **02-procesos/02-modelos-ingresos.md** - Estrategias de monetización

### Base de Datos (⭐ CRÍTICO PARA MER)
- **03-datos/01-principios-base-datos.md** - Diseño BD con enfoque iterativo
- **03-datos/02-modelo-datos.md** - Esquema de base de datos (10 entidades)
- **03-datos/sincronizacion-flags.md** - Feature flags en tiempo real
- **03-datos/estrategia-cache.md** - Caché multi-nivel

### APIs e Integración
- **04-apis/01-endpoints-principales.md** - Referencia REST API completa
- **04-apis/eventos-integracion.md** - Tipos de eventos y webhooks

### Módulos del Sistema
- **05-modulos-nucleares/01-dashboard-comercial.md** - Dashboard de ventas y KPIs
- **05-modulos-nucleares/02-gestion-contratos.md** - Gestión del ciclo de contratos
- **05-modulos-nucleares/operaciones.md** - Monitoreo y operaciones
- **05-modulos-nucleares/04-facturacion.md** - Billing y facturación
- **05-modulos-nucleares/05-configuracion.md** - Configuración global

### Riesgos y Decisiones
- **06-riesgos-y-decisiones/01-riesgos-identificados.md** - Matriz de riesgos
- **06-riesgos-y-decisiones/02-decisiones-arquitecturales.md** - Por qué cada decisión

## 🔑 Conceptos Clave

### Protected Resources (Recursos Protegidos)
Inventario exhaustivo de funcionalidades (botones, endpoints, vistas) que pueden activarse/desactivarse por tenant y plan.
- **Código único**: LIQ-001, DOC-003, SOC-001
- **Control**: Feature flags en runtime
- **Auditoría**: Trazabilidad exacta de acceso

### Plans (Ofertas Comerciales)
Combinaciones de funcionalidades con modelos de pricing flexibles:
- **Plan Básico**: Liquidación básica, 50 usuarios, $0
- **Plan Estándar**: Liquidación + Documentos, 100 usuarios, 10% recaudo
- **Plan Premium**: Todos los módulos, ilimitado, 20% recaudo

### Modelos de Revenue
- **Caute**: Período gratis N expedientes, luego se cobra
- **Porcentaje de Recaudo**: % de lo que el cliente recauda
- **Por Usuario**: $X por usuario/mes
- **Por Expediente**: $X por expediente procesado
- **Híbrido**: Combinación de los anteriores

### Tenant (Instancia del Cliente)
Una entidad (municipio) puede tener múltiples tenants (instancias en JikkoOps), cada uno con:
- BD independiente
- Plan asignado
- Feature flags configurados
- Contrato específico

## 🚀 Cambio de Paradigma: Enfoque Base-Datos-Primero

### Antes (Alto Riesgo)
```
Año 0: BD pequeña (10-20% definida)
Año 1-2: Agregamos tablas incrementalmente
Año 3+: Refactorización costosa y riesgosa
```

### Ahora (Recomendado)
```
Día 1: BD fuerte (90% definida) usando IA
Mes 1-12: Refinamiento iterativo
Año 2+: BD estable, escalable, mantenible
```

**Ventaja**: Usar IA para planificación futura es **10x más rápido** que refactorizaciones tardías.

## 📊 Integración de Transcript (2026-04-30)

La documentación integra insights de una reunión de 127 minutos con el equipo:

### Herramientas Clave
- **Alembic**: Versionamiento automático de BD (migrations reversibles)
- **ChatGPT**: Validación iterativa de esquemas de BD
- **Cloud Code**: Generación automática de migrations
- **MFA**: Autenticación Multi-Factor en endpoints críticos
- **Playwright**: Testing E2E automatizado
- **SDK Métricas**: Medición de costo por cada función

### Concepto Principal
> "La base de datos es el corazón del sistema.
> Las APIs y UI se adaptan a ella, nunca lo opuesto."

## 🔐 Requisitos de Seguridad

### MFA (Multi-Factor Authentication) - CRÍTICO
- `POST /liquidaciones` - Crear liquidación
- `PUT /feature-flags/{id}` - Cambiar activaciones
- `POST /contracts/{id}/cambiar-modelo` - Modificar pricing
- `DELETE /invoices/{id}` - Anular factura

**Implementación**: Google Authenticator (TOTP) + validación en backend

### Auditoría Fiscal
- Logs inmutables por **7 años** (requisito legal)
- Tabla `audit_log` con: quién, qué, cuándo, valores anterior/nuevo
- Especialmente crítico para cambios de precios e ingresos

### Encriptación
- En reposo: AES-256-GCM
- En tránsito: TLS 1.2+
- Datos sensibles: contratos, facturas, credenciales

## 📊 8 Casos de Uso Críticos (Para Validar BD)

1. **CU-001**: Crear contrato modelo-ingresos + porcentaje, generar factura
2. **CU-002**: Escalado automático cuando se supera límite modelo-ingresos
3. **CU-003**: MFA en liquidación crítica ($50M+)
4. **CU-004**: Reporte predictivo: "¿Cuánto dinero falta para pagar contratos?"
5. **CU-005**: Cambiar Protected Resource sin afectar clientes en producción
6. **CU-006**: Soportar 50M liquidaciones/mes con BD particionada
7. **CU-007**: Auditoría de 7 años (18.25B registros)
8. **CU-008**: Multi-tenancy con BD separada por tenant

## 🎯 Próximos Pasos

### Esta Semana
1. ✅ Documentación en español completada
2. ⏳ Casos de uso detallados por módulo
3. ⏳ Iterar BD en ChatGPT hasta 90% completitud
4. ⏳ Definir Alembic migrations

### Semanas 2-4
5. ⏳ Implementar MFA en endpoints críticos
6. ⏳ Configurar SDK Métricas
7. ⏳ Definir índices y particiones
8. ⏳ Testing con Playwright

### Meses 1-3
9. ⏳ Generar ERD/MER (Modelo Entidad-Relación)
10. ⏳ Documentación de BD lista para desarrollo
11. ⏳ APIs desarrolladas alrededor de BD
12. ⏳ UI/UX implementada en paralelo
13. ⏳ Sistema en producción

## 💻 Desarrollo

### Convenciones de Commit
Usar conventional commits:
```
feat: nueva funcionalidad
fix: corrección de bug
docs: cambios en documentación
refactor: refactorización de código
test: pruebas
```

### Ramas
- `main`: producción
- `develop`: integración de features
- `feature/*`: nuevas funcionalidades

### Testing
- Mínimo 80% de cobertura
- Tests unitarios, integración y E2E
- Validación de BD con casos de uso

## 📞 Preguntas Frecuentes

**P: ¿Por dónde comenzar?**  
A: Lee `RESUMEN_EJECUTIVO_ES.md` (5 min), luego `INDICE_DOCUMENTACION_ESPANOL.md`

**P: ¿Necesitamos interfaz gráfica?**  
A: No para el MVP. Un chatbot inteligente puede hacer operaciones complejas.

**P: ¿Cuánto toma diseñar la BD al 90%?**  
A: 2-4 horas iterando con ChatGPT + Cloud Code.

**P: ¿Es obligatorio Alembic?**  
A: Sí, para auditoría fiscal (requisito legal 7 años).

**P: ¿Cómo validamos que la BD es suficiente?**  
A: Ejecutar todos los 8 casos de uso contra ella.

## 📄 Licencia

Jikkosoft © 2026 - Todos los derechos reservados

---

**Última actualización**: 2026-04-30  
**Estado**: Documentación completa, lista para MER y desarrollo
