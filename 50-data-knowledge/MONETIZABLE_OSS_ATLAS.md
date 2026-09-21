# Monetizable OSS Atlas v1

Status: FEATURE metadata / non-runtime  
Tracking: `Ginkgo-Core/ginkgo-library#5`  
Research date: 2026-08-20

## Thesis

Ginkgo should monetize measurable business outcomes built with open-source leverage, not resell upstream projects as undifferentiated products.

```text
open-source leverage
+ stable Ginkgo capabilities
+ governed adapters
+ customer/proprietary data
+ integration with real processes
+ distribution
+ observability
+ ongoing operation
= monetizable system
```

The Library records provenance, license posture, adoption boundaries and evidence requirements. It does not make an implementation routing-eligible.

## Canonical gate

```text
UPSTREAM EXISTS
!= LICENSE CLEARED
!= ADAPTER EXISTS
!= CONTRACT PASSES
!= GOVERNED E2E PASSES
!= ROUTING ELIGIBLE
!= PRODUCTION ELIGIBLE
```

And:

```text
CAPABILITY != SKILL != BINDING != TOOL != ADAPTER != PROVIDER != EXECUTOR
```

## Priority model

### P0 — immediate architecture/product leverage

| Upstream | Ginkgo source role | License posture | Adoption boundary |
|---|---|---|---|
| n8n-io/n8n | EXECUTOR | Sustainable Use License / fair-code | Internal business, consulting and governed execution. Do not assume white-label or hosted multi-tenant rights. |
| langflow-ai/langflow | ADAPTER_RUNTIME_CANDIDATE | MIT | Evaluate behind Ginkgo semantic contracts. No public `langflow.*` business capability. |
| Comfy-Org/ComfyUI | MEDIA_RUNTIME_CANDIDATE | GPL-3.0 | Keep behind Creative/Pyre; model/custom-node licenses remain separate gates. |
| harry0703/MoneyPrinterTurbo | PATTERN_SOURCE / LAB_ADAPTER_CANDIDATE | MIT | Extract pipeline patterns; Ginkgo-native content capabilities remain the product boundary. |
| asiifdev/business-leads-ai-automation | PATTERN_SOURCE | MIT | Extract discovery/enrichment/outreach patterns. Upstream scraping behavior is not Ginkgo authorization. |

### P1 — portability, agent runtime and quality leverage

| Upstream | Ginkgo source role | License posture | Adoption boundary |
|---|---|---|---|
| browser-use/browser-use | ADAPTER_PROVIDER_CANDIDATE | MIT | Candidate for `browser.task.execute` after typed contract, permission/security tests and governed E2E. |
| activepieces/activepieces | EXECUTOR_CANDIDATE | CE described upstream as MIT; GitHub metadata can be NOASSERTION | Verify CE/enterprise file boundary before packaging or redistribution. |
| crewAIInc/crewAI | EXECUTOR_RUNTIME_CANDIDATE | MIT | Evaluate behind Executor Router. Cannot own Ginkgo provider/domain routing. |
| OpenHands/OpenHands | ENGINEERING_EXECUTOR_CANDIDATE | Core MIT; enterprise surfaces separate | Bounded engineering execution under TaskOps/Agent Foundry hard stops. |
| Agenta-AI/agenta | EVAL_OBSERVABILITY_CANDIDATE | MIT | Optional Foundry backend; Ginkgo eval/evidence contracts remain canonical. |
| langgenius/dify | CONDITIONAL_ADAPTER_RUNTIME_CANDIDATE | Modified Apache 2.0 with additional restrictions | Shared/multi-tenant or branding-sensitive deployment needs explicit license/commercial review. |
| enescingoz/awesome-n8n-templates | DISCOVERY_SOURCE | repository metadata NOASSERTION / path-specific review required | Every imported workflow needs independent provenance/license review. |

### P2 — benchmark/reference/lab

| Upstream | Ginkgo source role | License posture | Adoption boundary |
|---|---|---|---|
| lobehub/lobehub | OPERATOR_SURFACE_BENCHMARK | LobeHub Community License | Benchmark UX/operator patterns; no blind derivative distribution. |
| RayVentura/ShortGPT | PATTERN_SOURCE | MIT | Historical/experimental content pipeline reference, not default runtime dependency. |
| brightdata/ai-lead-generator | REFERENCE_SOURCE | GitHub repo has no standard license metadata; README states educational/internal demo use | Do not vendor as a production base. Bright Data itself is a separate commercial provider decision. |

## Architecture classification

### EXECUTOR

Executes operational work selected by the Executor Router. It is not a Provider merely because it can call APIs or models.

Candidates:

- n8n;
- Activepieces;
- CrewAI in an internal multi-agent execution role;
- OpenHands for bounded engineering tasks.

### ADAPTER_RUNTIME_CANDIDATE

A replaceable implementation surface behind semantic Ginkgo capability contracts.

Candidates:

- Langflow;
- Dify, conditionally;
- Browser Use for the distinct browser task capability.

### MEDIA_RUNTIME_CANDIDATE

Implementation runtime behind Media/Creative routing.

Candidate:

- ComfyUI.

### PATTERN_SOURCE

Architecture/workflow patterns may be extracted; upstream identity is not a runtime dependency or public capability.

Candidates:

- MoneyPrinterTurbo;
- ShortGPT;
- business-leads-ai-automation.

### REFERENCE_SOURCE

Useful for learning/comparison but unsuitable for direct adoption without re-engineering/legal clearance.

Candidate:

- Bright Data ai-lead-generator.

### DISCOVERY_SOURCE

Collection used to discover candidate workflows/components. Each discovered item has its own provenance decision.

Candidate:

- awesome-n8n-templates.

### OPERATOR_SURFACE_BENCHMARK

Used to evaluate UX/operator patterns, not to establish runtime authority.

Candidate:

- LobeHub.

## Product mappings

### Revenue Agent / RevenueOps

Primary Ginkgo capability family:

```text
sales.lead.discover
sales.lead.normalize
sales.lead.enrich
sales.lead.qualify
sales.intent.score
sales.outreach.compose
sales.sequence.plan
sales.meeting.prepare
sales.pipeline.record
sales.attribution.calculate
sales.forecast.calculate
sales.funnel.diagnose
```

Relevant sources:

- business-leads-ai-automation -> pattern source;
- Bright Data lead generator -> reference source;
- n8n/Activepieces -> potential executors;
- CRM/Calendar/WhatsApp/email/data vendors -> separate adapters/providers.

### Vertical Content Factory

Capability candidates:

```text
marketing.content.brief.generate
marketing.content.script.generate
media.voice.generate
media.asset.select
media.video.compose
marketing.content.package
marketing.content.publish
analytics.content.performance.read
```

Relevant sources:

- MoneyPrinterTurbo -> pattern source / optional lab adapter;
- ShortGPT -> pattern source;
- ComfyUI -> internal Media runtime behind existing Creative/Pyre boundary.

### Backoffice Browser Operations

Capabilities:

```text
browser.session.execute
browser.task.execute
```

Relevant sources:

- Playwright -> deterministic session candidate already incubated elsewhere;
- Browser Use -> goal-directed task candidate.

These capabilities are not silent fallbacks for each other.

### Agent/runtime portability

Relevant sources:

- Langflow;
- Dify;
- CrewAI;
- OpenHands;
- Agenta.

They must remain implementation/evaluation candidates rather than public business capability namespaces.

## Commercial adoption policy

Before Ginkgo reuses, distributes, hosts or derives from an upstream project, the registry must answer:

1. what exact upstream commit/version is being evaluated?
2. what license applies to the exact files/components used?
3. is commercial internal use allowed?
4. is redistribution allowed and under what obligations?
5. is SaaS/multi-tenant use restricted?
6. are branding notices/removal constrained?
7. are enterprise-only directories/features separated?
8. do bundled models/datasets/assets/custom nodes have different licenses?
9. are third-party APIs governed by separate terms/entitlements?
10. what evidence is required before lifecycle promotion?

A permissive repository license does not automatically clear models, datasets, images, music, voices, fonts, templates or API terms.

## Source-specific notes

### n8n

Treat as executor/orchestrator. Its fair-code posture makes portability strategically important. n8n workflow JSON is an execution artifact; PostgreSQL/TaskOps remains operational truth.

### Activepieces

Potential alternate executor. Validate Community Edition licensing at file/component level before distribution. Enterprise functionality must not be silently incorporated.

### Langflow

Strong candidate for dedicated/internal agent/RAG adapter experiments because of MIT licensing and visual composition, but graph/component IDs remain implementation details.

### Dify

Useful runtime/operator candidate for dedicated deployments. Additional license restrictions make explicit commercial review mandatory before shared SaaS or branding-dependent deployments.

### CrewAI

Potential multi-agent executor. Its planner/agents cannot become a second Ginkgo Domain Router or Provider Router.

### OpenHands

Potential engineering executor for Agent Foundry. Default authority excludes merge, release, deploy, secrets, permission escalation and production mutation.

### Browser Use

Candidate implementation of `browser.task.execute`; must respect allowed origins, bounded actions, evidence, credentials boundary and approval for side effects. Stealth/anti-bot bypass is not a Ginkgo authorization surface.

### ComfyUI

Already valuable in the Creative stack. Keep graphs/checkpoints/nodes isolated behind Pyre/adapter boundaries. Audit each model/custom node separately.

### MoneyPrinterTurbo / ShortGPT

Use as pipeline pattern sources. Ginkgo's defensible layer is vertical workflow, quality/provenance, distribution, attribution and ROI—not generic video volume.

### Lead-generation examples

Do not equate scraping code with compliant lead acquisition. Sources, lawful-use basis/consent where applicable, suppression, opt-out and channel policies remain explicit Ginkgo gates.

## Promotion requirements

No source record becomes implementation-eligible merely by entering the Library.

Minimum evidence progression:

```text
source_recorded
-> license_decision_recorded
-> semantic_mapping_defined
-> implementation_or_adapter_prototyped
-> deterministic_positive_tests
-> negative/security_tests
-> governed_e2e
-> lifecycle_review
```

Every newly curated source in this pack remains non-routing and non-production until a separate evidence-backed promotion decision.
