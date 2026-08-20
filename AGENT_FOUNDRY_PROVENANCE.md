# Agent Foundry — Provenance & Reuse Policy v1

Status: **FEATURE / REGISTRY POLICY**

This document defines how external agent-skill repositories are used as research inputs for the Ginkgo Agent Foundry.

## Core rule

```text
EXTERNAL SOURCE
-> immutable pin
-> provenance record
-> license decision
-> concept extraction
-> original Ginkgo specification
-> Ginkgo eval evidence
-> promotion review
```

External popularity, stars, a green upstream CI or production use by the upstream project do **not** validate a Ginkgo skill.

## Reuse classes

### 1. Concept-derived adaptation

Allowed by default for this feature pack.

Ginkgo may study architecture, workflow boundaries, testing ideas, failure modes and naming patterns, then write an original Ginkgo specification aligned to Ginkgo contracts.

The provenance record must retain the upstream repository and pinned commit that influenced the design.

### 2. Text/code reuse

Not allowed by default.

Before copying source, skill text, scripts, templates or fixtures, the exact source path must have:

- an identifiable license or permission covering the intended reuse;
- required attribution/notice obligations recorded;
- compatibility with the target Ginkgo repository/license confirmed;
- a security review when executable content is involved.

Repository-level license metadata is useful evidence but does not override path-specific notices or source-available exceptions.

### 3. Vendoring

Vendoring third-party skills into the Ginkgo runtime is out of scope for Agent Foundry v1. A future vendoring decision requires a dedicated dependency/provenance record, immutable version pin, security scan, update strategy and rollback/removal plan.

## Special handling — `anthropics/skills`

The source repository explicitly contains skill groups with different reuse terms. Ginkgo therefore records its license model as `mixed_or_path_specific` and makes **no repository-wide reuse assumption**.

Conceptual patterns such as `SKILL.md` packaging, metadata-driven discovery and dynamic loading may inform Ginkgo architecture. Copying a concrete skill implementation requires a path-specific license review first.

## Evidence separation

```text
SOURCE PROVENANCE != VALIDATION EVIDENCE
```

A provenance record answers:

- where the idea came from;
- what commit was inspected;
- what reuse rule applies.

Validation evidence answers:

- whether the Ginkgo-owned package behaves as claimed;
- which version/commit was tested;
- which scenarios passed or failed.

Neither becomes runtime Knowledge automatically.

## Promotion minimum

A source-derived Agent Foundry feature cannot become `VALIDATED` only because the upstream source is mature. It requires Ginkgo-owned evidence for the Ginkgo contract, including applicable:

- baseline behavior;
- retrieval/application behavior;
- pressure/adversarial scenarios;
- permission and mutation boundaries;
- context-budget behavior;
- regression behavior;
- reviewer approval.

## Current pinned sources

The machine-readable source of truth is:

`registry/sources/agent-skill-upstreams.yaml`

Current source families:

- `anthropics/skills`
- `mattpocock/skills`
- `obra/superpowers`
- `affaan-m/ECC`
- `ultraworkers/claw-code`

No upstream code or skill text is vendored by this feature branch.
