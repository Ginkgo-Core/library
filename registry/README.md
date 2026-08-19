# Arsenal Registry

The Arsenal/Library registry is architectural and discovery metadata. It describes **what exists, what is discoverable, what constraints apply and what evidence supports a record**. It is not a runtime router, runtime state store, credential store, Approval database, External Gate database or execution engine.

## P0 source of truth

This registry references, and does not duplicate, the P0 freeze in `Ginkgo-Core/ai-stack#15`:

- `ARCHITECTURE-INVARIANTS.md`
- `docs/adr/ADR-001-routing-model.md`
- `docs/adr/ADR-002-capability-naming.md`
- `contracts/core/*`

Skill/binding behavior is defined by the aligned contracts in `Ginkgo-Core/ginkgo-agents#9`.

Canonical integrated references for this correction:

- `Ginkgo-Core/ai-stack#15` — merged baseline `778b6ba8a2e2681357e2726f7d8b343a32537dcf`
- `Ginkgo-Core/ai-stack#9` — merged baseline `b47f72c8d8b193afdb5ef69e92e7ae8e557fd829`
- `Ginkgo-Core/ginkgo-agents#9` — merged baseline `89247220ffa15238cff209dba13088eae1a1a641`

Architecture precedence is therefore:

```text
ai-stack P0 contracts/invariants
  -> ginkgo-agents skill/binding behavior
  -> ginkgo-library registry/discovery metadata
```

The Library does not fork those contracts.

## Canonical runtime boundary

```text
Caller
  -> CapabilityRequest
  -> Capability Bus
  -> Domain Router
  -> skill/version
  -> adapter
  -> provider
  -> CapabilityResult
```

Operational executor selection is separate:

```text
TaskOps
  -> Executor Router
  -> Human | n8n | Hermes | OpenClaw | Sylvia
```

`Domain Router != Executor Router`.

The Domain Router decides **which eligible implementation runs**. The Library only declares implementation possibilities and constraints.

## Source-of-truth boundaries

```text
Library/GitHub = WHAT EXISTS / WHAT IS DISCOVERABLE / registry architecture / promotion evidence
Agents          = HOW A SKILL OR BINDING BEHAVES
ai-stack        = canonical contracts + runtime architectural boundary
Domain Router   = runtime capability-to-binding resolution
PostgreSQL      = canonical runtime state
Redis           = ephemeral cache/queue/coordination only
```

GitHub registry metadata must never masquerade as mutable runtime state.

## Registry entities

The Arsenal currently records:

- **CAPABILITY** — provider-agnostic public semantic contract.
- **SKILL/BINDING** — purpose-driven skill or internal implementation binding.
- **TOOL** — concrete API surface, MCP, CLI, connector, module or human-assisted surface.
- **PROVIDER** — external implementation/provider family behind an adapter boundary where a provider applies.
- **POLICY** — capability, binding/provider, Approval or External Gate constraints.
- **FALLBACK** — explicit failover policy metadata.
- **PROVENANCE/EVIDENCE** — source references supporting discovery/lifecycle claims.

`EXECUTOR` is a separate TaskOps/Executor Router concern and is not a synonym for any entity above.

The current registry does **not** have a standalone Adapter entity. This PR does not invent one or create a parallel registry. Internal binding records may carry the canonical `adapter_id` defined by `Ginkgo-Core/ginkgo-agents#9`, while Tool records exist only when a distinct concrete execution/tool surface is actually represented. A Tool ID must never reuse an Adapter ID. For the current Shopee read bindings, no separate Shopee API Tool surface is registered; `shopee-open-api` is reserved for the Adapter identity. A dedicated Adapter registry, if adopted, requires a follow-up architecture decision across the Library rather than a Commerce-only one-off.

## Files

- `providers.yaml` — provider families and discovery/access boundaries.
- `capabilities.yaml` — public outcome-level capabilities independent of providers.
- `skills.yaml` — purpose-driven skills and internal implementation bindings with lifecycle ceilings.
- `tools.yaml` — concrete API/tool execution surfaces; Tool != Provider != Adapter.
- `endpoints.yaml` — centralized invocation surfaces where eligible; registry presence is not routing authority.
- `policies.yaml` — capability, binding/provider, Approval and External Gate constraints.
- `fallbacks.yaml` — explicit P0 fallback policy declarations.
- `library.yaml` — Library index.
- `products.yaml` — canonical Ginkgo product/repository roles.

## Core invariant

```text
CALLER != CAPABILITY != SKILL != BINDING != TOOL != ADAPTER != PROVIDER != EXECUTOR
```

Amazon network Commerce example:

```text
public capability: commerce.catalog.search
internal binding:  amazon-spapi.catalog.search
tool/API surface:  amazon-sp-api
adapter identity:   amazon-spapi (defined/referenced by aligned Agents contract)
provider:           amazon-selling-partner
```

Shopee network Commerce example:

```text
public capability: commerce.catalog.read
internal binding:  shopee-open-api.catalog.read
tool/API surface:  n/a in this PR
adapter identity:   shopee-open-api
provider:           shopee-open-platform
```

`shopee-open-api` is therefore an Adapter identity only. A future Shopee Tool/API surface, if registered, must use a distinct Tool identity rather than reusing the Adapter ID.

Local normalization example:

```text
public capability: commerce.product.normalize
internal binding:  shopee.product.normalize
tool:              amazon-ops-shopee-normalizer
implementation:    Scale-Characters/amazon-ops#2:integrations/shopee/catalog.py#normalize_product
adapter:           n/a
provider:          n/a
```

The caller selects the capability intent and constraints. It never selects the binding, adapter, provider, raw endpoint or credential.

## Public capability naming

Public capabilities are provider-agnostic. Current Commerce public IDs are:

```text
commerce.catalog.search
commerce.catalog.read
commerce.pricing.read
commerce.inventory.read
commerce.orders.read
commerce.product.normalize
commerce.product.evaluate
```

Current internal Commerce bindings include:

```text
amazon-spapi.catalog.search
amazon-spapi.pricing.read
amazon-spapi.inventory.read
amazon-spapi.orders.read
shopee-open-api.catalog.read
shopee-open-api.orders.read
shopee.product.normalize
commerce.product.evaluate
```

Historical `commerce.amazon.*` and `commerce.shopee.*` IDs are compatibility aliases only. They are deprecated and must never be advertised as public capabilities.

For product normalization specifically, both legacy implementation names are deprecated aliases to the same final binding:

```text
commerce.shopee.product.normalize -> shopee.product.normalize
shopee-open-api.product.normalize -> shopee.product.normalize
```

The second alias is retained only for migration compatibility because its old name incorrectly implied Shopee Open API execution.

## Discovery is not authorization

The following states are distinct:

```text
DISCOVERABLE != ROUTING_ELIGIBLE != PRODUCTION_ELIGIBLE
```

A record may exist in the Library and still be blocked from runtime routing. `FEATURE` is never production-routable.

For this Commerce batch:

- Amazon bindings: `FEATURE / F1`, `routing_eligible: false`, live execution blocked.
- Shopee network read bindings: `FEATURE / F1 / PRE_APPROVAL`, `routing_eligible: false`, live execution blocked.
- `shopee.product.normalize`: `FEATURE / F1 / PRE_APPROVAL`, offline/local fixture scope only, `routing_eligible: false`, not production eligible.
- `commerce.product.evaluate`: `PROTOTYPE / F2`, eval/test only, not production eligible.

Offline execution of `shopee.product.normalize` does not promote lifecycle. Promotion still requires governed live-payload evidence and canonical contract validation.

## Approval vs External Gate

P0 Approval and External Gate are separate contracts/state machines.

```text
Approval:      NOT_REQUIRED | PENDING | APPROVED | REJECTED | EXPIRED
External Gate: CLEAR | BLOCKED | UNKNOWN | EXPIRED
```

An Approval transition cannot clear an External Gate.

For live Shopee acquisition, the registry declares the required gate:

```yaml
external_gate:
  id: shopee_business_approval
  required: true
  registry_observed_state: UNKNOWN
  runtime_state_source: PostgreSQL
```

`UNKNOWN` here is a source-backed registry observation indicating pending evidence. It is **not** the canonical mutable runtime gate state. Runtime policy/PostgreSQL owns that state.

The gate boundary is intentionally split:

```text
LIVE ACQUISITION
Shopee Open API read
  -> shopee_business_approval required
  -> governed payload obtained

LOCAL PROCESSING
payload already supplied/authorized
  -> shopee.product.normalize
  -> deterministic local Python transform
  -> no network External Gate
```

`shopee_business_approval` applies to the live acquisition path (`shopee-open-api.catalog.read`, `shopee-open-api.orders.read`). It is an upstream requirement when a live payload must first be obtained, but it is not an execution gate for deterministic normalization of a payload already available to the binding.

Human Approval cannot clear or replace the Shopee External Gate.

## Fallback P0 freeze

Default:

```yaml
fallback_policy: none
```

The only generic opt-in vocabulary permitted by P0 is:

```yaml
fallback_policy: same_semantics_only
```

No silent fallback may:

- cross marketplace;
- cross domain;
- change read into write or escalate permissions;
- promote `FEATURE` into production execution;
- change capability semantics;
- change privacy or cost class without an explicit reviewed policy.

A discovery candidate list is not an executable fallback chain. If no safe route exists, runtime fails closed with a normalized blocked/error result.

The full `fallbacks.yaml` audit in this PR keeps all current entries at `fallback_policy: none`. Existing multi-candidate records for `knowledge.research`, `creative.video.cloud` and `dev.async-coding` require follow-up before any `same_semantics_only` opt-in can be justified.

## Commerce economic evaluation

`commerce.product.evaluate` remains provider-neutral deterministic domain logic at `PROTOTYPE`.

Policy-owned thresholds:

```yaml
minimum_multiplier: 5.0
minimum_operating_margin_pct: 60
minimum_contribution_brl: 30
caller_may_override: false
```

Its Tool is `amazon-ops-product-evaluator` (`kind: python-module`, `taxonomy_role: tool`). Adapter and Provider are not applicable. `local` is execution location semantics, not a Provider identity, and is not attached to the evaluator as `provider` or `preferred_providers`.

Its domain decision `APPROVED` is not P0 Approval and grants no authority to publish, purchase, change pricing, change inventory or fulfill orders.

## Shopee evidence

Canonical evidence source: `Scale-Characters/amazon-ops#2` at head `bccfc0f5142f432f889158870c8f9e26e353fa65`.

GitHub Actions evidence recorded by the aligned Agents contract:

- Ruff: PASS
- Shopee offline: 28 passed
- General suite: 104 passed, 1 deselected
- Windows/Linux diagnostic: 1 expected failure, explicit and non-blocking
- credentials in CI: none
- mutations: OFF
- live Shopee: OFF

For `shopee.product.normalize`, this proves offline normalization behavior against local/fake inputs only.

```text
offline normalization evidence
!= governed live-payload contract validation
```

No approved live Shopee payload has yet proven the canonical Commerce normalization contract. Shopee Business Approval is required before that live payload can be acquired through the governed read path; the subsequent normalization contract validation is a separate promotion requirement.

## Secrets

Registry files may contain logical credential references such as `GITHUB_CONNECTION`, `AMZ_SP_REFRESH_TOKEN` or `SHOPEE_PARTNER_KEY`. Credential values must never be committed.

## Follow-ups outside this PR

This PR intentionally does not remodel unrelated domains or create new registry families. Explicit follow-ups are:

1. Decide whether the Library needs a first-class Adapter entity/registry across all domains; do not create a Commerce-only parallel registry.
2. Review pre-P0 non-Commerce capability naming such as local/cloud/provider-flavored public names against ADR-002.
3. Prove semantic/privacy/cost/lifecycle equivalence before any current multi-candidate fallback is allowed to opt into `same_semantics_only`.
4. Review legacy `preferred_skills`, `preferred_tools`, `fallbacks` and executor metadata outside Commerce so discovery vocabulary cannot be mistaken for runtime authority.