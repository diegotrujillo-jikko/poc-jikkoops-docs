# Decisiones Arquitecturales

## 1. Multi-Tenant con Bases de Datos Separadas (No Esquemas)

**Decisión**: Cada tenant tiene su propia base de datos PostgreSQL.

**Alternativas consideradas**:
- Row-level security (RLS) en una BD compartida
- Esquemas PostgreSQL separados
- Base de datos separada (elegido ✓)

**Razones**:
- Aislamiento de datos completo (seguridad)
- Performance independiente por tenant (no competencia de recursos)
- Escalabilidad: Distribuir tenants en múltiples servidores
- Compliance: Algunos municipios requieren datos locales/separados
- Backup granular: Recuperar un tenant sin afectar otros

**Tradeoff**:
- Más complejo de administrar
- Más recursos de infraestructura
- Mitigado con: Terraform, automatización de backup/restore

**Riesgo**: 
- Desincronización de esquemas entre tenants
- Mitigado: Script de validación esquema en deploy

## 2. Feature Flags como Control de Acceso (No JWT Claims)

**Decisión**: Usar tabla `feature_flags` en BD como fuente de verdad, no JWT tokens.

**Alternativas**:
- Incluir flags en JWT (estateless)
- Tabla DB con caché Redis (elegido ✓)

**Razones**:
- Control centralizado y dinámico sin relogin
- Cambios toman efecto inmediatamente (crítico para revenue)
- Auditable: Ver quién cambió qué y cuándo
- Rollback inmediato sin token reissue

**Tradeoff**:
- Una query más en cada request (mitigado con caché Redis)
- Token no es stateless
- Mitigado: Caché con TTL 5 min

**Riesgo**:
- Redis down → Fallback a BD (lento pero funciona)

## 3. Eventos (Pub/Sub) para Sincronización Multi-Módulo

**Decisión**: Usar message queue (Kafka/RabbitMQ) para eventos, no llamadas síncronas.

**Alternativas**:
- Llamadas síncronas REST entre módulos
- Eventos asíncronos con cola de mensajes (elegido ✓)

**Razones**:
- Desacoplamiento: Módulos no dependen uno del otro estar up
- Resiliencia: Si SILIN está lento, JikkoOps sigue funcionando
- Escalabilidad: Múltiples consumidores del mismo evento
- Auditoría: Event log completo es source of truth

**Tradeoff**:
- Complejidad: Manejar reconversión, order, duplicación
- Consistencia eventual (no inmediata)
- Mitigado: Validaciones en consumidor, event store 12 meses

**Riesgo**:
- Evento se pierde o procesa 2 veces
- Mitigado: Idempotencia, event sourcing, dead letter queue

## 4. Revenue Calculations en BD (No Microservicio)

**Decisión**: Cálculos de facturación ocurren en JikkoOps (Python/Node), no en microservicio separado.

**Alternativas**:
- Microservicio "Revenue Calculator"
- Lógica en BD (stored procedures)
- Lógica en aplicación JikkoOps (elegido ✓)

**Razones**:
- Código legible y auditable (Python/JavaScript vs SQL)
- Testing simplificado (unit + integration tests)
- Versionado junto con changelog (Git)
- Cambios controlados con code review

**Tradeoff**:
- Todo acoplado en JikkoOps
- Mitigado: Módulos pequeños y cohesivos en `/modules/revenue`

**Riesgo**:
- Bug en cálculo afecta muchos tenants
- Mitigado: Extensive testing, validation, auditoría

## 5. Postgres para BD Transaccional (No NoSQL)

**Decisión**: PostgreSQL como BD principal, no MongoDB/DynamoDB.

**Alternativas**:
- NoSQL document DB (MongoDB)
- Serverless (DynamoDB)
- PostgreSQL (elegido ✓)

**Razones**:
- ACID guarantees (crítico para dinero)
- Joins eficientes (tenemos relaciones complejas)
- Full-text search, JSON support (ambos útiles)
- SQL es familiar para auditoría y reportes
- Costo: Postgres es open source, DynamoDB es caro
- Compliance: Algunas regulaciones prefieren SQL

**Tradeoff**:
- Menos flexible que NoSQL para cambios de schema
- Mitigado: Versionamiento de migracione en Flyway/Alembic

**Riesgo**:
- Scaling horizontal más difícil (vs Cassandra)
- Mitigado: Sharding por tenant si escala lo requiere

## 6. Caché Multinivel (Browser + API Gateway + Redis)

**Decisión**: Tres capas de caché con different TTLs.

**Alternativas**:
- Una sola capa de caché
- Solo caché en browser
- Multinivel (elegido ✓)

**Razones**:
- L1 (Browser): Reduce load en servidor
- L2 (API Gateway): Rate limiting, protección
- L3 (Redis): Aplicación stateless, escalable

**Tradeoff**:
- Complejidad de invalidación
- Posible staleness de datos
- Mitigado: Invalidación activa por eventos, fallback a BD

**Riesgo**:
- Data stale por mucho tiempo
- Mitigado: TTLs ajustados por criticality

## 7. Protected Resources como Inventario Explícito

**Decisión**: Inventariar cada botón, endpoint, vista como "Protected Resource" con código único.

**Alternativas**:
- Feature toggle implícito (solo en código)
- Permisos granulares en IAM
- Inventario explícito (elegido ✓)

**Razones**:
- Auditable: Saber exactamente qué está activo
- Modularización: Agrupar en planes
- Comercial: Vender funcionalidades sin código
- Scaling: Agregar nuevas funcionalidades sin afectar existentes

**Tradeoff**:
- Overhead inicial de inventariar
- Mantener sincronizado con código
- Mitigado: Script de scan automático en deploy

**Riesgo**:
- Recurso "huérfano" (creado en código pero no inventariado)
- Mitigado: Build fail si hay recursos no inventariados

## 8. Modelos de Revenue Configurables (JSON, No Code)

**Decisión**: Guardar definición de modelos de revenue (modelo-ingresos, %, usuarios, etc) como JSON en BD.

**Alternativas**:
- Código hardcoded
- Decisiones comerciales manuales
- JSON configurable (elegido ✓)

**Razones**:
- Cambios sin código: CFO puede actualizarlos
- Escalado automático: Detectar límite modelo-ingresos y cambiar modelo
- Auditable: Ver qué modelo se aplicó cuando
- Testing: Probar nuevos modelos sin deploy

**Tradeoff**:
- Necesario parser/interpreter de modelos
- Menos flexible que código
- Mitigado: Soporte los casos 80/20

**Riesgo**:
- Modelo inválido → Facturas erradas
- Mitigado: Validación schema JSON, test before use

## 9. Módulos Integrados vs Construidos In-House

**Decisión**: Integrar SILIN, DOS, SOCIA (sistemas existentes), no construir nuestros.

**Alternativas**:
- Construir liquidación propia
- Integrar módulos existentes (elegido ✓)

**Razones**:
- Rápido a mercado
- Especialista en cada cosa (SILIN hace liquidación bien)
- Menor burden operacional
- Compliance ya existe en módulos

**Tradeoff**:
- Dependencia de otros equipos
- Cambios en SILIN nos afectan
- Mitigado: APIs versionadas, contractos claros

**Riesgo**:
- SILIN caído → JikkoOps no puede facturar
- Mitigado: Modo degradado, caché de últimos datos

## 10. Audit Log Inmutable (Insert-only Table)

**Decisión**: Tabla de auditoría donde solo se agrega, nunca se modifica/borra.

**Alternativas**:
- Logs en archivo (ELK stack)
- BD updatable con triggers
- Insert-only table (elegido ✓)

**Razones**:
- Inmutable por naturaleza DB
- Queryable como SQL (joins, reports)
- Cumple requerimientos fiscales (7 años)
- Parte de backup automático

**Tradeoff**:
- Tabla crece mucho (mitigado: archiving)
- Queries lentas en tabla grande (mitigado: índices)

**Riesgo**:
- Audit log mismo puede ser corrupto
- Mitigado: Validación integridad, backup separado

## 11. JWT para Autenticación, No Session Cookies

**Decisión**: Usar JWT (stateless) para APIs, session cookies para UI.

**Alternativas**:
- Solo session cookies
- Solo JWT
- Híbrido (elegido ✓)

**Razones**:
- APIs: JWT es standard para OAuth/OIDC
- Browser: Cookies en HTTP-only más seguro
- Escalabilidad: No necesita session store para APIs
- Compatibility: Integraciones externas usan JWT

**Tradeoff**:
- Dos mecanismos diferentes (complejidad)
- Mitigado: Middleware de autenticación centralizado

**Riesgo**:
- Token hijacking
- Mitigado: HTTPS obligatorio, corta duración (15 min)

## 12. Domain-Driven Design (Entidades Principales)

**Decisión**: Modelar con entidades: Entity, Tenant, Contract, Invoice, ProtectedResource, Plan.

**Alternativas**:
- Anemic model (solo datos)
- Rich model (elegido ✓)

**Razones**:
- Lógica cerca de datos
- Validaciones en entidad, no esparcidas
- Menos bugs (invariantes en lugar)
- Entendible para nuevos devs

**Tradeoff**:
- Más líneas de código
- ORM overhead
- Mitigado: Herramientas modernas (SQLAlchemy, TypeORM)

**Riesgo**:
- Lógica duplicada entre módulos
- Mitigado: Shared library de dominio

## Matriz de Decisiones

```
Decisión                 | Criticality | Reversibilidad | Reversión Cost
─────────────────────────────────────────────────────────────────────────
1. DB separada/tenant    | Crítica     | Difícil        | Alto (data migration)
2. Feature Flags en BD   | Crítica     | Fácil          | Bajo (código + datos)
3. Eventos (Pub/Sub)     | Alta        | Media          | Medio (refactor)
4. Revenue en app        | Crítica     | Difícil        | Alto (tests + deploy)
5. PostgreSQL            | Crítica     | Muy Difícil    | Muy Alto (data)
6. Caché Multinivel      | Media       | Fácil          | Bajo (config)
7. Protected Resources   | Alta        | Media          | Medio (inventario)
8. Models JSON           | Media       | Fácil          | Bajo (schema)
9. Módulos integrados    | Alta        | Muy Difícil    | Muy Alto (dev)
10. Audit Insert-only    | Crítica     | Muy Difícil    | Muy Alto (data)
11. JWT + Cookies        | Alta        | Fácil          | Bajo (auth lib)
12. DDD Entities         | Media       | Fácil          | Bajo (refactor)
```

## Revisión y Cambio

Las decisiones arquitecturales se revisan:
- **Anualmente**: Por architecture review board
- **Ad-hoc**: Si emergen nuevos requerimientos críticos
- **Post-incident**: Si incidente reveló limitación

Para cambiar una decisión crítica:
1. Propuesta escrita con justificación
2. Análisis de impacto (costo, tiempo, riesgo)
3. Aprobación de CTO + CFO
4. Plan de transición (no big-bang)
5. Rollback plan por si falla
