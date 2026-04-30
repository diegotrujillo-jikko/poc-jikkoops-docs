# Modelo de Datos de JikkoOps

## Entidades Principales

### 1. Cliente (Entity)

```sql
TABLE entities
├── id: UUID (PK)
├── nombre: STRING
├── nit: STRING (unique)
├── tipo: ENUM ['municipio', 'gobernacion', 'dian', 'otro']
├── region: STRING (departamento)
├── email: STRING
├── telefono: STRING
├── direccion: TEXT
├── estado: ENUM ['activo', 'inactivo', 'cancelado']
├── fecha_creacion: TIMESTAMP
├── fecha_actualizacion: TIMESTAMP
└── metadata: JSON (datos adicionales)
```

### 2. Tenant (Instancia de cliente en JikkoOps)

```sql
TABLE tenants
├── id: UUID (PK)
├── entity_id: UUID (FK → entities)
├── nombre_tecnico: STRING (unique) -- e.g., 'municipio_cali'
├── base_de_datos: STRING (conexión DB del tenant)
├── region: STRING
├── plan_id: UUID (FK → plans)
├── usuarios_limite: INT
├── expedientes_mes_limite: INT
├── estado: ENUM ['activo', 'inactivo', 'en_prueba']
├── fecha_activacion: DATE
├── fecha_vencimiento: DATE
├── metadata: JSON (config técnica)
└── feature_flags: JSONB (flags activos)
```

### 3. Contrato

```sql
TABLE contracts
├── id: UUID (PK)
├── tenant_id: UUID (FK → tenants)
├── numero: STRING (unique) -- e.g., 'CONTRATO-2026-CALI-001'
├── fecha_firma: DATE
├── fecha_inicio: DATE
├── fecha_vencimiento: DATE
├── tipo_contrato: ENUM ['principal', 'renovacion', 'enmienda']
├── estado: ENUM ['borrador', 'en_revision', 'activo', 'vencido', 'cancelado']
├── servicios: ARRAY<UUID> (referencias a servicios/módulos)
├── modelo_revenue: JSON (configuración de pricing)
├── valor_total_cop: DECIMAL
├── forma_pago: ENUM ['transferencia', 'tdc', 'debito']
├── documento_pdf: BYTEA (contrato escaneado)
├── fecha_creacion: TIMESTAMP
└── fecha_actualizacion: TIMESTAMP
```

### 4. Producto (Combination de funcionalidades)

```sql
TABLE products
├── id: UUID (PK)
├── nombre: STRING
├── descripcion: TEXT
├── funcionalidades: ARRAY<UUID> (referencias a features)
├── protected_resources: ARRAY<UUID> (recursos incluidos)
├── estado: ENUM ['borrador', 'activo', 'deprecated']
├── fecha_creacion: TIMESTAMP
└── fecha_actualizacion: TIMESTAMP
```

### 5. Plan (Oferta comercial)

```sql
TABLE plans
├── id: UUID (PK)
├── nombre: STRING -- e.g., 'Plan Básico', 'Plan Estándar'
├── descripcion: TEXT
├── producto_id: UUID (FK → products)
├── funcionalidades: ARRAY<UUID> (features de este plan)
├── protected_resources: ARRAY<UUID> (recursos disponibles)
├── usuario_limite: INT (NULL = ilimitado)
├── expediente_limite_mes: INT (NULL = ilimitado)
├── precio_fijo: DECIMAL (NULL si no aplica)
├── modelo_revenue: JSON (modelo-ingresos, %, usuario, expediente, etc.)
├── estado: ENUM ['activo', 'deprecated']
├── fecha_creacion: TIMESTAMP
└── fecha_actualizacion: TIMESTAMP
```

### 6. Funcionalidad (Feature / Grupo de recursos)

```sql
TABLE features
├── id: UUID (PK)
├── nombre: STRING -- e.g., 'Liquidación', 'Expedientes'
├── descripcion: TEXT
├── modulo: ENUM ['SILIN', 'DOS', 'SOCIA', 'IAM']
├── protected_resources: ARRAY<UUID> (recursos que componen esta feature)
├── criticidad: ENUM ['critica', 'alta', 'media', 'baja']
├── estado: ENUM ['activo', 'beta', 'deprecated']
└── fecha_creacion: TIMESTAMP
```

### 7. Protected Resource (Recurso inventariado)

```sql
TABLE protected_resources
├── id: UUID (PK)
├── codigo: STRING (unique) -- e.g., 'LIQ-001'
├── nombre: STRING
├── tipo: ENUM ['button', 'endpoint', 'view', 'action']
├── modulo: ENUM ['SILIN', 'DOS', 'SOCIA', 'IAM']
├── feature_id: UUID (FK → features, NULL si huérfano)
├── descripcion: TEXT
├── criticidad: ENUM ['critica', 'alta', 'media', 'baja']
├── dependencias: ARRAY<STRING> (códigos de recursos que requiere)
├── estado: ENUM ['activo', 'huerfano', 'deprecated']
├── fecha_creacion: TIMESTAMP
└── fecha_actualizacion: TIMESTAMP
```

### 8. Tenant Entitlement (Qué tiene cada tenant activado)

```sql
TABLE tenant_entitlements
├── id: UUID (PK)
├── tenant_id: UUID (FK → tenants)
├── funcionalidad_id: UUID (FK → features)
├── protectedresource_id: UUID (FK → protected_resources)
├── estado: ENUM ['activo', 'inactivo']
├── fecha_activacion: DATE
├── fecha_vencimiento: DATE (NULL si permanente)
├── motivo: STRING (ej: 'renovacion anual', 'upgrade de plan')
└── aprobado_por: UUID (FK → usuarios)
```

### 9. Factura

```sql
TABLE invoices
├── id: UUID (PK)
├── tenant_id: UUID (FK → tenants)
├── numero_factura: STRING (unique) -- e.g., 'FACT-2026-04-CALI'
├── periodo_inicio: DATE
├── periodo_fin: DATE
├── fecha_emision: DATE
├── fecha_vencimiento: DATE
├── estado: ENUM ['borrador', 'emitida', 'pagada', 'vencida', 'anulada']
├── lineas: ARRAY<JSON> (líneas de facturación detalladas)
│   └── cada línea:
│       ├── descripcion: STRING
│       ├── cantidad: DECIMAL
│       ├── precio_unitario: DECIMAL
│       ├── subtotal: DECIMAL
│       ├── origen: ENUM ['expediente', 'usuarios', 'recaudo', 'fijo']
│       └── validacion: JSON (auditoría)
├── subtotal: DECIMAL
├── iva: DECIMAL
├── descuentos: DECIMAL
├── total: DECIMAL
├── documento_pdf: BYTEA
├── enviado_al_cliente: TIMESTAMP
├── fecha_pago: DATE (NULL si no pagada)
└── fecha_actualizacion: TIMESTAMP
```

### 10. Feature Flag (Control en runtime)

```sql
TABLE feature_flags
├── id: UUID (PK)
├── tenant_id: UUID (FK → tenants)
├── protected_resource_id: UUID (FK → protected_resources)
├── codigo_recurso: STRING (denorm para performance)
├── activo: BOOLEAN
├── fecha_activacion: TIMESTAMP
├── fecha_desactivacion: TIMESTAMP (NULL si activo)
├── activado_por: UUID (FK → usuarios)
├── razon: TEXT (auditoría)
└── metadata: JSON
```

## Índices Críticos

```sql
-- Performance de consultas frecuentes
CREATE INDEX idx_tenants_entity ON tenants(entity_id);
CREATE INDEX idx_contracts_tenant ON contracts(tenant_id);
CREATE INDEX idx_invoices_tenant_periodo ON invoices(tenant_id, periodo_inicio, periodo_fin);
CREATE INDEX idx_feature_flags_tenant_activo ON feature_flags(tenant_id, activo);
CREATE INDEX idx_protected_resources_codigo ON protected_resources(codigo);
CREATE INDEX idx_protected_resources_feature ON protected_resources(feature_id);
```

## Relaciones Clave

```
Entity (Cliente físico)
  ├─→ Tenants (1..N) - instancia en JikkoOps
  │    ├─→ Contracts (1..N) - acuerdos de servicio
  │    │    └─→ Facturas (1..N) - documentos de facturación
  │    ├─→ Plan (1..1) - plan contratado
  │    │    └─→ Funcionalidades (1..N)
  │    │         └─→ Protected Resources (1..N)
  │    └─→ Tenant Entitlements (1..N) - qué está activo
  │         └─→ Feature Flags (1..1 por recurso) - control runtime
  │
  └─→ Contactos (1..N) - usuarios administrativos
       └─→ Roles (1..N) - permisos en JikkoOps
```

## Vistas Útiles

```sql
-- Vista: Qué tiene cada tenant activo
VIEW tenant_active_resources AS
SELECT 
    te.tenant_id,
    pr.codigo,
    pr.nombre,
    f.nombre as funcionalidad,
    ff.activo,
    ff.fecha_activacion
FROM tenant_entitlements te
JOIN protected_resources pr ON te.protectedresource_id = pr.id
JOIN features f ON pr.feature_id = f.id
LEFT JOIN feature_flags ff ON te.tenant_id = ff.tenant_id 
                              AND te.protectedresource_id = ff.protected_resource_id
WHERE te.estado = 'activo'
ORDER BY te.tenant_id, f.nombre, pr.codigo;

-- Vista: Revenue por contrato (próximo mes)
VIEW projected_revenue AS
SELECT 
    c.id as contract_id,
    t.id as tenant_id,
    e.nombre as cliente,
    c.numero_contrato,
    c.modelo_revenue,
    (c.modelo_revenue->>'valor_proyectado')::DECIMAL as revenue_proyectado,
    c.fecha_vencimiento,
    (c.fecha_vencimiento - NOW()::DATE) as dias_para_vencer
FROM contracts c
JOIN tenants t ON c.tenant_id = t.id
JOIN entities e ON t.entity_id = e.id
WHERE c.estado = 'activo'
ORDER BY dias_para_vencer ASC;
```

## Consideraciones de Almacenamiento

### Datos Sensibles
- Contratos: Almacenar versión PDF protegida
- Facturas: Auditar acceso
- Configuración de DB/API: Cifrar en reposo
- Logs de cambios: Retener 7 años (requisito fiscal)

### Backups
- Diario de BD completa
- Retención: 30 días (últimos 30 backups)
- Backup mensual archivado: 7 años

### GDPR / Privacidad
- Datos de contacto: Permitir anonimización
- Datos de operación: Retención según regulación local
- Derechos de acceso: Implementar endpoint para exports
