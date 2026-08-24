# Open-Generative-AI — Extraction Map

Date: 2026-08-24
Upstream: `Anil-matcha/Open-Generative-AI`
Status: research / source pattern; not runtime authority.

## Executive finding

Open-Generative-AI is best understood as a creative product shell that combines a large model catalog, Studio UX, Vibe Workflow, agent UI/design canvas and local inference. Much of the cloud workflow/agent execution remains behind MuAPI. The useful strategy is therefore **pattern extraction**, not wholesale adoption.

## Architecture observed

```text
Open-Generative-AI
├── Next.js / React Studio
├── model schema registry
├── MuAPI client + submit/poll normalization
├── Vibe Workflow submodule
├── agent hub submodule
├── design-agent submodule
└── Electron local inference
    ├── stable-diffusion.cpp
    └── Wan2GP adapter
```

The root project uses Git submodules for Vibe Workflow, Open-Poe/Open AI Agents and Open-AI-Design-Agent. This makes the repository an integration shell rather than one fully self-contained engine.

## High-value patterns to extract

### 1. Schema-driven model catalog

The upstream Studio carries a very large generated `models.js` registry. Records describe model id, endpoint/provider and model-specific input schemas. This allows one UI to render controls for many models without hardcoding every form.

Ginkgo translation:

```text
capability
  -> internal skill/binding
  -> provider adapter
  -> model observation
```

Model metadata is internal routing input, never caller authority.

### 2. Async generation contract

Upstream normalizes generation around:

```text
submit -> request_id -> poll -> completed/failed -> normalized output
```

Ginkgo translation: `MediaJob` with QUEUED/RUNNING/SUCCEEDED/FAILED/CANCELLED state, correlation IDs, cost, progress, normalized error and asset refs.

### 3. Agent event protocol

The Design Agent client processes events equivalent to:

- text/info/error;
- tool_call/tool_result;
- plan_propose;
- canvas_op;
- approve/reject/cancel;
- incremental job-event polling.

Ginkgo translation is the canonical `creative.*` event vocabulary in ai-stack Media Runtime v1.

### 4. Asset labels and lineage

Uploaded/generated assets receive stable labels and can become inputs to later edit/animation operations. Derived results can be placed beside the source asset.

Ginkgo translation: Asset Ledger with durable `asset_id`, `producer_job_id`, `source_asset_refs`, provenance and normalized generation metadata.

### 5. Wan2GP discovery adapter

Wan2GP function/API names drift across builds. The upstream adapter probes the Gradio server and resolves function names using exact name -> aliases -> safe family match.

Ginkgo translation: generic provider discovery observation. Discovery never implies routing eligibility.

### 6. Local inference runtime

Electron manages stable-diffusion.cpp binaries/models and captures generation progress from stdout/stderr. This is genuine local inference rather than a remote proxy.

Ginkgo translation: `StableDiffusionCppProvider` behind the same internal `MediaProvider` protocol as cloud providers.

### 7. Visual media DAG

Vibe Workflow provides a React Flow graph with typed text/image/video/audio/API nodes and workflow persistence.

Ginkgo translation:

```text
visual graph -> workflow IR -> compiler -> n8n | native DAG | agent executor
```

Every domain operation still crosses the Capability Bus.

## Do not inherit

### MuAPI as architectural authority

The Vibe Workflow FastAPI helpers and agent/design-agent servers largely proxy execution to `api.muapi.ai`. MuAPI can remain one replaceable provider/binding, but must not become Ginkgo's workflow or agent authority.

### Caller/UI provider selection

Ginkgo's frozen invariants prohibit caller-controlled provider/adapter/model/tool selection. Provider/model information belongs to internal routing metadata.

### Long-lived API keys in renderer/localStorage

Keys/secrets should use governed secret references and server/runtime boundaries.

### Megacomponents

The upstream Studio, workflow graph and canvas contain very large JSX modules. Extract contracts, flows and UX patterns rather than copying monolithic components.

### Unverified executable/model download

The local downloader fetches executable/model artifacts and prepares them for execution. Ginkgo requires expected digest/provenance verification before install/execute.

## Security notes

Positive patterns:

- Electron uses `webSecurity: true`, `contextIsolation: true`, `nodeIntegration: false`;
- preload exposes a narrow IPC bridge;
- integrated upload proxy validates auth, HTTPS target and allowed S3/host targets and blocks dangerous upload types;
- identity-scoped persistence was added to prevent browser-state leakage across users.

Hardening requirements before reuse:

- remove/avoid permissive CSP `unsafe-eval` / `unsafe-inline` where possible;
- checksum/signature/provenance verification for downloaded executables and weights;
- provider URL allowlists for all proxy/upload paths;
- server-side secret ownership;
- consistent normalized error and audit events.

## Ginkgo landing map

| Upstream pattern | Ginkgo destination | Priority |
|---|---|---:|
| model schema registry | ginkgo-runtime + source registries | P0 |
| submit/poll normalization | ai-stack MediaJob + runtime | P0 |
| local provider adapters | ginkgo-runtime | P0 |
| creative event protocol | ai-stack + ginkgo-agents | P0 |
| asset lineage | ai-stack Asset Ledger | P0 |
| provider discovery | ginkgo-runtime | P0 |
| visual media workflow | workflow/media layer | P1 |
| creative canvas | UI/product layer | P1 |
| Studio UX | selective product reuse | P1 |
| Electron desktop shell | optional product surface | P2 |

## Source-of-truth rule

This document is research evidence. Canonical contracts live in `Ginkgo-Core/ai-stack`; canonical capability/skill/provider registry records live in Ginkgo registries; runtime implementation lives in `Ginkgo-Core/ginkgo-runtime`.
