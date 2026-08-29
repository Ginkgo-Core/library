---
id: comfyui
name: ComfyUI
aliases: [Comfy UI]
kind: runtime
category: media.video
subcategory: generative-workflows
status: promoted
maturity: production-candidate
priority: critical
summary: Node-based local generative media runtime already integrated in Ginkgo ai-stack as an on-demand creative GPU service.
canonical_url: https://github.com/comfyanonymous/ComfyUI
repository_url: https://github.com/comfyanonymous/ComfyUI
website_url: https://www.comfy.org/
docs_url: https://docs.comfy.org/
license: GPL-3.0
open_source: true
self_hostable: true
commercial_service: false
integration_surfaces:
  api: true
  mcp: false
  cli: true
  sdk: false
  webhook: false
  docker: true
capabilities:
  - media.image.generate
  - media.image.transform
  - media.video.generate
  - media.workflow.execute
  - media.model.local.execute
  - media.control.condition
  - media.avatar.generate
tags: [image, video, gpu, workflow, nodes, local, diffusion, self-hosted]
canonical_owner: ai-stack
ginkgo_products: [ai-stack, ginkgo-pyre, sylvia]
promotion_target: Ginkgo-Core/ai-stack
operational_profile:
  deployment: self-hosted-local-gpu
  data_leaves_environment: no-by-default
  gpu_required: true
  maintenance_burden: high
  cost_profile: local-compute
risk:
  security: medium
  vendor_lock_in: low
  maintenance: high
  license: medium
source_discovered_at: 2026-08-17
source_verified_at: 2026-08-29
last_curated_at: 2026-08-29
provenance:
  - type: ginkgo_runtime
    ref: Ginkgo-Core/ai-stack/PROJETO/COMFYUI_RUNTIME.md
  - type: official_repo
    ref: https://github.com/comfyanonymous/ComfyUI
---

# ComfyUI

## TL;DR

ComfyUI is the **local/GPU generative execution engine** in the Ginkgo media toolchain. Unlike HeliosGen and Donkey Cut, it is already present in `Ginkgo-Core/ai-stack` and therefore has a promoted library record.

## Current Ginkgo runtime

The ai-stack runs ComfyUI as an on-demand `creative` Docker profile, loopback-only on `127.0.0.1:8188`, with GPU access and external model/data volumes. It is deliberately not exposed publicly and does not auto-start with the base stack.

The canonical runtime documentation remains:

```text
Ginkgo-Core/ai-stack/PROJETO/COMFYUI_RUNTIME.md
```

This library record does not override that operational source of truth.

## Canonical Ginkgo role

```mermaid
flowchart LR
    C[Capability Bus] --> DR[Domain Router]
    DR --> S[media-generation skill]
    S --> A[ComfyUI adapter]
    A --> CUI[ComfyUI API]
    CUI --> GPU[Local GPU]
    GPU --> M[External model assets]
```

ComfyUI is an implementation behind an adapter. A caller must never select it directly.

## Relationship with HeliosGen

HeliosGen and ComfyUI are parallel generation implementation candidates:

- **HeliosGen:** easier multi-provider/cloud workflow UX;
- **ComfyUI:** deeper local model graph and GPU control.

The Domain Router may make either eligible for a capability based on internal policy, lifecycle and execution requirements. They are not automatic fallbacks for one another.

## Relationship with Donkey Cut

ComfyUI produces specialized image/video assets; Donkey Cut composes and edits assets on a timeline. A higher-level skill may chain both through normalized media artifacts.

```text
ComfyUI -> generated asset -> normalized artifact -> Donkey Cut -> edited export
```

## Guardrails already adopted

- on-demand runtime only;
- loopback API only;
- no model weights in Git;
- external persistent model/input/output storage;
- custom-node allowlist with pin/license/security review requirements;
- reproducible container/image pins;
- no assumption that Pyre or any caller owns the runtime.

## Next integration step

The next step is not another ComfyUI installation. It is a governed **media adapter contract** that normalizes ComfyUI, HeliosGen and Donkey Cut behind provider-agnostic capabilities and evidence.
