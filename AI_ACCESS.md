# AI Access Guide

This file is the shortest entrypoint for AI systems reading or writing the Ginkgo Library.

## Objective

Find the smallest amount of context needed to answer a question or update the library without scanning the whole repository.

## Read paths by intent

### "Do we already know a tool for X?"

1. `registry/library.yaml`
2. `indexes/BY-CAPABILITY.md`
3. relevant curated record

### "What can we adopt now?"

1. `indexes/ADOPT-NOW.md`
2. `registry/library.yaml`
3. relevant curated records
4. `PROMOTION_POLICY.md`

### "What is self-hostable / open source / cheap?"

Use the corresponding index:

- `indexes/OPEN-SOURCE.md`
- `indexes/SELF-HOSTABLE.md`
- `indexes/LOW-COST.md`

### "What belongs in product X?"

1. `indexes/BY-GINKGO-PRODUCT.md`
2. relevant curated records
3. `architectures/ECOSYSTEM.md`

### "Ingest these new clippings"

1. `INGESTION_GUIDE.md`
2. `LIBRARY_SCHEMA.md`
3. `TAXONOMY.md`
4. `templates/ITEM_TEMPLATE.md`
5. source files in `00-inbox/`

### "Audit whether an existing record is still current"

1. curated record
2. its provenance/sources
3. official current sources
4. update `source_verified_at`
5. adjust lifecycle/status only when evidence supports it

## Machine-first files

The following files are designed for programmatic retrieval:

```text
registry/library.yaml
registry/categories.yaml
registry/products.yaml
```

Human-readable indexes are generated/maintained from the same canonical metadata.

## Search strategy

Prefer searching by these fields:

```text
id
name
aliases
category
capabilities
tags
canonical_owner
ginkgo_products
status
open_source
self_hostable
integration_surfaces
```

Avoid searching raw clipping prose first.

## Retrieval boundary

```mermaid
flowchart LR
    Q[Question] --> R[Registry]
    R --> I[Index]
    I --> C[Curated Record]
    C -->|need evidence| P[Provenance]
    P --> RAW[Raw Clipping / Upstream Source]
```

The further right the query goes, the more expensive the context becomes.

## Write strategy

When adding a new discovery:

```text
raw input
  ↓
00-inbox
  ↓
deduplicate
  ↓
identify canonical source
  ↓
classify
  ↓
create/update one canonical curated record
  ↓
update registry
  ↓
update relevant indexes
```

Do not create several curated files for several articles about the same technology.

## Context limits

Agents should not ingest entire 30k–100k word clippings unless necessary. Prefer:

- title and metadata;
- relevant sections;
- canonical source;
- targeted search/extraction;
- summarized provenance.

Raw source remains available for audit.

## Safe writing

Before any write:

- check for existing `id` or aliases;
- check canonical URL for duplicates;
- sanitize secrets and personal/client information;
- use lowercase kebab-case filename;
- preserve provenance;
- keep claims scoped to evidence.

## Output expectation

A good library answer should identify:

```text
best matching technology/pattern
status
why it fits
trade-offs
where it belongs in Ginkgo
source freshness
next action
```

The library is optimized for **decision retrieval**, not archival nostalgia.
