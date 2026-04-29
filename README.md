# JikkoOps - Modular Back-Office for Intelligent Governance

## What is JikkoOps?

JikkoOps is a **modular, cloud-based back-office platform** that enables municipalities, gobernaciones, and public entities in Colombia to:

✅ **Commercialize digital services**: Create and sell government services as customizable plans  
✅ **Control feature activation**: Enable/disable functionality per client without code deployment  
✅ **Monetize flexibly**: Support caute pricing, revenue share, per-user, per-transaction, and hybrid models  
✅ **Integrate seamlessly**: Orchestrate liquidation (SILIN), documents (DOCS), citizen services (SOCIA), and payment systems  
✅ **Maintain compliance**: Immutable audit logs, encrypted data, granular role-based access  

**In short**: JikkoOps lets you **package government services into products, sell them as plans, and charge different customers different prices** — all while maintaining complete audit trails.

---

## The JikkoOps Ecosystem

```
                    SIGIA
              (Intelligent Governance)
                       |
        ┌──────────┬───┴────┬────────┐
        |          |        |        |
      SILIN      DOCS     SOCIA     IAM
    (Liquidation)(Docs) (Citizens)(Auth)
        |          |        |        |
        └──────────┴────┬───┴────────┘
                        |
                   JIKKOOPS
              (This Back-Office)
                        |
         ┌──────┬───────┼──────┬──────┐
         |      |       |      |      |
       Plans Products Revenue Tenants Flags
```

**JikkoOps coordinates the back-office**:
- Defines what services (Plans) are available
- Decides which customers get which services (Protected Resources + Feature Flags)
- Calculates what to charge each customer (Revenue Models)
- Monitors health and escalates when thresholds are hit (Operations)
- Issues invoices and tracks payments (Billing)

---

## Documentation Structure

### **Start Here** 👇

| Document | Purpose | Audience |
|----------|---------|----------|
| [CLAUDE.md](./CLAUDE.md) | Development guidance for Claude AI | Developers, architects |
| [01-arquitectura/vision-general.md](./01-arquitectura/vision-general.md) | System overview and key concepts | Everyone |
| [02-procesos/flujo-contratos.md](./02-procesos/flujo-contratos.md) | How customers move from prospect to paid client | Product, Sales, Operations |

### **Deep Dives**

| Folder | What's Inside |
|--------|---------------|
| **01-arquitectura/** | System design, Protected Resources, feature inventory |
| **02-procesos/** | Contract lifecycle, revenue models, pricing flexibility |
| **03-datos/** | Database schema, caching strategy, data synchronization |
| **04-apis/** | REST endpoints, integration events to other modules |
| **05-modulos-core/** | UI modules: Dashboard, Contracts, Operations, Billing, Configuration |
| **06-riesgos-decisiones/** | Risk matrix, architectural decisions, trade-offs |

### **Quick Reference**

- **How does billing work?** → [02-procesos/revenue-share-models.md](./02-procesos/revenue-share-models.md)
- **What's a Protected Resource?** → [01-arquitectura/protected-resources.md](./01-arquitectura/protected-resources.md)
- **How do feature flags work?** → [03-datos/feature-flags-sync.md](./03-datos/feature-flags-sync.md)
- **What APIs are available?** → [04-apis/endpoints-principales.md](./04-apis/endpoints-principales.md)
- **What can go wrong?** → [06-riesgos-decisiones/riesgos-identificados.md](./06-riesgos-decisiones/riesgos-identificados.md)

---

## Key Concepts Explained

### **Protected Resources** 🔐
Every button, endpoint, view, and action is inventoried with a unique code:
- `LIQ-001`: Liquidation button
- `DOC-003`: Digital signature
- `SOC-001`: Notification

Why? So we can **activate/deactivate per customer without touching code**.

### **Plans** 📦
Bundles of Protected Resources with pricing:
- **Plan Básico**: Liquidation only, 50 users, free
- **Plan Estándar**: Liquidation + Documents, 50 users, 10% revenue share
- **Plan Premium**: All features, unlimited users, 15% revenue share

### **Feature Flags** 🚩
Controls which Protected Resources are ON/OFF for each customer at runtime:
- Tied to their Plan (automatic)
- Adjusted for escalado (when volume limits hit)
- Changeable manually by operations team

### **Revenue Models** 💰
How we charge:
- **CAUTE** ("trust, pay later"): Free until N expedients, then charge % of future revenue
- **PERCENTAGE_REVENUE**: Always % of collected revenue (10-20%)
- **PER_USER**: $X per active user per month
- **PER_EXPEDIENT**: $X per liquidation/transaction
- **HYBRID**: Any combination

---

## Sample Workflow: A New Customer

### Day 1: Prospecting
- Sales sees opportunity in **G-COPS** (revenue orchestrator)
- Creates CRM lead for "Municipio Cali"

### Day 5: Proposal
- Sales proposes **Plan Estándar**: Liquidation + Documents, 50 users, 10% recaudo (revenue)
- Customer reviews and approves

### Day 10: Contract Activation
- Manager enters contract in JikkoOps
- System automatically:
  - Activates feature flags (LIQ-*, DOC-* = ON)
  - Creates database for tenant
  - Sends "welcome" event to CILIN, DOS, SOCIA
  - Notifies customer with access credentials

### Day 11–180: Usage (Caute Phase)
- Customer processes 500 liquidations/month → **$0 charge**
- System tracks: expedients counter, users active, etc.

### Day 181: Escalado (Limit Exceeded)
- Customer processed 1,050 expedients (exceeds 1,000 caute limit)
- System automatically:
  - Flags escalado event
  - Activates "% recaudo" flags
  - Notifies customer: "You've exceeded trial limit. Starting revenue share model."

### Day 185–: Billing
- Monthly, system generates invoice:
  - Recaudos processed: $1.5M
  - JikkoOps revenue (10%): $150,000
  - Sent to customer, customer pays

### Day 365: Renewal
- Contract expires
- Operations dashboard alerts: "CALI contract expires in 30 days"
- Sales reaches out with renewal proposal → Loop back to Day 5

---

## Architecture Highlights

### Multi-Tenant by Design
- Each customer has own PostgreSQL database
- Complete data isolation
- Scalable: Distribute tenants across servers

### Event-Driven
- Changes to flags emit events
- CILIN, DOS, SOCIA subscribe and react
- No tight coupling, resilient to failures

### Feature Flags as First-Class Citizen
- Every feature is a flag
- Flags stored in DB + Redis cache
- Change takes effect in <5 seconds (cache TTL)

### Immutable Audit Trail
- All changes logged (who, when, what, why)
- Required by Colombian fiscal authorities (7-year retention)
- Cannot be modified or deleted

---

## Technology Stack

**Proposed** (To Be Confirmed):

| Component | Choice | Why |
|-----------|--------|-----|
| Backend | Python (FastAPI) or Node.js | Fast, scalable, good for async/events |
| Database | PostgreSQL (per tenant) | ACID guarantees, JSON support, compliance |
| Cache | Redis | Multi-level caching, feature flags, sessions |
| Events | Kafka or RabbitMQ | Pub/sub for module sync, audit trail |
| Auth | JWT + OAuth2/OIDC | Stateless APIs, SIGIA integration |
| Frontend | React | (if UI needed; else headless API) |
| Infrastructure | Docker + Kubernetes | Container orchestration, scaling |
| Monitoring | Prometheus + Grafana | Alerts, KPIs, uptime tracking |

---

## Getting Started

### 1. **Understand the System**
   - Read [01-arquitectura/vision-general.md](./01-arquitectura/vision-general.md) (15 min)
   - Read [02-procesos/flujo-contratos.md](./02-procesos/flujo-contratos.md) (20 min)

### 2. **Learn Key Concepts**
   - Protected Resources: [01-arquitectura/protected-resources.md](./01-arquitectura/protected-resources.md)
   - Revenue Models: [02-procesos/revenue-share-models.md](./02-procesos/revenue-share-models.md)
   - Feature Flags: [03-datos/feature-flags-sync.md](./03-datos/feature-flags-sync.md)

### 3. **Explore Architecture**
   - Data Model: [03-datos/data-model.md](./03-datos/data-model.md)
   - APIs: [04-apis/endpoints-principales.md](./04-apis/endpoints-principales.md)
   - Integrations: [04-apis/integration-events.md](./04-apis/integration-events.md)

### 4. **Review Decisions**
   - Why we chose DB-per-tenant: [06-riesgos-decisiones/decisiones-arquitecturales.md](./06-riesgos-decisiones/decisiones-arquitecturales.md)
   - What can go wrong: [06-riesgos-decisiones/riesgos-identificados.md](./06-riesgos-decisiones/riesgos-identificados.md)

### 5. **Develop**
   - Check [CLAUDE.md](./CLAUDE.md) for coding conventions, safe changes, commands

---

## FAQs

**Q: Is this a tax/liquidation system?**  
A: No. JikkoOps is the *back-office* that *manages* access to liquidation (CILIN) and other services. CILIN does the actual tax calculations.

**Q: Why multiple revenue models?**  
A: Different customers have different payment capability. Some pay upfront, some pay by volume, some by number of users. Flexibility = more sales.

**Q: What happens if CILIN goes down?**  
A: JikkoOps can't confirm new expedients were processed, so it degrades gracefully (can't generate invoices until CILIN recovers). Cached data keeps the system partly functional.

**Q: How do we prevent fraud in revenue calculations?**  
A: Code review on all changes, audit logs of calculations, monthly independent validation, no direct SQL modifications to invoice amounts.

**Q: Can a customer see our pricing model?**  
A: No. Pricing is configured by Operations, stored securely, and only shown to authorized managers. Customers see what they owe, not how we calculated it.

---

## Contributing

When adding features or modifying JikkoOps:

1. **Understand the impact**: Which Plans are affected? Which tenants? Which revenue models?
2. **Follow safe change rules**: Pricing changes need approval. Feature additions need Protected Resource inventory.
3. **Test thoroughly**: Revenue tests are critical. Integration tests for CILIN/DOS/SOCIA sync.
4. **Update documentation**: Keep the .md files in sync with code changes.
5. **Code review**: Especially for revenue, flags, contracts, audit trails.

---

## Questions?

- **Architecture questions?** → See [06-riesgos-decisiones/decisiones-arquitecturales.md](./06-riesgos-decisiones/decisiones-arquitecturales.md)
- **API questions?** → See [04-apis/endpoints-principales.md](./04-apis/endpoints-principales.md)
- **Operational questions?** → See [05-modulos-core/operacion.md](./05-modulos-core/operacion.md)
- **Development setup?** → See [CLAUDE.md](./CLAUDE.md)

---

## Glossary

| Term | Meaning |
|------|---------|
| **Protected Resource** | A feature (button, endpoint, view) with a unique code, activated via flags |
| **Plan** | A bundle of Protected Resources + pricing model sold to customers |
| **Tenant** | One customer's instance of JikkoOps (has own DB, flags, entitlements) |
| **Feature Flag** | ON/OFF switch for a Protected Resource per tenant |
| **Escalado** | Automatic promotion when customer exceeds a limit (e.g., caute → percentage) |
| **Caute** | "Trust, pay later" pricing model |
| **Entitlement** | What a customer is authorized to use (Plan + Flags) |
| **SIGIA** | Larger ecosystem: SILIN, DOCS, SOCIA, IAM, JikkoOps |
| **Revenue Model** | Formula for charging (%, per user, per expedient, hybrid, etc.) |

---

## License & Attribution

**Jikkosoft Internal Use Only**

Created for the JikkoOps proof-of-concept based on discussions with architecture, product, and operations teams (April 2026).

Last updated: **April 29, 2026**
