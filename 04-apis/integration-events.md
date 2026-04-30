# Eventos de Integración

## Overview

JikkoOps emite eventos para que otros sistemas (SILIN, DOS, SOCIA, sistemas externos) se enteres de cambios y actúen en consecuencia.

## Arquitectura de Eventos

```
Evento ocurre en JikkoOps
        ↓
Message Queue (RabbitMQ / Kafka)
        ↓
Suscriptores:
├── SILIN (liquidación)
├── DOS (documentos)
├── SOCIA (ciudadano)
├── Webhooks externos
└── Auditoría
```

## Eventos Soportados

### 1. contract.activated

Se emite cuando un contrato se activa y comienza a servir.

```json
{
    "event_type": "contract.activated",
    "event_id": "evt-uuid",
    "timestamp": "2026-04-29T10:30:00Z",
    "data": {
        "contract_id": "uuid",
        "contract_numero": "CONTRATO-2026-CALI-001",
        "tenant_id": "uuid",
        "tenant_nombre_tecnico": "municipio_cali",
        "entity_nombre": "Municipio de Cali",
        "plan_id": "uuid",
        "plan_nombre": "Plan Estándar",
        "servicios_activados": ["SILIN", "DOS"],
        "protected_resources_activados": [
            "LIQ-001", "LIQ-004", "DOC-001", "DOC-003"
        ],
        "fecha_vigencia_inicio": "2026-05-01",
        "fecha_vigencia_fin": "2027-05-01",
        "modelo_revenue": {
            "tipo": "CAUTE",
            "periodo_modelo-ingresos_dias": 180
        }
    }
}
```

**Destinatarios**: SILIN, DOS, SOCIA (para inicializar)

**Acción esperada**:
- SILIN: Crear tenant y configurar liquidación
- DOS: Crear espacios documentales
- SOCIA: Crear usuario administrador

### 2. contract.deactivated

Se emite cuando un contrato vence o es cancelado.

```json
{
    "event_type": "contract.deactivated",
    "event_id": "evt-uuid",
    "timestamp": "2026-04-29T16:00:00Z",
    "data": {
        "contract_id": "uuid",
        "contract_numero": "CONTRATO-2025-CALI-001",
        "tenant_id": "uuid",
        "razon": ["vencimiento" | "cancelacion_cliente" | "cancelacion_mora"],
        "fecha_efectiva": "2026-05-01",
        "datos_archivo": {
            "expedientes_pendientes": 12,
            "recaudos_pendientes": 1500000
        }
    }
}
```

**Acción esperada**:
- SILIN: Marcar tenant como inactivo, pausar liquidaciones
- DOS: Hacer backup de documentos
- SOCIA: Bloquear acceso de usuarios
- Auditoría: Archivar datos

### 3. escalado_volumen.detectado

Se emite cuando el tenant supera un límite y cambia el modelo de revenue.

```json
{
    "event_type": "escalado_volumen.detectado",
    "event_id": "evt-uuid",
    "timestamp": "2026-04-29T08:15:00Z",
    "data": {
        "tenant_id": "uuid",
        "tenant_nombre": "municipio_cali",
        "evento": "modelo-ingresos_completado",
        "estadistica_anterior": {
            "modelo": "CAUTE",
            "expedientes_limite": 1000,
            "expedientes_procesados": 1050
        },
        "estadistica_nueva": {
            "modelo": "CAUTE_THEN_PERCENTAGE",
            "porcentaje_recaudo": 0.10,
            "minimo_mensual": 50000,
            "expedientes_procesados": 1050
        },
        "fecha_efectiva": "2026-04-30T00:00:00Z",
        "impacto_financiero": {
            "expedientes_excedentes": 50,
            "costo_expediente_extra": 200,
            "factura_mes_siguiente": 50000
        }
    }
}
```

**Acción esperada**:
- JikkoOps: Activar flags de "% de recaudo"
- SILIN: Preparar cálculo de porcentaje de recaudos
- Notificación: Enviar email al cliente sobre cambio de modelo

### 4. flags.changed

Se emite cuando se activan o desactivan Protected Resources.

```json
{
    "event_type": "flags.changed",
    "event_id": "evt-uuid",
    "timestamp": "2026-04-29T12:00:00Z",
    "data": {
        "tenant_id": "uuid",
        "cambios": [
            {
                "codigo_recurso": "SOC-002",
                "nombre": "Notificaciones digitales",
                "tipo": "action",
                "modulo": "SOCIA",
                "accion": "activado",
                "razon": "Upgrade a Plan Premium"
            },
            {
                "codigo_recurso": "REP-002",
                "nombre": "Reportes avanzados",
                "tipo": "view",
                "modulo": "SILIN",
                "accion": "activado",
                "razon": "Upgrade a Plan Premium"
            }
        ],
        "total_cambios": 2,
        "activados": 2,
        "desactivados": 0,
        "fecha_efectiva": "2026-05-01T00:00:00Z"
    }
}
```

**Acción esperada**:
- SILIN/DOS/SOCIA: Actualizar su caché de flags
- UI: Mostrar/ocultar botones según nuevos flags
- Auditoría: Registrar quién y por qué se activaron

### 5. invoice.created

Se emite cuando se genera una factura.

```json
{
    "event_type": "invoice.created",
    "event_id": "evt-uuid",
    "timestamp": "2026-05-01T09:00:00Z",
    "data": {
        "invoice_id": "uuid",
        "invoice_numero": "FACT-2026-05-CALI",
        "tenant_id": "uuid",
        "periodo_inicio": "2026-04-01",
        "periodo_fin": "2026-04-30",
        "fecha_emision": "2026-05-01",
        "fecha_vencimiento": "2026-05-15",
        "subtotal": 110000,
        "iva": 20900,
        "total": 130900,
        "lineas": [
            {
                "descripcion": "Expedientes procesados (550 x $200)",
                "cantidad": 550,
                "precio_unitario": 200,
                "subtotal": 110000,
                "origen": "expediente"
            }
        ],
        "documento_pdf_url": "https://jikkoops.internal/facturas/uuid.pdf",
        "forma_pago": "transferencia"
    }
}
```

**Acción esperada**:
- SILIN: Registrar factura en sistema de cobro
- Email: Enviar factura al cliente
- Contabilidad: Registrar en sistema de ingresos
- Auditoría: Log de generación

### 6. invoice.paid

Se emite cuando se registra un pago de factura.

```json
{
    "event_type": "invoice.paid",
    "event_id": "evt-uuid",
    "timestamp": "2026-05-10T14:30:00Z",
    "data": {
        "invoice_id": "uuid",
        "invoice_numero": "FACT-2026-05-CALI",
        "tenant_id": "uuid",
        "fecha_pago": "2026-05-10",
        "monto_pagado": 130900,
        "referencia_transaccion": "TRX-123456789",
        "medio_pago": "transferencia_bancaria",
        "banco": "Banco de Occidente"
    }
}
```

**Acción esperada**:
- Contabilidad: Marcar factura como pagada
- Email: Enviar recibido de pago al cliente
- Auditoría: Log de pago

### 7. tenant.vencimiento_cercano

Se emite 30, 15 y 7 días antes del vencimiento del contrato.

```json
{
    "event_type": "tenant.vencimiento_cercano",
    "event_id": "evt-uuid",
    "timestamp": "2026-04-04T08:00:00Z", // 30 días antes
    "data": {
        "tenant_id": "uuid",
        "contract_numero": "CONTRATO-2026-CALI-001",
        "fecha_vencimiento": "2027-05-01",
        "dias_restantes": 30,
        "recordatorio_numero": 1,
        "ultima_renovacion": "2026-01-15",
        "contacto_renewal": {
            "nombre": "Juan Pérez",
            "email": "juan@cali.gov.co",
            "telefono": "+57..."
        }
    }
}
```

**Acción esperada**:
- Email: Recordatorio al contacto de renovación
- JikkoOps: Crear ticket de follow-up comercial
- CRM: Marcar como "a contactar"

### 8. user.created

Se emite cuando se crea un nuevo usuario administrativo en un tenant.

```json
{
    "event_type": "user.created",
    "event_id": "evt-uuid",
    "timestamp": "2026-04-29T11:30:00Z",
    "data": {
        "user_id": "uuid",
        "tenant_id": "uuid",
        "email": "admin@cali.gov.co",
        "nombre": "Carlos López",
        "roles": ["admin"],
        "permiso_para_modificar_revenue": false,
        "modulos_acceso": ["SILIN", "DOS", "SOCIA"]
    }
}
```

**Acción esperada**:
- IAM (SIGIA): Crear usuario en sistema de autenticación
- Email: Enviar credenciales y welcome message
- Auditoría: Log de creación

### 9. protected_resource.inventoried

Se emite cuando un nuevo Protected Resource es inventariado (generalmente en deploy).

```json
{
    "event_type": "protected_resource.inventoried",
    "event_id": "evt-uuid",
    "timestamp": "2026-04-29T15:45:00Z",
    "data": {
        "codigo": "SOC-005",
        "nombre": "Búsqueda avanzada de expedientes",
        "tipo": "view",
        "modulo": "SOCIA",
        "criticidad": "media",
        "estado": "huerfano",
        "razon_inventario": "Deploy 2026-04-29",
        "requiere_agrupacion": true
    }
}
```

**Acción esperada**:
- JikkoOps: Notificar a manager sobre recurso huérfano
- Auditoría: Registrar nuevo recurso

## Almacenamiento y Retención de Eventos

```
Event Store (Event Sourcing):
├── Almacenamiento: Kafka / EventStoreDB
├── Retención: 12 meses
├── Replicación: 3 réplicas (HA)
├── Compresión: Después de 3 meses
└── Backup: Diario, retención 7 años
```

## Consumidor de Eventos - Ejemplo

```javascript
// Node.js con RabbitMQ

import amqp from 'amqplib';

const QUEUE = 'jikkoops.events';

async function consumeEvents() {
    const connection = await amqp.connect('amqp://rabbitmq:5672');
    const channel = await connection.createChannel();
    
    await channel.assertQueue(QUEUE, { durable: true });
    
    channel.consume(QUEUE, async (msg) => {
        const event = JSON.parse(msg.content.toString());
        
        console.log(`Event: ${event.event_type}`, event.data);
        
        switch(event.event_type) {
            case 'contract.activated':
                await handleContractActivated(event.data);
                break;
            case 'escalado_volumen.detectado':
                await handleEscalado(event.data);
                break;
            case 'flags.changed':
                await handleFlagsChanged(event.data);
                break;
            // ... más handlers
        }
        
        // Acknowledge
        channel.ack(msg);
    });
}

async function handleContractActivated(data) {
    // Inicializar tenant en SILIN, DOS, SOCIA
    console.log(`Activando servicios para ${data.entity_nombre}`);
    
    // Llamar APIs de módulos
    await fetch(`https://cilin.internal/api/v1/tenants`, {
        method: 'POST',
        body: JSON.stringify({
            tenant_id: data.tenant_id,
            plan: data.plan_nombre
        })
    });
}

consumeEvents().catch(console.error);
```

## Dead Letter Queue (DLQ)

Si un evento falla múltiples veces, se envía a DLQ para análisis:

```
Dead Letter Queue:
├── Almacenamiento: jikkoops.events.dlq
├── Retención: 30 días
├── Monitoreo: Alertas automáticas
└── Manual recovery: Manager puede reintentar
```

## Garantías de Entrega

```
Orden: ✓ Por tenant (eventos del mismo tenant en orden)
       ✗ Globalmente (eventos de diferentes tenants pueden no estar ordenados)

Entrega: At least once
         Suscriptor debe ser idempotente (si procesa 2x, OK)

Timeout: 30 segundos para procesar evento
         Si falla, se reintenta hasta 3 veces
```
