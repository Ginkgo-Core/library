# Contributing to Ginkgo Library

## Contribution types

- new clipping batch;
- new curated technology record;
- update to an existing record;
- validation/benchmark evidence;
- new architecture or comparison;
- status/promotion update;
- taxonomy/index maintenance.

## New discovery

For unprocessed material, place it under:

```text
00-inbox/clippings/<batch>/
```

Do not pretend raw material is curated.

## New curated item

1. Search for an existing `id`, alias, canonical URL or GitHub repo.
2. Choose one primary category from `TAXONOMY.md`.
3. Copy `templates/ITEM_TEMPLATE.md`.
4. Fill required frontmatter from `LIBRARY_SCHEMA.md`.
5. Preserve provenance.
6. Add/update `registry/library.yaml`.
7. Update relevant indexes.
8. Add Mermaid if architecture materially benefits.

## Editing existing items

Prefer improving the canonical record instead of creating a second file.

When changing:

- status;
- category;
- owner;
- product applicability;
- source freshness;

also update the registry/indexes.

## Commit style

Examples:

```text
library: add Higgsfield curated record
library: ingest local vault batch 2026-08
library: merge duplicate Hermes clippings
library: validate Graphify source and license
library: promote creative router pattern to Pyre
```

## Review checklist

- [ ] no secrets or sensitive data;
- [ ] canonical source identified when possible;
- [ ] duplicate search performed;
- [ ] frontmatter parses;
- [ ] facts vs inference are clear;
- [ ] license/data risks recorded;
- [ ] Ginkgo use case is explicit;
- [ ] owner/promotion target makes sense;
- [ ] registry/indexes updated;
- [ ] raw provenance retained when relevant.

## Pull request scope

Prefer coherent batches:

- one technology + related indexes;
- one clipping batch ingestion;
- one taxonomy migration;
- one validation wave.

Avoid mixing unrelated cleanup and new research in one PR when review would become difficult.
