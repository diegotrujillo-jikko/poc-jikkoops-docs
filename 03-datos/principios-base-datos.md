# Principios de Diseño de Base de Datos - JikkoOps

## Concepto Fundamental

**La base de datos es el corazón del sistema.** Las APIs y UX se adaptan a ella, nunca lo opuesto.

## Enfoque Base-Datos-Primero

### Por Qué

Una BD débil al inicio requiere refactorizaciones costosas:
- Año 0-6: Agregamos tablas
- Año 12: Agregamos más tablas
- Año 24+: BD se vuelve un "monstruo"
- Resultado: Reescritura completa, alto riesgo

### Solución: Diseño para el Futuro

1. **Recopilar todos los casos de uso** (presentes y futuros estimados)
2. **Diseñar BD al 90% de completitud** desde el inicio
3. **Usar IA (ChatGPT) como experto** en validación
4. **Iterar rápidamente** (minutos, no semanas)
5. **Versionar con Alembic** (cada cambio es rastreable)

**Resultado**: BD estable por 5+ años, sin refactorizaciones mayores.

## Proceso Recomendado

### Fase 1: Recopilación de Casos de Uso

Documentar preguntas que el sistema debe responder:

```
CU-001: "¿Cuánto cuesta liquidar 50M expedientes/mes?"
CU-002: "¿Soporta MFA en liquidación?"
CU-003: "¿Puedo cambiar modelo de pricing sin afectar cliente actual?"
CU-004: "¿Qué contratos están venciendo en 90 días?"
CU-005: "¿Cuál es el recaudo proyectado vs real?"
CU-006: "¿Cuántos usuarios activos hay por tenant?"
CU-007: "¿Qué endpoints consumen más tokens?"
CU-008: "¿Cuánto vale el downtime de 1 minuto en producción?"
```

### Fase 2: Diseño Iterativo con IA

**Paso a paso:**

1. **Crear prompt base** en ChatGPT:
   ```
   "Diseña una BD PostgreSQL para JikkoOps que soporte:
   - 50M liquidaciones/mes
   - Multi-tenant (cada entidad tiene su BD)
   - Modelos de pricing: caute, %, usuario, expediente, híbrido
   - MFA en endpoints críticos
   - Auditoría de 7 años
   - Métricas por función
   
   ¿Qué tablas necesito?"
   ```

2. **ChatGPT propone esquema inicial**

3. **Validar cada tabla con preguntas:**
   ```
   P: "¿Esta tabla soporta escalado automático de modelo caute→porcentaje?"
   R: No → Agregamos columna `escalado_tipo`, `escalado_fecha`, `escalado_por_uuid`
   
   P: "¿Cómo registro cada cambio de feature flag?"
   R: Necesitamos tabla `feature_flag_audit` con quién, cuándo, anterior, nuevo
   
   P: "¿Puedo particionar la tabla de liquidaciones?"
   R: Sí, por rango de fechas o hash de tenant_id para mejorar performance
   ```

4. **Iterar hasta 90% completitud**
   - Primera tanda: 90% de tablas necesarias
   - No esperamos 100% (imposible predecir todo)
   - Pero evitamos cambios mayores

### Fase 3: Definir Índices y Particiones

```sql
-- Índices críticos para performance
CREATE INDEX idx_contracts_tenant_estado 
  ON contracts(tenant_id, estado);

CREATE INDEX idx_invoices_tenant_periodo 
  ON invoices(tenant_id, periodo_inicio, periodo_fin);

CREATE INDEX idx_feature_flags_tenant_activo 
  ON feature_flags(tenant_id, activo);

-- Particionamiento por rango de fechas
CREATE TABLE liquidaciones 
  PARTITION BY RANGE (DATE_TRUNC('month', fecha_creacion))
```

### Fase 4: Versionar con Alembic

Cada cambio a la BD es una **migration** versionada:

```bash
# Cloud Code genera automáticamente
alembic revision --autogenerate -m "Agregar tabla de MFA"

# Output: versions/abc123_agregar_tabla_mfa.py
# Contiene: ↑ (upgrade) y ↓ (downgrade)

# Aplicar a DB
alembic upgrade head

# Rollback si hay problema
alembic downgrade -1
```

**Ventaja**: Auditoría completa de cambios + rollbacks automáticos.

## Herramientas Clave

### Alembic (Migrations Automáticas)

Qué hace:
- Detecta cambios en modelos SQLAlchemy
- Genera migration files automáticamente
- Aplica/revierte cambios en BD
- Mantiene historico de versiones

Por qué importa:
- Auditoría fiscal: 7 años de historico
- Rollbacks rápidos si algo sale mal
- Multi-environment (dev, staging, prod)

### ChatGPT como Experto en BD

```
Diego: "¿Esta BD aguanta 50M liquidaciones?"
ChatGPT: "No. Necesitas:
  - Particionar por fecha
  - Índices en (tenant_id, estado)
  - Caché en Redis para queries frecuentes
  - Agregar tabla de métricas para tracking"

Diego: "¿Cómo implemento MFA?"
ChatGPT: "Agregar tabla mfa_credentials con:
  - secret (cifrado)
  - backup_codes
  - intentos_fallidos (para lockout)
  - Validar en endpoints críticos"
```

### Cloud Code / Claude para Generar Migrations

```
Diego: "Genera migration para agregar tabla de MFA"
Claude: "
# versions/20260430_mfa_credentials.py
def upgrade():
  op.create_table('mfa_credentials',
    sa.Column('id', sa.UUID),
    sa.Column('user_id', sa.UUID),
    sa.Column('secret', sa.String(encrypted)),
    ...
  )
def downgrade():
  op.drop_table('mfa_credentials')
"
```

## Validación de Completitud

Para cada caso de uso, verificar:

```
CU-001: "Crear contrato con modelo caute + porcentaje"
├─ ¿Tabla contracts existe? ✓
├─ ¿Columna modelo_revenue? ✓
├─ ¿Tabla tenant_entitlements para flags? ✓
├─ ¿Registro automático en audit_log? ✓
└─ ¿Calcular próxima factura? Necesita columna límite_caute

CU-002: "Liquidación requiere MFA"
├─ ¿Tabla mfa_credentials? ✓
├─ ¿Validación en endpoint? Aplicar en API
├─ ¿Registro de intentos fallidos? ✓
└─ ¿Cifrado del secret? ✓
```

## Especificación de Tablas Críticas

### 1. contracts

```sql
TABLE contracts
├── id: UUID (PK)
├── tenant_id: UUID (FK)
├── numero: STRING (unique)
├── fecha_firma: DATE
├── fecha_inicio: DATE
├── fecha_vencimiento: DATE
├── modelo_revenue: JSON (caute, %, usuario, expediente, híbrido)
├── limite_caute: INT (expedientes)
├── porcentaje_recaudo: DECIMAL (%)
├── valor_total_cop: DECIMAL
├── estado: ENUM (borrador, en_revisión, activo, vencido, cancelado)
└── audit_log_id: UUID (FK → audit_log)
```

### 2. feature_flags

```sql
TABLE feature_flags
├── id: UUID (PK)
├── tenant_id: UUID (FK)
├── protected_resource_id: UUID (FK)
├── activo: BOOLEAN
├── fecha_activacion: TIMESTAMP
├── fecha_desactivacion: TIMESTAMP
├── activado_por: UUID (FK → usuarios)
├── razon: TEXT ("renovación anual", "upgrade de plan", "escalado automático")
└── audit_log_id: UUID (FK)
```

### 3. mfa_credentials

```sql
TABLE mfa_credentials
├── id: UUID (PK)
├── user_id: UUID (FK)
├── secret: STRING (TOTP secret, AES-256-GCM encrypted)
├── habilitado: BOOLEAN
├── intentos_fallidos: INT (para lockout después de 5)
├── ultimo_uso: TIMESTAMP
├── backup_codes: ARRAY<STRING> (para recuperación)
├── fecha_creacion: TIMESTAMP
└── audit_log_id: UUID (FK)
```

### 4. sdk_metrics

```sql
TABLE sdk_metrics
├── id: UUID (PK)
├── tenant_id: UUID (FK)
├── resource_id: STRING (LIQ-001, DOC-003, etc)
├── execution_time_ms: INT
├── tokens_used: INT
├── cost_usd: DECIMAL (tokens × tarifa)
├── success: BOOLEAN
├── timestamp: TIMESTAMP
└── INDEX (tenant_id, timestamp)
```

### 5. audit_log

```sql
TABLE audit_log
├── id: UUID (PK)
├── tenant_id: UUID (FK)
├── usuario_id: UUID (FK)
├── tabla_afectada: STRING (contracts, feature_flags, etc)
├── registro_id: UUID (qué registro cambió)
├── operacion: ENUM (INSERT, UPDATE, DELETE)
├── valores_anterior: JSONB (antes)
├── valores_nuevo: JSONB (después)
├── timestamp: TIMESTAMP
├── ip_address: STRING
└── razon: TEXT (auditoría manual)
```

## Performance y Escalabilidad

### Para 50M Liquidaciones/Mes

**Estrategia:**
- Particionar tabla `liquidaciones` por mes
- Índice en (tenant_id, estado, fecha_creacion)
- Caché Redis de queries frecuentes
- Archivado de datos >2 años

```sql
CREATE TABLE liquidaciones 
  PARTITION BY RANGE (DATE_TRUNC('month', fecha_creacion))

CREATE TABLE liquidaciones_202604 PARTITION OF liquidaciones
  FOR VALUES FROM ('2026-04-01') TO ('2026-05-01');
```

### Para Multi-Tenant

**Estrategia:**
- BD separada por tenant (no rows-level-security)
- Conexión routing en capa API
- Backups independientes
- Escalamiento independiente

```python
# En API
tenant_id = request.headers['X-Tenant-ID']
db_connection = get_tenant_db(tenant_id)
```

## Evolución Futura (Sin Refactorización)

BD diseñada al 90% permite:
- ✅ Agregar nuevas funcionalidades sin cambios mayores
- ✅ Agregar campos en tablas existentes (ALTER TABLE)
- ✅ Crear nuevas tablas para nuevos módulos
- ✅ Cambiar modelos de pricing sin afectar histórico
- ✅ Escalado a 500M records/mes solo con mejoras de índices

## Checklist para Diseño de BD

- [ ] Casos de uso documentados por módulo
- [ ] Iteración con ChatGPT completada (90%)
- [ ] Alembic migrations definidas
- [ ] Índices críticos identificados
- [ ] Particiones planificadas
- [ ] Tablas de auditoría implementadas
- [ ] MFA integrada en endpoints críticos
- [ ] SDK Métricas en cada tabla principal
- [ ] Backups y recuperación probados
- [ ] Validación contra todos los CUs

---
**Versión**: 1.0 | **Creado**: 2026-04-30
