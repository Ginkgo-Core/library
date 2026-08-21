# Lead Intelligence Provider Atlas v1

Status: `FEATURE / F1` discovery/provenance layer. This document does not authorize runtime routing.

## Decision summary

Ginkgo should not replace Manus with another monolithic lead tool. The target is a Ginkgo-native Lead Intelligence capability family with provider adapters underneath it.

```text
Ginkgo Lead Intelligence
-> provider-neutral Sales capabilities
-> Sales Domain Router
-> adapter/provider portfolio
```

Recommended posture:

1. **Firecrawl** — first provider candidate for broad web discovery and enrichment.
2. **Ledger Lead Finder** — optional legacy adapter only if its provenance/terms/quality can be established.
3. **gosom/google-maps-scraper** — technical benchmark/lab candidate only until target-source terms are cleared.
4. **Google Places API** — source-specific restricted candidate; do not assume it is valid as a persistent lead-list database merely because phone/place details are available through the API.

## Why Firecrawl is the leading candidate

The current Firecrawl ecosystem exposes:
- search;
- scrape;
- structured extraction;
- directory/company extraction patterns;
- lead-generation workflow skills;
- CLI/MCP/agent integrations.

This aligns with Ginkgo because Firecrawl can remain an implementation behind adapters instead of becoming the workflow brain.

### Upstream pins observed on 2026-08-21

Core:

```text
firecrawl/firecrawl
071d7eb6fe284919bf4468ee97050da7281cbeb3
license: AGPL-3.0
```

Workflow reference:

```text
firecrawl/firecrawl-workflows
94cc91229d6cedc0613f140d3d013b150bc8e1b0
license: ISC
relevant pattern: firecrawl-lead-gen
```

The workflow license and core runtime license are intentionally recorded separately.

### Adoption boundary

Do not vendor upstream lead-generation behavior as canonical Ginkgo behavior.

Use upstream as:
- provider implementation candidate;
- workflow pattern source;
- evaluation reference.

Ginkgo owns:
- capability semantics;
- ICP contract;
- source policy;
- evidence model;
- deduplication;
- qualification;
- contact validation;
- CRM persistence;
- TaskOps scheduling;
- communication/transaction gates.

## Ledger Lead Finder

Observed behavior from the initial experiment:
- recurring lead collection was configured;
- the tool returned businesses with telephone fields;
- telephone availability did not prove WhatsApp account availability;
- the surrounding browser workflow could save records to CRM;
- provenance/terms/interface semantics are not yet sufficiently established for Ginkgo routing.

Therefore:

```text
Ledger Lead Finder != Lead Intelligence skill
```

Possible future role:

```text
sales.lead.discover
-> Domain Router
-> ledger-lead-finder adapter
-> external tool
```

Only if validation passes.

Reject or keep lab-only if:
- provenance is opaque;
- source/storage rights cannot be established;
- it cannot expose deterministic input/output/error semantics;
- it maps `phone available` to `WhatsApp valid`;
- it requires uncontrolled browser authority.

## gosom/google-maps-scraper

Observed upstream pin:

```text
gosom/google-maps-scraper
4676350a5bfdf5889e60fbd267794ab768013f02
license: MIT
```

Technical advantages:
- Docker/self-hosted;
- CLI;
- REST API;
- rich business fields;
- agent skill;
- PostgreSQL/output integrations;
- suitable performance/architecture benchmark.

Critical distinction:

```text
MIT license for scraper software
!=
permission to scrape/store target-source content
```

Google Maps Platform terms contain restrictions against scraping/copying Maps content. Therefore the Ginkgo record stays:

```text
FEATURE_RESTRICTED
routing_eligible: false
lab_only: true
```

No proxy rotation or anti-detection strategy is an architectural workaround for a source-policy denial.

## Google Places API

Place Details can technically expose establishment data including address and phone fields. Technical capability is useful for source research but does not remove Google Maps Platform restrictions around use/copying/storage.

Default Ginkgo posture:

```text
persistent_lead_database_provider: false
routing_eligible: false
requires_source_specific_terms_review: true
```

If later approved for a narrower application use, the adapter must carry the exact allowed retention, attribution and purpose constraints.

## Desired source portfolio

The long-term objective is not one scraper. It is a governed portfolio of sources such as:
- licensed business directories;
- public corporate registries where reuse permits the intended purpose;
- business websites;
- policy-approved web search/extraction providers;
- customer-owned/internal CRM data;
- explicit business-contact sources.

Each source has an independent eligibility record.

## Provider-neutral decomposition

```text
sales.lead.discover
sales.lead.normalize
sales.lead.enrich
sales.lead.validate
sales.lead.qualify
sales.lead.deduplicate
sales.lead.batch.build
sales.pipeline.record
sales.lead.report.generate
```

The design deliberately separates source discovery from downstream communication.

## Contact channel evidence

Canonical rule:

```text
phone_available != whatsapp_valid
```

Allowed statuses:
- `VALID`
- `INVALID`
- `UNKNOWN`
- `NOT_CHECKED`

WhatsApp may be marked `VALID` only when an approved evidence mechanism explicitly supports that conclusion.

Not sufficient alone:
- generic telephone field;
- “phone available” UI label;
- successful navigation to a `wa.me` URL.

Lead Intelligence must not send a message or place a call just to test an account.

## Data quality model

A source/provider should be evaluated on:
- coverage;
- field completeness;
- evidence traceability;
- freshness;
- false-positive rate;
- duplicate rate;
- contact validation quality;
- accepted-lead yield;
- cost per accepted lead;
- rate-limit reliability;
- source terms/storage compatibility.

The commercial metric is not “rows per minute.” It is closer to:

```text
cost / accepted, evidence-backed, policy-eligible lead
```

## Promotion evidence packet

Before any provider can become routing-eligible:
- immutable source/version reference where possible;
- license/terms decision;
- adapter contract;
- positive fixture tests;
- negative/policy tests;
- error normalization;
- cost/rate policy;
- provenance mapping;
- governed read-only E2E;
- reviewer approval.

A provider cannot promote itself by popularity, feature count or upstream lead-gen claims.

## Cross-repository authority

```text
ai-stack
-> architecture + domain contracts + source-policy semantics

ginkgo-agents
-> Lead Intelligence behavior/loadout/evals

ginkgo-library
-> provider/source provenance + terms/license + discovery metadata

ginkgo-runtime / adapters
-> future implementation only after validation
```

The Library records provider intelligence. It never selects the runtime provider.
