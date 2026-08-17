---
batch_id: YYYY-MM-source-name
processed_at: YYYY-MM-DD
status: processing
---

# Processing Report

## Batch metrics

```yaml
files_received: 0
files_processed: 0
exact_duplicates: 0
semantic_duplicates: 0
canonical_projects_identified: 0
curated_records_created: 0
curated_records_updated: 0
validation_candidates: 0
promotion_candidates: 0
watch_items: 0
archived_or_rejected: 0
security_findings: 0
```

## High-value discoveries

| Item | Category | Decision | Destination |
|---|---|---|---|

## Duplicates merged

Document raw sources that mapped into the same canonical record.

## Security/sanitization findings

Record any files that were excluded or require manual handling. Never paste discovered secrets into this report.

## Curated records created/updated

- `path/to/item.md`

## Items needing current web validation

- item — reason

## Promotion candidates

- item — target repo — next action

## Remaining work

- [ ] ...

## Completion

Set frontmatter `status: complete` only when the batch has been inventoried and every raw item has a triage disposition.
