# Taxonomy

The taxonomy is optimized for retrieval and architectural ownership, not academic completeness.

## Top-level categories

```text
10-agents
20-automation
30-models-routing
40-media
50-data-knowledge
60-app-platform
70-design
80-infrastructure
90-business-use-cases
```

## 10 — Agents

```text
agents.frameworks
agents.runtimes
agents.skills
agents.memory
agents.evaluation
agents.self-improvement
agents.coding
agents.research
agents.multi-agent
agents.tool-use
agents.context-management
```

Examples: Hermes Agent, OpenCode, Claude Code workflows, agent memory, skill registries.

## 20 — Automation

```text
automation.n8n
automation.workflow-engines
automation.browser
automation.queues-jobs
automation.integrations
automation.messaging
automation.webhooks
automation.scheduling
```

## 30 — Models & Routing

```text
models.gateways
models.routing
models.inference
models.local
models.coding
models.multimodal
models.embedding
models.reranking
models.cost-optimization
```

Examples: LiteLLM, OpenRouter patterns, local inference runtimes, model routers.

## 40 — Media

```text
media.image
media.video
media.audio-voice
media.avatars-identities
media.comfyui
media.creative-commerce
media.lipsync
media.upscaling
media.motion
```

Examples: Higgsfield, ComfyUI, TTS/ASR engines, media generation APIs.

## 50 — Data & Knowledge

```text
data.rag
data.vector-search
data.knowledge-graphs
data.databases
data.scraping-ingestion
data.document-processing
data.search
data.memory-store
data.analytics
```

## 60 — App & Platform

```text
app.frontend
app.backend
app.auth
app.payments
app.crm
app.saas-patterns
app.admin
app.communication
app.marketplace
app.cms
```

## 70 — Design

```text
design.systems
design.ux-review
design.motion
design.threejs
design.gsap
design.lottie
design.remotion
design.visual-regression
design.accessibility
design.prototyping
```

## 80 — Infrastructure

```text
infra.docker
infra.deployment
infra.observability
infra.queues
infra.storage
infra.networking
infra.security
infra.secrets
infra.compute
infra.gpu
infra.serverless
```

## 90 — Business Use Cases

```text
business.social-factory
business.commerce
business.real-estate
business.sales
business.workforce
business.customer-service
business.marketing
business.creator-economy
business.education
business.identity
business.legal
business.finance
```

## Capability naming

Capabilities describe what a technology can do, independent of folder.

Pattern:

```text
<domain>.<object>.<verb>
```

Examples:

```text
media.video.generate
media.identity.persist
content.copy.humanize
agents.code.execute
automation.browser.control
data.graph.build
infra.models.route
app.crm.self-host
```

A technology may have many capabilities but one primary category.

## Tags

Tags are flexible secondary retrieval keys. Useful classes:

```text
open-source
self-hostable
api
mcp
cli
docker
low-cost
gpu
local-first
cloud
production-candidate
reference-only
```

Avoid tag explosions. Prefer capabilities for functional search.

## Product ownership tags

Use `ginkgo_products` for product applicability:

```text
ai-stack
sylvia
ginkgo-source
ginkgo-agents
ginkgo-connect
ginkgo-pyre
ginkgo-wave
ginkgo-commerce-os
leaf
scale-group
ginkgo-library
```

These are not primary categories.

## When an item spans categories

Choose the category that represents its **core product identity**.

Example:

Higgsfield:

```yaml
category: media.video
capabilities:
  - media.video.generate
  - media.identity.persist
  - media.product-ad.generate
  - media.motion.control
```

Do not duplicate Higgsfield under four folders.

## New taxonomy entries

Add a new category only when:

- at least several items need it;
- existing categories distort retrieval;
- the new category has a clear semantic boundary.

Do not create categories for individual vendors.
