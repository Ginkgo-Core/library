---
id: salesforce
name: Salesforce
aliases: [Sales Cloud, Service Cloud, Agentforce, Data 360]
kind: platform
category: app.crm
status: validated
maturity: production-proven
priority: critical
summary: Enterprise CRM and customer-data platform with sales, service, marketing, commerce and agentic AI capabilities; recommended as a governed enterprise provider beneath Ginkgo Runtime rather than a platform to reproduce.
canonical_url: https://www.salesforce.com/br/
website_url: https://www.salesforce.com/br/
docs_url: https://developer.salesforce.com/docs
license: proprietary
open_source: false
self_hostable: false
commercial_service: true
integration_surfaces:
  api: true
  mcp: unknown
  cli: true
  sdk: true
  webhook: true
  docker: false
capabilities:
  - app.crm.system-of-record
  - app.crm.accounts
  - app.crm.contacts
  - app.crm.leads
  - app.crm.opportunities
  - app.sales.pipeline
  - app.sales.forecast
  - app.service.cases
  - app.customer-data
  - app.marketing
  - agents.agentforce
  - integration.rest-api
  - integration.events
  - automation.flow
tags:
  - salesforce
  - crm
  - enterprise
  - customer-360
  - agentforce
  - data-360
  - sales
  - service
  - provider
canonical_owner: ginkgo-connect
ginkgo_products:
  - ginkgo-runtime
  - ginkgo-ops
  - ginkgo-connect
  - ginkgo-library
promotion_target: Ginkgo-Core/runtime
operational_profile:
  deployment: vendor-cloud
  data_leaves_environment: true
  gpu_required: false
  maintenance_burden: medium
  cost_profile: high
risk:
  security: medium
  vendor_lock_in: high
  maintenance: medium
  license: medium
source_discovered_at: 2026-09-08
source_verified_at: 2026-09-08
last_curated_at: 2026-09-08
provenance:
  - type: official_site
    ref: https://www.salesforce.com/br/crm/
  - type: official_pricing
    ref: https://www.salesforce.com/br/sales/pricing/
  - type: official_agentforce
    ref: https://www.salesforce.com/br/agentforce/
  - type: official_data
    ref: https://www.salesforce.com/br/marketing/data/
---

# Salesforce

## TL;DR

Salesforce is a strategic **enterprise CRM/customer-data provider** for Ginkgo. It should not replace Ginkgo Runtime, Ginkgo Ops or the provider abstraction. It should sit below the Runtime as an external system of record for customer-facing domains.

Canonical position:

```text
Ginkgo Ops
  -> AI Employee
  -> Ginkgo Runtime
  -> Capability Bus
  -> SalesforceProvider
  -> Salesforce
```

Recommended decision:

- **GO** as Tier-1 enterprise CRM provider.
- **GO** for Agentforce / Data 360 architectural benchmarking.
- **NO-GO** for rebuilding Salesforce-style enterprise CRM breadth inside Ginkgo Ops.
- **NO-GO** for coupling AI Employees directly to Salesforce object APIs.

## What Salesforce is

Salesforce is primarily an enterprise CRM and customer platform. Its strongest center of gravity is front-office operations:

- account and contact management;
- lead and opportunity management;
- pipeline and forecasting;
- customer service and cases;
- marketing and customer journeys;
- commerce;
- customer data unification;
- automation;
- AI agents and actions.

It is not a traditional full ERP in the same sense as Odoo, SAP S/4HANA or Oracle ERP.

## Strategic position in Ginkgo

Salesforce should be treated as a **Customer System of Record** when the client already uses it.

Examples of domains Salesforce can own:

```text
Account
Contact
Lead
Opportunity
Pipeline
Forecast
Case
Customer profile
Customer journey
Sales activities
Service activities
```

Ginkgo should own:

```text
workspace
AI employee configuration
capability requests
policy decisions
approvals
execution evidence
conversation state
provider bindings
cross-system orchestration
agent memory
usage and observability
```

## Why it matters to Ginkgo

Salesforce is strategically important for two reasons.

### 1. Enterprise market access

A large enterprise customer will often already have Salesforce. Requiring migration to a Ginkgo-native CRM would block adoption. A governed Salesforce provider makes Ginkgo additive instead of disruptive.

### 2. Architectural benchmark

Salesforce's direction around Customer 360, Data 360, Agentforce, actions and business context overlaps strongly with the problem Ginkgo is solving.

This makes Salesforce a reference implementation for:

- contextual agents;
- customer data grounding;
- permission-aware actions;
- action catalogs;
- enterprise AI governance;
- business-process automation.

The correct response is not to clone Salesforce. It is to make Ginkgo interoperable with it while keeping the Ginkgo capability model provider-neutral.

## Recommended provider contract

Initial capabilities:

```text
crm.account.search
crm.account.get

crm.contact.search
crm.contact.get
crm.contact.create
crm.contact.update

crm.lead.search
crm.lead.create
crm.lead.update
crm.lead.qualify

crm.opportunity.search
crm.opportunity.get
crm.opportunity.create
crm.opportunity.update

sales.pipeline.read
sales.forecast.read

service.case.search
service.case.get
service.case.create
service.case.update

customer.context.read
customer.timeline.read
```

The AI Employee should never receive raw Salesforce-specific object semantics when a stable Ginkgo capability exists.

Bad:

```text
AI Employee -> PATCH /services/data/.../sobjects/Opportunity/...
```

Preferred:

```text
AI Employee
  -> crm.opportunity.update
  -> Policy Engine
  -> SalesforceProvider
  -> Salesforce API
```

## Provider mapping concept

Example:

```yaml
capability: crm.opportunity.update
provider: salesforce
operation:
  object: Opportunity
  action: update
policy:
  risk: medium
  approval: conditional
```

Provider-specific mappings stay inside the adapter/provider layer.

## Identity and authorization

Do not use one unrestricted Salesforce principal for all Ginkgo operations.

Recommended baseline:

- one provider binding per Ginkgo Organization / Salesforce org;
- explicit scopes and permission sets;
- least-privilege integration identities;
- per-capability authorization policy;
- credential rotation;
- audit correlation between Ginkgo execution and Salesforce mutation;
- no credentials in repo or workflow JSON.

## Idempotency

Writes triggered by agents or n8n must be idempotent where business semantics permit.

Each write should carry a Ginkgo execution identity:

```text
organization_id
execution_id
capability_request_id
provider_binding_id
idempotency_key
```

For create operations, use external IDs or a Ginkgo correlation field where deployment policy allows it.

## Event strategy

Salesforce events can feed Ginkgo, but event delivery must not become the system of truth.

Preferred pattern:

```text
Salesforce event
  -> ingestion
  -> normalize
  -> dedupe
  -> Ginkgo event
  -> policy/workflow
```

When the event represents a state transition, reconcile against Salesforce before high-impact downstream execution where stale or duplicated events would be dangerous.

## Agentforce vs Ginkgo AI Employees

Agentforce and Ginkgo AI Employees overlap conceptually but should not be treated as identical layers.

### Agentforce

Best when the operating context and actions are primarily inside Salesforce.

### Ginkgo AI Employees

Best when work spans multiple systems:

```text
Salesforce
SAP
Odoo
Gmail
WhatsApp
Google Workspace
Stripe
Supabase
n8n
internal APIs
```

The strategic Ginkgo thesis is therefore cross-system:

> the AI Employee belongs to the organization, not to one SaaS vendor.

## Cost posture

Salesforce is a premium enterprise platform. Public pricing varies significantly by product and edition and should be re-verified at proposal time.

Ginkgo commercial implications:

- do not bundle Salesforce licensing into a generic SMB package;
- sell integration, governance and AI orchestration independently from Salesforce licensing;
- preserve portability to other CRMs;
- account for API/automation/AI consumption limits in implementation sizing.

## Build vs Buy vs Reference

| Decision | Recommendation |
|---|---|
| Build enterprise CRM breadth | NO |
| Replace Salesforce in existing enterprise | NO |
| Integrate Salesforce | YES |
| Use as customer system of record | YES, when already authoritative |
| Study Agentforce patterns | YES |
| Couple Runtime directly to Salesforce | NO |
| Normalize through Capability Bus | YES |

## Ginkgo destination

```text
Ginkgo Library
    -> validated Salesforce knowledge
    -> Ginkgo Connect provider contract
    -> Ginkgo Runtime capability binding
    -> Ginkgo Ops / AI Employees consume provider-neutral capabilities
```

The first implementation should be read-heavy and low-risk before write actions are promoted.

## Initial PoC

Phase 1:

```text
crm.account.search
crm.contact.search
crm.opportunity.search
sales.pipeline.read
service.case.search
```

Phase 2:

```text
crm.lead.create
crm.opportunity.create
service.case.create
```

Phase 3:

```text
crm.opportunity.update
crm.lead.qualify
service.case.update
```

High-impact automations require explicit policy and approval gates.

## Sources

- Salesforce CRM: https://www.salesforce.com/br/crm/
- Sales Cloud pricing: https://www.salesforce.com/br/sales/pricing/
- Agentforce: https://www.salesforce.com/br/agentforce/
- Data 360: https://www.salesforce.com/br/marketing/data/
- Developer documentation: https://developer.salesforce.com/docs

## Verification notes

Pricing, packaging and AI consumption models change frequently. Treat exact commercial values as time-sensitive and re-verify before quoting customers.