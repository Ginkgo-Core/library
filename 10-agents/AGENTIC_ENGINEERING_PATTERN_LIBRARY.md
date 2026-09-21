# Agentic Engineering Pattern Library

Status: curated reference; not runtime configuration.

This note extracts reusable patterns from ECC, gstack, Pi, claude-mem and agent-operator projects into Ginkgo design guidance.

## Pattern 1 — Engineering as a staged system

Source influences:

- https://github.com/affaan-m/ECC
- https://github.com/garrytan/gstack

A strong coding agent should not be modeled as a single “write code” action. The useful pattern is a staged process with explicit intent boundaries:

```text
understand
-> challenge assumptions
-> plan
-> architecture review
-> implement
-> code review
-> QA
-> security/release validation
-> ship decision
-> retrospective
```

Ginkgo translation: expose stable engineering capabilities and let TaskOps/Executor Router choose the executor. Do not expose host-specific slash commands as the cross-system contract.

## Pattern 2 — Research before mutation

A recurring failure mode in autonomous coding systems is patch-first behavior: a symptom appears, the agent changes code immediately, and three patches later nobody knows the root cause.

Ginkgo rule:

```text
engineering.investigate
!=
engineering.implement
```

Investigation should return hypotheses, tests, eliminated causes and a probable root cause. Mutation requires a separate authorized task or explicit mutation contract.

## Pattern 3 — Evidence is part of the result

An agent saying “done” is not evidence.

Useful evidence includes:

- exact repo/ref inspected;
- files changed or reviewed;
- tests executed;
- commands/checks executed;
- screenshots or browser observations where applicable;
- CI status;
- external sources;
- known skipped checks;
- residual risks.

The evidence envelope should travel with `CapabilityResult` rather than living only in prose.

## Pattern 4 — Memory needs promotion, not accumulation

Source influence: https://github.com/thedotmack/claude-mem

Persistent capture is useful, but raw persistence alone creates a context landfill. A better lifecycle is:

```text
observation
-> compression
-> candidate
-> promotion policy
-> durable memory
-> scoped retrieval
```

Important properties:

- provenance survives compression;
- memory has scope;
- inferred content is tagged;
- durable promotion is explicit/policy-driven;
- deletion invalidates indexes;
- retrieval uses progressive disclosure.

## Pattern 5 — Executor is replaceable

Source influence: https://github.com/earendil-works/pi

An executor may offer a CLI, runtime, model abstraction, tool calling and state. None of those make it the public Ginkgo contract.

```text
TaskOps
-> Executor Router
-> eligible executor
-> bounded task envelope
-> evidence
```

This makes it possible to compare Codex, Claude Code, OpenCode, Hermes, OpenClaw, Pi or future runtimes without rewriting public capabilities.

## Pattern 6 — Sandbox before autonomy

Any executor that can reach filesystem, process, network or credentials needs a hard boundary. “The prompt says not to touch it” is not a security architecture.

Evaluation checklist:

- mount scope;
- process isolation;
- network policy;
- credentials injection/rotation;
- command audit;
- timeouts;
- cancellation;
- output capture;
- resource limits;
- artifact handling.

## Pattern 7 — Agent identity != capability identity

Operator products often place named agents at the center of UX. That is useful for humans, but the backend contract should remain capability-centric.

Example:

```text
Callima requests engineering.code.review
Hermes requests engineering.code.review
WhatsApp TaskOps requests engineering.code.review
```

All three should hit the same capability contract. The caller identity affects policy and permissions, not capability semantics.

## Pattern 8 — Skills should compose

A large “super skill” is hard to validate. Prefer small skills whose outputs can feed the next stage.

Example composition:

```text
engineering.plan
-> engineering.architecture.review
-> implementation task
-> engineering.code.review
-> engineering.qa
-> engineering.release.validate
```

Each stage can fail independently and return bounded evidence.

## Pattern 9 — Discovery must be quarantined from routing

Catalogs such as awesome MCP lists are useful to discover tools. They must not update provider eligibility automatically.

Discovery plane:

```text
catalog -> candidate -> review -> FEATURE
```

Runtime plane:

```text
validated registry -> policy -> Domain Router
```

The two datasets may be linked, but one does not directly control the other.

## Pattern 10 — Learn from operator products without inheriting their monolith

Open WebUI, LobeHub, Dify and Langflow contain valuable ideas around collaboration, model/tool UX, RAG, workflow building and observability. The Ginkgo approach should harvest those UX/operational patterns while retaining its four-plane architecture.

## Proposed Ginkgo engineering skill suite

```text
engineering.plan
engineering.architecture.review
engineering.code.review
engineering.security.audit
engineering.qa
engineering.investigate
engineering.release.validate
engineering.retro
engineering.freeze
engineering.devex.review
```

## Acceptance rule for a new engineering skill

A candidate engineering skill should not enter `VALIDATED` until it has:

1. bounded semantics;
2. typed input/output;
3. mutation posture;
4. permission posture;
5. evidence contract;
6. deterministic tests;
7. negative tests;
8. at least one real repository trial;
9. no hidden provider/executor selection by caller;
10. reviewer approval.
