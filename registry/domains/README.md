# Domain Registry Packs

Domain packs stage new capability families inside the existing Ginkgo Library without making them runtime authority.

They are additive registry metadata and remain subordinate to the canonical P0 architecture:

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

A domain pack may declare candidate capabilities, skills/bindings, tools, providers, policies, fallbacks and provenance in one reviewable unit while the feature is still `FEATURE`.

Rules:

- domain packs do not create a second CapabilityRequest/CapabilityResult contract;
- domain packs do not perform runtime routing;
- PostgreSQL remains runtime-state authority;
- public capabilities remain provider-agnostic;
- provider/tool metadata is never caller routing authority;
- `DISCOVERABLE != ROUTING_ELIGIBLE != PRODUCTION_ELIGIBLE`;
- FEATURE records are never production-routable;
- a pack must explicitly state lifecycle ceiling, provenance and promotion requirements;
- after validation/integration, records may be normalized into the canonical entity registries without changing their semantic IDs.

Current staged pack:

- `analytics.yaml` — Analytics Visual Intelligence v1, including Power BI as an internal binding candidate.
