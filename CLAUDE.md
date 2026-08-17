# CLAUDE.md — Ginkgo Library

Use `AGENTS.md` as the authoritative operating policy for all AI agents in this repository.

## Repository role

This is a **neutral curated intelligence library**, not a production runtime.

Primary jobs:

- ingest raw discoveries;
- normalize and deduplicate them;
- build canonical technology records;
- document use cases and Mermaid architectures;
- maintain provenance;
- classify maturity and promotion targets;
- support AI retrieval and future architectural decisions.

## Context strategy

Read only the minimum useful context:

```text
AI_ACCESS.md
  ↓
registry/library.yaml
  ↓
relevant index
  ↓
relevant curated record
  ↓
raw source only if required
```

Do not load the full clipping corpus for routine questions.

## Editing constraints

- Preserve stable IDs.
- Never commit secrets.
- Keep raw and curated layers distinct.
- Prefer canonical upstream sources over secondary clippings.
- Add Mermaid only when it improves understanding.
- Keep machine-readable metadata synchronized with human documentation.
- Do not promote experimental tools to production status without evidence.

## Naming

Canonical curated files use lowercase kebab-case.

Examples:

```text
hermes-agent.md
higgsfield.md
awesome-llm-apps.md
open-webui.md
```

## Desired output quality

Curated records should be concise enough for retrieval but complete enough to make an architectural decision without reopening every source.
