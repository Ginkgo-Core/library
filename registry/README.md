# Arsenal Registry

The registry is the machine-readable map agents use to discover what Ginkgo can do and how to execute it safely.

```text
agent goal
  -> capabilities.yaml
  -> skills.yaml
  -> policies.yaml
  -> tools.yaml
  -> endpoints.yaml
  -> fallbacks.yaml
  -> normalized result
```

## Files

- `providers.yaml` — provider families and access/billing boundaries.
- `capabilities.yaml` — outcome-level capabilities independent of vendors.
- `skills.yaml` — purpose-driven reusable agent actions.
- `tools.yaml` — concrete APIs, MCPs, CLIs, connectors and human-assisted surfaces.
- `endpoints.yaml` — centralized invocation surfaces; avoids endpoint duplication in skills.
- `policies.yaml` — routing, cost, safety and knowledge-promotion policy.
- `fallbacks.yaml` — safe fallback chains by capability.
- `library.yaml` — Library index.
- `products.yaml` — canonical Ginkgo product/repository roles.

## Core invariant

```text
CAPABILITY != SKILL != TOOL != PROVIDER
```

Example:

```text
capability: knowledge.research
skill: research.market-deep-scan
tool: google-deep-research-ui
provider: google-ai
```

The skill expresses **why/what the agent is doing**. The tool/provider expresses **how it is currently implemented**.

## Secrets

Registry files contain only logical credential references such as `GITHUB_CONNECTION` or `FAL_API_KEY`. Credential values must never be committed.

## Execution truth

Tools must declare one of `autonomous`, `supervised`, `connector`, or `human_assisted`. Never model a web-only product as an autonomous API merely because it is useful.
