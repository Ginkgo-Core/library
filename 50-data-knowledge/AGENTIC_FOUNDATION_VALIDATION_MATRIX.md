# Agentic Foundation — Validation Matrix

Status: `FEATURE / evaluation planning`

This matrix converts the repository atlas into evidence gates. It does not activate providers or declare runtime readiness.

## Rule

A popular upstream repository is not automatically a validated Ginkgo provider.

```text
UPSTREAM EXISTS
!= ADAPTER EXISTS
!= CONTRACT PASSES
!= GOVERNED E2E PASSES
!= ROUTING ELIGIBLE
```

## Wave 1

| Candidate | Ginkgo role | First capability | Current posture | Required proof before PROTOTYPE/VALIDATED |
|---|---|---|---|---|
| Firecrawl | external web-data provider | `web.search` | FEATURE / routing blocked | typed adapter, normalized errors, rate-limit/retry behavior, read-only fixture tests, governed E2E |
| Playwright | browser operation tool | `browser.session.execute` | FEATURE / routing blocked | sandbox, origin allowlist, managed session boundary, approval interlock, trace evidence, governed E2E |
| MarkItDown | local document conversion provider | `knowledge.document.convert` | FEATURE / routing blocked | scoped source resolver, format fixture corpus, malformed input tests, max-size enforcement, local E2E |
| PaddleOCR | local OCR/document AI provider | `document.structure.extract` | FEATURE / routing blocked | pinned model artifacts, resource caps, multilingual/layout fixtures, provenance, local E2E |
| Graphify | local knowledge-graph provider | `knowledge.graph.query` | FEATURE / routing blocked | graph artifact schema, stale graph policy, EXTRACTED/INFERRED preservation, limits, local E2E |
| WorldMonitor | external intelligence provider | `intelligence.events.search` | FEATURE / routing blocked | license/terms review, source attribution, freshness semantics, normalized errors, rate-limit behavior, governed E2E |

## Contract baseline already defined

The cross-repository first wave now targets these domain contract pairs in `Ginkgo-Core/ai-stack`:

- `web-search.v1` / `web-search-result.v1`
- `browser-session-execute.v1` / `browser-session-result.v1`
- `document-convert.v1` / `document-convert-result.v1`
- `document-structure-extract.v1` / `document-structure-result.v1`
- `knowledge-graph-query.v1` / `knowledge-graph-query-result.v1`
- `intelligence-events-search.v1` / `intelligence-events-result.v1`
- governed `memory-record.v1`

These contracts deliberately prevent caller-selected provider/tool/adapter/executor routing and inline credential fields.

## Evaluation scorecard

Score each candidate from `0` to `3` per axis.

| Axis | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| Semantic fit | wrong abstraction | partial | good | exact capability fit |
| Contract stability | unstable/opaque | weak | documented | stable + versionable |
| Isolation | uncontrolled | partial | sandboxable | strongly bounded |
| Provenance | absent | weak | usable | first-class/traceable |
| Error normalization | opaque | partial | mappable | deterministic |
| Resource control | absent | manual | configurable | policy-enforceable |
| Security posture | unknown | concerns | acceptable | strong controls/evidence |
| License posture | blocked/unknown | review needed | acceptable constraints | clear permissive/approved use |
| Locality/privacy | forced external | mixed | controllable | local-first when needed |
| Operability | brittle | manual | observable | production-operable |

### Thresholds

- `< 18`: keep as Library/reference only.
- `18–23`: prototype candidate; no production routing.
- `24–27`: validation candidate after gates.
- `28–30`: strong candidate, still requires governed E2E and reviewer approval.

A score never overrides a failed mandatory gate.

## Candidate-specific tests

### Firecrawl

Positive:
- query normalization;
- result limit enforcement;
- source URL preservation;
- publication timestamp normalization where upstream provides it.

Negative:
- caller passes `provider`/`endpoint`/`api_key`;
- invalid domain filters;
- provider timeout;
- 429/rate limit;
- malformed provider result.

### Playwright

Positive:
- read-only navigation inside allowed origin;
- deterministic screenshot/trace artifact reference;
- browser context isolation.

Negative:
- navigation outside allowlist;
- raw JavaScript/script injection as caller input;
- inline cookie/password/token;
- mutation without approval;
- artifact path escape;
- step/timeout exhaustion.

### MarkItDown

Positive:
- PDF/DOCX/PPTX/XLSX/HTML/text fixtures;
- stable Markdown structural expectations;
- metadata/provenance capture.

Negative:
- source outside scoped resolver;
- oversized input;
- corrupted document;
- plugin installation request;
- hidden network fetch when policy is local-only.

### PaddleOCR

Positive:
- scanned text;
- multilingual text;
- table/layout extraction;
- coordinate/confidence normalization.

Negative:
- excessive pages/pixels;
- unsupported/corrupt media;
- model/backend selection from caller;
- missing model checksum/pin.

### Graphify

Positive:
- known graph fixture;
- path/query results;
- preservation of `EXTRACTED` vs `INFERRED`;
- source-reference retention.

Negative:
- stale graph under strict freshness policy;
- arbitrary filesystem path;
- result node/edge limit exceeded;
- malformed graph artifact.

### WorldMonitor

Positive:
- attributed read-only events;
- event/observation time separation;
- freshness calculation;
- multi-source references.

Negative:
- missing source attribution;
- stale result when max age requested;
- 401/403/429/5xx normalization;
- caller transport/MCP tool selection;
- license/data-use gate unresolved.

## Memory track

`claude-mem` remains a pattern source, not the canonical Ginkgo memory provider.

Validation focuses on Ginkgo-native invariants:

- tenant/workspace scoping;
- source provenance;
- sensitivity classification;
- promotion state;
- retention/TTL;
- supersession;
- forget/invalidation state;
- retrieval disclosure bounded by caller authorization.

## Pi executor track

Pi is evaluated under the Execution Plane only.

Mandatory before any TaskOps eligibility:

1. sandbox boundary;
2. filesystem allowlist;
3. network policy;
4. credential isolation;
5. timeout/cancellation;
6. evidence capture;
7. TaskOps policy enforcement;
8. failure containment.

Pi does not become a Capability Plane provider merely because it can call tools.

## Promotion evidence package

Every promoted candidate should eventually have a GitHub evidence packet containing:

```text
source/version pin
license review
threat model
permission manifest
contract tests
negative tests
normalized error matrix
resource-limit tests
secret scan
supply-chain checks
read-only or local E2E evidence
observability evidence
reviewer approval
```

Until that packet exists, the default remains `routing_eligible: false`.
