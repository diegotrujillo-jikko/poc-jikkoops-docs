# Riesgos Identificados en JikkoOps

## Matriz de Riesgos

| # | Riesgo | Probabilidad | Impacto | Severidad | Dueño | Mitigación |
|---|--------|--------------|---------|-----------|-------|------------|
| 1 | Desincronización de flags entre BD y aplicación | Media | Alto | 🔴 CRÍTICO | DevOps | Health check c/5min |
| 2 | Error en cálculo de revenue share | Baja | Crítico | 🔴 CRÍTICO | CFO | Auditoría mensual |
| 3 | Módulo SILIN/DOS/SOCIA caído | Media | Alto | 🔴 CRÍTICO | Operación | SLA 99.9%, fallover |
| 4 | Pérdida de datos en BD | Muy baja | Crítico | 🔴 CRÍTICO | DBA | Backup diario + test restore |
| 5 | Escalado de volumen no detectado | Baja | Medio | 🟡 ALTO | Operación | Job automático c/hora |
| 6 | Cliente intenta acceder post-vencimiento | Medio | Bajo | 🟡 ALTO | Operación | Bloqueo automático en vencimiento |
| 7 | Factura genera con datos errados | Media | Medio | 🟡 ALTO | Accountant | Validación antes de emitir |
| 8 | Cambio no autorizado en cálculos revenue | Baja | Crítico | 🔴 CRÍTICO | Legal | Code review + audit log |
| 9 | Saturación de caché (Redis down) | Baja | Medio | 🟡 ALTO | DevOps | Fallback a BD + alertas |
| 10 | Notificación/email no llega a cliente | Media | Bajo | 🟢 MEDIO | Operación | Log de intentos + reintento |

## 1. Desincronización de Flags

**Descripción**: Feature flags en BD no coinciden con lo que debería estar según plan.

**Causa posible**:
- Error en sincronización automática
- Actualización de plan sin actualizar flags
- Bug en lógica de escalado

**Impacto**:
- Usuario ve/usa funcionalidades no contratadas
- O no ve funcionalidades que sí tiene contratadas
- Problemas de facturación

**Mitigación**:
- ✓ Health check cada 5 minutos (detecta desincronización)
- ✓ Resincronización manual disponible
- ✓ Auditoría de cambios de flags
- ✓ Alertas automáticas si discrepancia > 10%

**Acción si ocurre**:
1. Manager ve alerta en Operación
2. Click "Resincronizar"
3. Sistema valida y actualiza
4. Envía notificación a cliente si afectado

## 2. Error en Cálculo de Revenue Share

**Descripción**: Factura se genera con cantidad errónea de expedientes, usuarios, o % recaudo.

**Causa posible**:
- Sincronización fallida con SILIN
- Bug en fórmula de cálculo
- Datos inconsistentes entre sistemas

**Impacto**:
- JikkoOps factura menos/más de lo debido
- Conflicto con cliente
- Error contable

**Mitigación**:
- ✓ Validación de integridad de datos antes de facturas
- ✓ Comparación con mes anterior (alerta si >50% diferencia)
- ✓ Revisión manual de facturas en borrador antes de emitir
- ✓ Auditoría independiente mensual
- ✓ No permitir cambios en cálculos sin aprobación

**Acción si ocurre**:
1. Manager nota discrepancia en factura (estado: Borrador)
2. Click "Resincronizar datos" (recolecta de SILIN)
3. Revisa líneas y cantidades
4. Emite o anula según corresponda

## 3. Módulo SILIN/DOS/SOCIA Caído

**Descripción**: Uno de los módulos integrados no responde (timeout, error HTTP 500, etc).

**Causa posible**:
- Error en código del módulo
- BD lenta/caída
- Problema de red
- Sobrecarga

**Impacto**:
- Tenant no puede liquidar (SILIN)
- Tenant no puede crear documentos (DOS)
- Tenant no puede notificar (SOCIA)
- JikkoOps no puede facturas (si no puede confirmar expedientes)

**Mitigación**:
- ✓ Health check cada 5 minutos
- ✓ SLA 99.9% (máx 45 min/mes downtime)
- ✓ Failover automático si disponible
- ✓ Alertas inmediatas a equipo de DevOps
- ✓ Modo degradado para algunas operaciones

**Acción si ocurre**:
1. JikkoOps detecta módulo caído (health check)
2. Alerta a DevOps inmediatamente
3. Operación: Notifica a cliente ("Servicio temporal indisponible")
4. DevOps: Intenta recuperación automática
5. Si persiste >15 min: Escalada a architect
6. Después de resolver: Root cause analysis

## 4. Pérdida de Datos en BD

**Descripción**: Datos de contratos, facturas o configuración se pierden por error.

**Causa posible**:
- Fallo en backup
- Error en restore
- Corrupción de BD
- Ataque/ransomware

**Impacto**:
- Crítico: Pérdida de evidencia de contratos
- Imposible reconstruir facturación
- Potencial demanda legal

**Mitigación**:
- ✓ Backup automático diario
- ✓ Retención de 30 backups en línea + archivos (7 años)
- ✓ Replicación en 3 locations
- ✓ Test de restore semanal (automático)
- ✓ Encriptación en reposo
- ✓ DBA on-call 24/7

**Acción si ocurre** (es una emergencia):
1. Detener todos los writes a BD
2. Contactar DBA de inmediato
3. Evaluar último backup válido
4. Restaurar de backup
5. Validar integridad de datos
6. Post-incident review

## 5. Escalado de Volumen No Detectado

**Descripción**: Tenant supera límite modelo-ingresos pero el sistema no detecta ni cambia modelo de revenue.

**Causa posible**:
- Job de escalado falla silenciosamente
- Sincronización de datos con SILIN retrasada
- Lógica de detección bug

**Impacto**:
- Tenant continúa sin pagar (pasada la fecha modelo-ingresos)
- JikkoOps pierde ingresos
- Errores en facturación

**Mitigación**:
- ✓ Job automático cada 1 hora (redundante)
- ✓ Manual check diario por Operations Manager
- ✓ Dashboard muestra expedientes procesados vs límite
- ✓ Alerta si expedientes > 95% límite modelo-ingresos
- ✓ Email proactivo a cliente cuando se acerca límite

**Acción si ocurre**:
1. Manager ve en Dashboard que expedientes > límite
2. Click "Aplicar escalado" (manual)
3. Sistema activa flags y genera enmienda
4. Ajusta próxima facturación

## 6. Cliente Intenta Acceder Post-Vencimiento

**Descripción**: Contrato vence pero cliente sigue intentando acceder.

**Causa posible**:
- Bloqueo automático no funcionó
- Manager no marcó como vencido
- Sincronización de flags falló

**Impacto**:
- Cliente puede usar servicios no pagados
- Confusión sobre estado del servicio
- Riesgo legal

**Mitigación**:
- ✓ Bloqueo automático en fecha_vencimiento
- ✓ Email al cliente 30, 15, 7 días antes
- ✓ Acceso deshabilitado automáticamente al vencer
- ✓ Dashboard aviso: "Contrato vencido, contacte comercial"

**Acción si ocurre**:
1. Cliente reporta acceso denegado
2. Manager verifica fecha vencimiento
3. Si vencido: Ofrece renovación
4. Si renovación → Reactivar contrato

## 7. Factura Genera con Datos Errados

**Descripción**: Factura se emite con cantidades, precios o clientes incorrectos.

**Causa posible**:
- Manager edita factura en borrador incorrectamente
- Sincronización de datos falla antes de facturas
- Error en lógica de cálculo

**Impacto**:
- Cliente reclama facturación
- Potencial no pago
- Retrasos en contabilidad

**Mitigación**:
- ✓ Validación antes de emitir (datos consistentes)
- ✓ Comparación con mes anterior (alerta si >50% diferencia)
- ✓ Resincronizar datos disponible antes de emitir
- ✓ Revisión manual obligatoria en estado Borrador
- ✓ Auditoría de cambios en líneas

**Acción si ocurre**:
1. Manager nota error antes de emitir (está en Borrador)
2. Click "Resincronizar datos"
3. Revisa cantidades nuevamente
4. Emite o anula

## 8. Cambio No Autorizado en Cálculos Revenue

**Descripción**: Alguien modifica fórmula de cálculo sin autorización.

**Causa posible**:
- Acceso sin controlar a código
- Developer hace cambio por error
- Ataque interno

**Impacto**:
- Cambio en cómo se calcula revenue
- Posible fraude interno
- Problemas de compliance

**Mitigación**:
- ✓ Code review obligatorio para cambios en revenue
- ✓ Audit log de cambios en configuración
- ✓ Alerta si se modifica modelo de revenue
- ✓ Permisos restringidos (solo CFO + CTO)

**Acción si ocurre** (requiere investigación):
1. Detectar cambio no autorizado (audit log)
2. Revertir cambio a versión anterior
3. Investigación de quién hizo cambio
4. Revisar si hubo impacto en facturas
5. Post-incident: Mejorar controles

## 9. Saturación de Caché (Redis Down)

**Descripción**: Redis (caché) se cae o queda sin espacio.

**Causa posible**:
- Demasiados datos cacheados
- Fuga de memoria en Redis
- Hardware insuficiente

**Impacto**:
- Performance degradada (todo va a BD)
- Usuarios ven respuestas lentas
- Riesgo de timeout en módulos

**Mitigación**:
- ✓ Fallback automático a BD
- ✓ Política de eviction LRU
- ✓ Limite de memory (2GB)
- ✓ Alertas si memory > 80%
- ✓ Replicación de Redis

**Acción si ocurre**:
1. Sistema detecta Redis no disponible
2. Automático: Usa BD directamente (más lento, pero funciona)
3. Alerta a DevOps: "Redis issue, fallback active"
4. DevOps: Investiga y recupera Redis
5. Performance vuelve a normal automáticamente

## 10. Email/Notificación No Llega

**Descripción**: Email de factura, alerta, renovación no llega al cliente.

**Causa posible**:
- Proveedor de email caído
- Email filtrado como spam
- Dirección de email incorrecta
- Error en plantilla

**Impacto**:
- Cliente no se entera de vencimiento (bajo)
- Cliente no recibe factura (medio)
- Cliente pierde acceso sin aviso (alto)

**Mitigación**:
- ✓ Log de intentos de email
- ✓ Reintento automático (3 veces)
- ✓ Alerta si email falla
- ✓ Fallback: SMS o aviso en dashboard
- ✓ Validación de email antes de enviar

**Acción si ocurre**:
1. Sistema log: Email falla
2. Reintenta automáticamente cada hora (hasta 3 veces)
3. Si sigue fallando: Alerta a Operations Manager
4. Manager: Contacta al cliente por teléfono
5. Verifica email correcto en sistema

## Seguimiento de Riesgos

```
Revisión de riesgos: Trimestral
├── Actualizar probabilidades según trending
├── Revisar mitigaciones implementadas
├── Identificar nuevos riesgos
└── Ajustar dueños y acciones

Reporte a Junta: Mensualmente
├── Riesgos nuevos
├── Cambios en severidad
├── Incidentes ocurridos relacionados
└── Recomendaciones
```
