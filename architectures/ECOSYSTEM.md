# Ginkgo Technology Intelligence Ecosystem

## Role separation

```mermaid
flowchart TD
    EXT[External world<br/>GitHub / docs / articles / videos / tools] --> LIB[Ginkgo Library]

    LIB --> SRC[Ginkgo Source]
    LIB --> AG[Ginkgo Agents]
    LIB --> CON[Ginkgo Connect]
    LIB --> PYRE[Pyre]
    LIB --> STACK[ai-stack]
    LIB --> SYL[Sylvia]

    SRC --> PRODUCTS[Products / client solutions]
    AG --> PRODUCTS
    CON --> PRODUCTS
    PYRE --> PRODUCTS
    STACK --> PRODUCTS
    SYL --> PRODUCTS

    PRODUCTS --> OBS[Operational evidence]
    OBS --> LIB
```

## Mental model

```text
Library = memory of what exists and what we know
Source = architect/composer of solutions
Agents = reusable cognitive capabilities
Connect = governed external context
Pyre = creative/media engine
ai-stack = shared runtime/workshop
Sylvia = orchestrator
Products = business implementations
```

## Promotion direction

The library should not become a runtime dependency of products. It produces knowledge and promotion decisions.

```text
research record
   ↓
validated capability
   ↓
issue / PR / implementation in canonical owner repo
   ↓
product consumes stable owner contract
```

## Feedback direction

Operational evidence can update library knowledge:

```text
provider cost
benchmark
failure rate
conversion impact
maintenance burden
security issue
upstream abandonment
```

This keeps the library alive rather than a frozen bookmarks folder.

## Example — media provider

```mermaid
flowchart LR
    L[Ginkgo Library<br/>research + comparison] --> P[Pyre<br/>canonical media owner]
    P --> R[Creative Router]
    R --> H[Higgsfield adapter]
    R --> C[ComfyUI adapter]
    R --> F[Other provider]
    P --> W[Wave]
    P --> O[Commerce OS]
    P --> S[Scale Group]
```

The library documents Higgsfield. Pyre integrates it. Consumers do not each implement it independently.

## Example — coding executor

```mermaid
flowchart LR
    L[Ginkgo Library] --> A[Ginkgo Agents<br/>evals + patterns]
    A --> S[Sylvia]
    S --> R[Executor Router]
    R --> O[OpenCode]
    R --> C[Claude Code]
    R --> X[Other executor]
```

Again: research and architecture remain separable from runtime implementation.
