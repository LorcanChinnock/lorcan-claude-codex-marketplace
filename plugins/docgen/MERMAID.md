# MERMAID.md — diagram conventions for all docgen skills

Diagrams earn their place by showing something prose cannot. A diagram that restates a sentence is clutter. When you do include one, use the rules below so the output looks consistent across docs and imports cleanly into Excalidraw.

## Allowed diagram types

Only these four. No state diagrams, no gantt, no mindmap, no git graph, no pie charts, no journey.

| Type | When to use | Mermaid directive |
|---|---|---|
| Flowchart | Service topology, high-level architecture, pipeline stages, decision flow | `flowchart LR` (prefer left-to-right) |
| Sequence | Request flow across 2+ actors, async handoffs, retry/timeout behaviour | `sequenceDiagram` |
| Class | Domain model, aggregate boundaries, code-shape of key types | `classDiagram` |
| Entity-relationship | Data model, table relationships, cardinalities | `erDiagram` |

If none of the four fits, skip the diagram and write prose.

## Pastel palette

Pick one role per node. Keep roles consistent across all diagrams in the same doc so a reader learns the colour code once.

| Role | Fill | Stroke | Use for |
|---|---|---|---|
| Interface / external | `#BDE0FE` | `#5A8FBF` | Public APIs, external systems, third-party services |
| User / client | `#FFC8DD` | `#BF6985` | Browsers, mobile apps, end users, CLIs |
| Data / storage | `#B8E0D2` | `#5A8F7B` | Databases, caches, object stores, queues at rest |
| Compute / processing | `#FDE4A6` | `#BF9955` | Services, workers, functions, handlers |
| Async / events | `#CDB4DB` | `#7B5F87` | Message brokers, event streams, pub/sub topics |
| Infra / neutral | `#E2E2E2` | `#707070` | Load balancers, DNS, ingress, infra primitives |

Text colour is always `#1A1A1A` (near-black). Stroke width is 2px.

## Styling by diagram type

### Flowchart

```
flowchart LR
    classDef iface fill:#BDE0FE,stroke:#5A8FBF,stroke-width:2px,color:#1A1A1A
    classDef user fill:#FFC8DD,stroke:#BF6985,stroke-width:2px,color:#1A1A1A
    classDef data fill:#B8E0D2,stroke:#5A8F7B,stroke-width:2px,color:#1A1A1A
    classDef compute fill:#FDE4A6,stroke:#BF9955,stroke-width:2px,color:#1A1A1A
    classDef async fill:#CDB4DB,stroke:#7B5F87,stroke-width:2px,color:#1A1A1A
    classDef infra fill:#E2E2E2,stroke:#707070,stroke-width:2px,color:#1A1A1A

    U[Browser]:::user --> LB[Load balancer]:::infra
    LB --> API[Orders API]:::compute
    API --> DB[(Orders DB)]:::data
    API --> Q[[Events]]:::async
    Q --> W[Fulfilment worker]:::compute
```

Group related nodes in a `subgraph` when the diagram has 3+ logical clusters. Give each subgraph a short title. Use `style <subgraphId> fill:<fill>,stroke:<stroke>,stroke-width:1px,stroke-dasharray:4 4` with the same palette at lighter weight, for example fill `#F0F7FF` for an "API tier" group.

Keep edge labels short: the verb, not a sentence. "writes", "reads", "publishes", "invokes".

### Sequence

```
sequenceDiagram
    participant U as Browser
    participant API as Orders API
    participant DB as Orders DB
    participant Q as Events

    U->>API: POST /orders
    API->>DB: INSERT order
    DB-->>API: row id
    API->>Q: publish OrderCreated
    API-->>U: 201 Created
```

Use `participant` not `actor` (participants import to Excalidraw more cleanly). Solid arrows for sync calls, dashed arrows for responses or async. Add `Note over X: text` only when the note names a constraint (timeout, retry policy, idempotency key).

Participant styling through `%%{init}%%` front-matter is supported but not required. If you add colour, apply it via theme variables at the top of the block.

### Class

```
classDiagram
    class Order {
        +UUID id
        +Money total
        +OrderStatus status
        +place() void
        +cancel(reason) void
    }
    class LineItem {
        +UUID id
        +ProductId product
        +int quantity
    }
    Order "1" --> "many" LineItem : contains
```

Show only the attributes and methods that matter to the design conversation. Dropping private getters is fine. Mark aggregate roots in prose above the diagram, not inside it.

### Entity-relationship

```
erDiagram
    ORDER ||--o{ LINE_ITEM : contains
    ORDER }|--|| CUSTOMER : placed_by
    LINE_ITEM }o--|| PRODUCT : refers_to

    ORDER {
        uuid id PK
        uuid customer_id FK
        timestamp placed_at
        string status
    }
```

Include PK/FK annotations. Include only the fields that matter to the design. A full schema dump belongs in a separate reference, not here.

## Excalidraw import tips

All four diagram types above import via Excalidraw's "Mermaid to Excalidraw" feature. Things that help:

- Keep node labels short. Long labels wrap awkwardly after import.
- Stick to the palette above. Custom hex values import, but the palette is picked to stay readable on Excalidraw's default white canvas.
- Avoid Unicode arrows or symbols in labels.
- Avoid very deep subgraph nesting (3+ levels). Import collapses layout.
- For flowcharts with more than ~15 nodes, split into two diagrams by concern rather than one large diagram.

## Placement in the doc

- One diagram per section maximum. Two related diagrams in one section is usually one too many.
- One-line framing above the diagram, naming what it shows. No narration below.
- Fence the diagram with `` ```mermaid `` and nothing else in the block.

## Diagram checks

Before finishing the doc, verify:

- Diagram type is one of the four allowed.
- Every node uses a palette role (no default grey-on-white boxes).
- The diagram adds information the prose does not already state.
- Labels are short, imperative, no emojis, no Unicode symbols.
- The mermaid block parses (no stray punctuation, no unclosed subgraphs).
