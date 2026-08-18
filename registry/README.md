# Arsenal Registry

The registry is the machine-readable map agents use to discover what Ginkgo can do and how to execute it safely.

```text
caller
  -> skill runtime contract
  -> capabilities.yaml / Capability Bus
  -> skills.yaml
  -> policies.yaml / Router
  -> tools.yaml / adapter
  -> providers.yaml
  -> endpoints.yaml when eligible
  -> fallbacks.yaml
  -> normalized result
```

The canonical runtime envelope and lifecycle live in `Ginkgo-Core/ginkgo-agents/SKILL_RUNTIME_CONTRACT.md`. This registry describes **what exists and what is eligible**, not a second implementation of the Atlas.

## Files

- `providers.yaml` — provider families and access/billing boundaries.
- `capabilities.yaml` — outcome-level capabilities independent of vendors.
- `skills.yaml` — purpose-driven reusable agent actions and their current lifecycle ceiling.
- `tools.yaml` — concrete APIs, MCPs, CLIs, connectors and human-assisted surfaces.
- `endpoints.yaml` — centralized invocation surfaces; avoids endpoint duplication in skills.
- `policies.yaml` — routing, cost, safety, lifecycle gates and execution policy.
- `fallbacks.yaml` — safe fallback chains by capability.
- `library.yaml` — Library index.
- `products.yaml` — canonical Ginkgo product/repository roles.

## Core invariant

```text
CALLER != CAPABILITY != SKILL != TOOL != PROVIDER
```

Example:

```text
capability: knowledge.research
skill: research.market-deep-scan
tool: google-deep-research-ui
provider: google-ai
```

The skill expresses **why/what the agent is doing**. The tool/provider expresses **how it is currently implemented**.

## Caller-facing lifecycle

New skill records use:

```text
FEATURE -> PROTOTYPE -> VALIDATED -> INTEGRATED -> PRODUCTION
```

Existing `F0/F1/F2/F3/S1/S2/S3`, `SPECIFIED` and `SKILL` markers remain compatibility metadata owned by the Skill Atlas. Registry discovery must never interpret a FEATURE record as routing permission.

## Approval fields

`approval_required` is the canonical field for new records. Existing `human_approval` fields remain supported as legacy registry metadata.

Human approval is independent from provider/external gates. For example, a skill may declare `approval_required: false` and still be blocked because `shopee_business_approval` has not been satisfied.

## Secrets

Registry files contain only logical credential references such as `GITHUB_CONNECTION`, `AMZ_SP_REFRESH_TOKEN` or `SHOPEE_PARTNER_KEY`. Credential values must never be committed.

## Execution truth

Tools must declare one of `autonomous`, `supervised`, `connector`, or `human_assisted`. Never model a web-only product as an autonomous API merely because it is useful.

A tool can also be known to the Arsenal while remaining `routing_eligible: false`. Discovery metadata is not execution authorization.

## Marketplace invariant

Marketplace is a routing constraint, not a fallback dimension:

```text
Amazon request -> Amazon-eligible implementation or BLOCKED
Shopee request -> Shopee-eligible implementation or BLOCKED
```

Never silently cross marketplaces, escalate read to write, or bypass PRE-APPROVAL/validation gates.
