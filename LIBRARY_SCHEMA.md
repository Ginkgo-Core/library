# Library Schema

Every curated item must be understandable by humans and queryable by machines.

## Canonical frontmatter

```yaml
id: unique-kebab-id
name: Human-readable name
aliases: []
kind: project
category: agents.frameworks
subcategory: null
status: curated
maturity: experimental
priority: medium

summary: One-sentence description.

canonical_url: https://github.com/example/project
repository_url: https://github.com/example/project
website_url: null
docs_url: null
license: Apache-2.0

open_source: true
self_hostable: true
commercial_service: false

integration_surfaces:
  api: true
  mcp: false
  cli: true
  sdk: true
  webhook: false
  docker: true

capabilities:
  - agents.tool-use
  - agents.memory

tags:
  - agent
  - open-source

canonical_owner: ginkgo-library
ginkgo_products:
  - ai-stack
  - sylvia
promotion_target: Ginkgo-Core/ai-stack

operational_profile:
  deployment: self-hosted
  data_leaves_environment: unknown
  gpu_required: unknown
  maintenance_burden: medium
  cost_profile: low

risk:
  security: medium
  vendor_lock_in: low
  maintenance: medium
  license: low

source_discovered_at: 2026-08-17
source_verified_at: 2026-08-17
last_curated_at: 2026-08-17

provenance:
  - type: clipping
    ref: local-or-repo-path
  - type: official_repo
    ref: https://github.com/example/project
```

## Required fields

The minimum for a `curated` item is:

```yaml
id:
name:
kind:
category:
status:
priority:
summary:
open_source:
self_hostable:
canonical_owner:
tags: []
source_verified_at:
```

Unknown values should be explicit (`unknown` or `null`) rather than guessed.

## Field definitions

### `id`

Stable lowercase kebab-case identifier. Never change it just because branding changes.

### `aliases`

Historical names, common abbreviations, repo names and terms an agent might search.

### `kind`

Allowed baseline values:

```text
project
platform
library
framework
runtime
model
skill
pattern
protocol
service
article
research
benchmark
dataset
template
```

### `category`

Uses `TAXONOMY.md`. Prefer one canonical primary category and many capabilities/tags rather than duplicating files across folders.

### `status`

```text
inbox
raw
triaged
curated
validated
promoted
watch
deprecated
archived
```

### `maturity`

```text
concept
experimental
prototype
production-candidate
production-proven
legacy
unknown
```

### `priority`

```text
critical
high
medium
low
```

Priority means relevance to Ginkgo, not general popularity.

### `integration_surfaces`

Describe how Ginkgo could programmatically use the item. Do not infer API/MCP support without evidence.

### `capabilities`

Machine-searchable functional abilities, e.g.:

```text
media.video.generate
media.avatar.consistency
agents.tool-use
automation.browser
data.knowledge-graph
infra.model-routing
app.crm
```

### `canonical_owner`

The conceptual owner of the capability in Ginkgo. Usually one of:

```text
ginkgo-library
ai-stack
ginkgo-agents
ginkgo-source
ginkgo-connect
ginkgo-pyre
ginkgo-wave
ginkgo-commerce-os
sylvia
leaf
scale-group
```

### `promotion_target`

Exact repository where adoption should occur, if known.

The library remains the research record after promotion.

## Recommended Markdown body

```markdown
# Name

## TL;DR

## Why it matters

## Core capabilities

## Ginkgo use cases

## Architecture

```mermaid
flowchart ...
```

## Integration surface

## Build vs Buy vs Reference

## Operational profile

## Security / privacy / licensing

## Alternatives

## Ginkgo destination

## Sources and provenance

## Verification notes
```

## Provenance rules

Raw discovery and canonical truth are different concepts.

Example:

```yaml
provenance:
  - type: youtube_clipping
    ref: 00-inbox/clippings/video-x.md
  - type: official_repo
    ref: https://github.com/owner/repo
  - type: official_docs
    ref: https://docs.example.com
```

A secondary clipping may explain **why we discovered something**. Official upstream usually establishes **what the thing currently is**.

## Duplicate rule

Many raw records may map to one canonical `id`.

```text
clipping A ─┐
clipping B ─┼─> hermes-agent.md
repo note ──┤
video note ─┘
```

Do not create `hermes-agent-2.md` because a second article was clipped.

## Date semantics

- `source_discovered_at`: when Ginkgo first captured it.
- `source_verified_at`: last time authoritative current sources were checked.
- `last_curated_at`: last meaningful synthesis/edit.

Do not update `source_verified_at` for cosmetic edits.

## Machine-readability constraints

- YAML frontmatter must parse cleanly.
- Dates use `YYYY-MM-DD`.
- Booleans use `true/false`, not prose.
- Unknown booleans use `unknown` only where schema permits string tri-state.
- Lists stay lists, even with one item.
- URLs are canonical, not tracking links when avoidable.
- Stable identifiers are lowercase ASCII kebab-case.
