# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**JikkoOps** is a modular back-office system for intelligent governance (SIGIA ecosystem). It enables municipalities, gobernaciones, and public entities to:

- **Commercialize services**: Create, manage, and sell digital government services as customizable plans
- **Control access**: Use Protected Resources and feature flags to activate/deactivate functionality by tenant and plan
- **Monetize flexibly**: Support multiple revenue models (modelo-ingresos, percentage, per-user, per-expedient, hybrid)
- **Integrate seamlessly**: Sync with SILIN (liquidation), DOCS (documents), SOCIA (citizen services), and external systems
- **Audit comprehensively**: Maintain immutable logs of all changes for compliance

## Architecture at a Glance

```
User Requests
    ↓
JikkoOps API (REST)
    ├─→ Protected Resources (inventory of features)
    ├─→ Feature Flags (runtime activation per tenant)
    ├─→ Plans (commercial offerings)
    └─→ Tenant Entitlements (what each client has)
    ↓
DB (PostgreSQL per tenant) + Cache (Redis) + Message Queue (Events)
    ├─→ SILIN (liquidation integration)
    ├─→ DOCS (document management integration)
    ├─→ SOCIA (citizen services integration)
    └─→ External webhooks
```

**Key Concept**: JikkoOps doesn't compute tax/liquidation logic—it orchestrates *which* clients get *which* functionality and *how* to charge them.

## Project Structure

```
poc-jikkoops-docs/
├── 00-general/                    # Entry docs, transcripts, PDFs
│
├── 01-arquitectura/               # System design & concepts
│   ├── vision-general.md          # Overview of JikkoOps architecture
│   └── protected-resources.md     # Feature inventory and activation system
│
├── 02-procesos/                   # Business processes
│   ├── flujo-contratos.md         # Full customer lifecycle (prospect → renewal)
│   └── revenue-share-models.md    # Monetization strategies (modelo-ingresos, %, users, expedients, hybrid)
│
├── 03-datos/                      # Data layer
│   ├── data-model.md              # Entity relationship diagram + SQL schema
│   ├── feature-flags-sync.md      # How flags sync between DB, cache, and runtime
│   └── cache-strategy.md          # Multi-level caching (browser, gateway, Redis, DB)
│
├── 04-apis/                       # Integration & endpoints
│   ├── endpoints-principales.md   # REST API reference (products, plans, tenants, flags, invoices, etc.)
│   └── integration-events.md      # Event types emitted to SILIN, DOS, SOCIA, webhooks
│
├── 05-modulos-core/               # Core UI modules
│   ├── dashboard-comercial.md     # Sales dashboard (KPIs, renewals, pipeline)
│   ├── gestion-contratos.md       # Contract lifecycle (create, review, activate, monitor)
│   ├── operacion.md               # Operations (health, flags, users, alerts, escalados)
│   ├── facturacion.md             # Billing (generate, review, emit, collect)
│   └── configuracion.md           # Global settings (products, plans, models, integrations)
│
├── 06-riesgos-decisiones/         # Risk & architectural decisions
│   ├── riesgos-identificados.md   # Risk matrix, causes, impact, mitigations
│   └── decisiones-arquitecturales.md # Why we chose DB-per-tenant, events, Protected Resources, etc.
│
├── CLAUDE.md                      # This file
└── README.md                      # User-facing introduction
```

## Key Concepts (Read First)

### 1. **Protected Resources** (Inventario de Funcionalidades)

Every button, endpoint, view, and action in JikkoOps is a "Protected Resource" with a unique code:
- `LIQ-001`: Button to liquidate
- `DOC-003`: Digital signature
- `SOC-001`: Manual notification

**Why**: 
- Granular control: Activate/deactivate per tenant without code
- Auditable: See exactly what's available for each client
- Commercial: Bundle resources into Plans, sell to customers

**Stored in**: `protected_resources` table (DB) + caché (Redis, TTL 15 min)

### 2. **Feature Flags** (Control en Runtime)

Determines if a Protected Resource is ON/OFF for a specific tenant at runtime.

**Example**:
- Contract is "Plan Estándar" → Flags: LIQ-001=ON, SOC-001=OFF (Plan Premium only)
- Escalado detected (modelo-ingresos limit exceeded) → Flags: SOC-002 (% recaudo)=ON automatically

**Stored in**: `feature_flags` table + Redis caché (TTL 5 min)

### 3. **Plans** (Ofertas Comerciales)

A Plan is a combination of:
- Funcionalidades (e.g., Liquidación, Expedientes, Notificaciones)
- Protected Resources enabled for those features
- Revenue model (modelo-ingresos, %, users, etc.)
- Límites (users, expedients/month, storage)

**Example**: "Plan Estándar" = SILIN + DOCS, 50 users, $X/month

### 4. **Tenant** (Instancia de Cliente)

One **Entity** (municipio) → One or more **Tenants** (instances in JikkoOps) → Each tenant has own DB, plan, flags.

### 5. **Revenue Models** (Cómo Se Cobra)

- **CAUTE**: Free for N expedients, then % of revenue or next tier
- **PERCENTAGE_REVENUE**: % of collected revenue
- **PER_USER**: $X per active user per month
- **PER_EXPEDIENT**: $X per expedient processed
- **HÍBRIDO**: Combinations (e.g., modelo-ingresos then percentage)

**Critical**: Changes to pricing require auditable approval (CFO validates, legal reviews).

## Tech Stack

**To Be Confirmed With Team**:
- Language: Python (FastAPI) or Node.js (Express/NestJS)
- Database: PostgreSQL (one DB per tenant)
- Cache: Redis (3-tier: browser, API gateway, application)
- Events: Kafka or RabbitMQ (pub/sub for SILIN, DOCS, SOCIA sync)
- Auth: JWT + OAuth2 (OIDC for SIGIA integration)
- Frontend: React (if UI in JikkoOps, else headless API)

## Safe Change Rules

🚫 **DO NOT change without approval**:
- Revenue share calculations (formula, percentages, minimums)
- Contract terms (dates, services, pricing)
- Protected Resource codes (breaking reference in plans/tenants)
- Audit log entries (immutable by design)

✓ **Safe to change**:
- UI/UX (visual styling, layout)
- Feature names/descriptions (if code stays same)
- Feature flag activations (with operator approval)
- New Protected Resources (if properly inventoried)

**Process**: Code review + auditable trail (who, when, why)

## Coding Conventions

**To Be Defined With Team**, but typically:
- **Naming**: camelCase for functions/variables, PascalCase for classes/types, UPPER_SNAKE for constants
- **Immutability**: Prefer creating new objects over mutating
- **DDD**: Model with rich domain entities (Entity, Tenant, Contract, Invoice, Plan, ProtectedResource)
- **Testing**: 80%+ coverage, test revenue calculations exhaustively
- **API conventions**: RESTful, versioned (/v1/), standardized error responses
- **Logging**: Audit trail for all revenue/contract/flag changes

## Specific Commands

```bash
# Development setup
npm install  # or pip install -r requirements.txt
npm run dev  # Start dev server

# Linting & formatting
npm run lint
npm run format

# Testing
npm test                  # All tests
npm run test:revenue     # Just revenue calculation tests (critical!)
npm run test:flags       # Flag sync tests

# Database
npm run db:migrate       # Run migrations
npm run db:seed         # Seed dev data
npm run db:backup       # Backup current tenant DB

# Deployment
npm run build            # Build for production
npm run deploy           # Deploy to staging/prod

# Scripts
npm run inventory:scan   # Scan code for new Protected Resources
npm run inventory:sync   # Sync matrix of resources
npm run validate:models  # Validate revenue models in config
```

## Feature Flags and Gates

JikkoOps uses feature flags to control:
- Which Protected Resources are active per tenant
- Automatic escalado de volumen (modelo-ingresos → percentage)
- New functionality rollout (beta features)

**View flags**: Operations dashboard or `GET /tenant/flags`
**Change flags**: Only via UI with approval (no direct SQL edits)
**Sync flags**: Automatic (every 5 min) + manual resync available

## Testing

**Critical to test thoroughly**:
1. **Revenue calculations**: Multiple models, edge cases (limits, minimums, caps)
2. **Flag sync**: When contract activates, when escalado triggers, manual changes
3. **Tenant isolation**: Ensure data from one tenant never leaks to another
4. **Integrations**: Events to SILIN, DOCS, SOCIA sync correctly

**Run before shipping**:
```bash
npm test                      # Full suite
npm run test:revenue         # Revenue (most critical)
npm run validate:contracts   # Integrity of contracts in DB
```

## Deployment Notes

- **Backward compatibility**: APIs must be versioned; old endpoints stay for 6 months
- **Database migrations**: Always reversible; test on staging first
- **Secrets**: Never commit API keys, DB passwords, or JWT secrets—use env vars
- **Monitoring**: Alert on >5 min uptime degradation, >10% flag desync, any revenue calc changes

## When Working on New Features

1. **Read first**: 01-arquitectura/vision-general.md + 02-procesos/flujo-contratos.md
2. **Map to system**: Which Protected Resources needed? Which Plans affected? Which modules integrate?
3. **Code review**: Especially if touching revenue, flags, or contracts
4. **Test thoroughly**: Existing tests must pass, new tests for new code
5. **Document**: Update relevant .md file if behavior changes

## Questions or Blockers?

- **Architecture questions**: See 06-riesgos-decisiones/decisiones-arquitecturales.md
- **Data model questions**: See 03-datos/data-model.md
- **API questions**: See 04-apis/endpoints-principales.md
- **Risk questions**: See 06-riesgos-decisiones/riesgos-identificados.md
