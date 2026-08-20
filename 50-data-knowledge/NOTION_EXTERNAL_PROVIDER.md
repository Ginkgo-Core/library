# Notion External Provider

Status: FEATURE / F1
Priority: P1
Runtime eligibility: false

## Role

Notion is a human-facing workspace, knowledge and work-management provider behind Ginkgo adapters.

It may host documents, project/task surfaces, operational notes, meeting outputs and curated knowledge candidates. It is not the canonical architecture/code source of truth and it is not the operational runtime state source of truth.

## Boundaries

- GitHub: architecture, contracts, code, versioned engineering docs.
- PostgreSQL/Supabase: operational application/runtime state.
- Redis: ephemeral state.
- Vector stores: semantic retrieval state when applicable.
- Notion: collaborative workspace and human-readable knowledge/work surface.

## Knowledge promotion

Content in Notion is a source or candidate, not automatically canonical knowledge.

```text
Notion content
-> governed read capability
-> provenance/evidence
-> Knowledge Promotion Pipeline
-> review/policy
-> canonical promoted knowledge
```

## Event-driven use

Notion webhooks may trigger event processing, but they remain signals only:

```text
Notion webhook
-> Event Gateway
-> Identity / Policy
-> TaskOps when work must be created/updated
-> Capability Bus
-> governed execution/evidence
```

## Initial bindings

- `knowledge.document.search` -> `notion-api.document.search`
- `knowledge.document.read` -> `notion-api.document.read`
- `knowledge.document.publish` -> `notion-api.document.publish`
- `work.task.create` -> `notion-api.task.create`
- `work.task.update` -> `notion-api.task.update`
- `work.project.read` -> `notion-api.project.read`
- `work.project.update` -> `notion-api.project.update`

All bindings are non-routable until implementation and governed evidence exist.

## Custom Agents

Notion Custom Agents are not part of the provider binding above. They are a P2 candidate for the separate External Executor path and require their own executor contract and validation.
