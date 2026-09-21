# Media Model Meritocracy v1

Status: research design. Runtime routing authority remains the ai-stack Domain Router.

## Principle

A caller requests a semantic capability. The runtime selects an eligible implementation. Model/provider names are never public routing controls.

```text
CapabilityRequest
  -> semantic compatibility gate
  -> policy / lifecycle / external-gate eligibility
  -> candidate implementations
  -> score
  -> chosen internal route
```

## Candidate scoring inputs

Suggested internal dimensions:

| Dimension | Example signal |
|---|---|
| semantics | exact operation/modalities supported |
| quality | validated task-specific eval score |
| latency | recent p50/p95 generation time |
| cost | normalized expected cost per output |
| privacy | local-only / governed-cloud eligibility |
| availability | provider/engine health |
| references | max images/video/audio references |
| output | aspect ratio, duration, resolution, format |
| reproducibility | seed/determinism support |
| reliability | recent normalized failure/retry rate |
| lifecycle | FEATURE/PROTOTYPE/etc ceiling |
| external gates | entitlement/billing/legal availability |

## Example: fast vertical social video

Internal-only illustrative ranking:

```text
LTX local          high latency/cost efficiency
Wan local          good privacy/cost, slower
Seedance cloud     strong quality/speed
Kling cloud        strong quality, higher expected cost
Veo                premium quality, usually unnecessary
```

The literal order is not canonical. Scores must come from current evaluation/telemetry evidence.

## Example: premium commercial

The weights shift toward quality, prompt adherence, motion quality, reference fidelity and text/brand requirements. A different implementation may win without changing the public capability (`media.video.generate`).

## Non-negotiable constraints

- provider/model/tool/adapter restriction cannot come from the caller;
- lifecycle ineligible candidates are removed before scoring;
- blocked External Gates remove candidates before scoring;
- fallback is `none` unless `same_semantics_only` is explicitly authorized;
- a cheaper implementation cannot win if it changes requested semantics;
- routing observations belong in audit/evidence, not the caller authority surface;
- provider/model discovery never promotes an implementation by itself.

## Telemetry record proposal

```yaml
implementation_ref: internal-binding-id
capability: media.video.generate
window: 24h
observations:
  success_rate: 0.98
  latency_p50_ms: 22000
  latency_p95_ms: 51000
  mean_cost_usd: 0.14
  quality_eval: 0.86
  health: healthy
```

Telemetry is runtime evidence. PostgreSQL is authoritative for current operational state; GitHub remains authoritative for architecture, registry and promotion evidence.
