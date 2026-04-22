# TEMPLATE.md — feature design document

Fixed shape for a feature-design doc. Read this on step 4 before drafting. Fill each section, keep the headings exactly as written, and mark anything unknown as `TBD` inline.

## Header

Top of the file, before the first `##`:

```markdown
# <Feature name>

**Status:** <Draft | Proposed | Accepted | Superseded>
**Audience:** <Engineers on this team | Engineers on other teams | Product and engineering | Stakeholders or executives>
**Last updated:** <YYYY-MM-DD (today's date in the user's locale if known, else UTC)>
**Owner:** <TBD unless the user provided one>
```

No tagline, no table of contents. Jump straight into the first section.

## Required sections in order

Keep the headings verbatim. Do not reorder. If a section has nothing to say, keep the heading and write one sentence explaining why it is empty (e.g. "No user-facing behaviour, this is an internal-only worker").

### 1. Context and motivation

2–5 short sentences. Lead with the problem and why now. Name the trigger (incident, metric, product ask, deadline, compliance requirement) if there is one. No history of the codebase unless it changes a decision.

### 2. Goals and non-goals

Two sub-sections, bulleted:

```markdown
### Goals
- <outcome the design must deliver, written as a testable statement>
- ...

### Non-goals
- <what this design explicitly does not cover, to stop scope creep>
- ...
```

Aim for 2–5 goals and 2–4 non-goals. If non-goals is empty the reader will assume the doc covers everything, which is usually wrong.

### 3. User-facing behaviour

What a user (human, calling service, or operator depending on Audience) observes. Flows, key scenarios, error states they care about. Prefer a short numbered list of scenarios. If the feature has no user-facing surface, say so and point at who the feature serves instead.

### 4. Architecture overview

One-line framing sentence. Then one `flowchart LR` Mermaid diagram showing the services, data stores, and external systems involved, styled per MERMAID.md. Use subgraphs to group by tier or domain if the diagram has 3+ clusters.

Follow the diagram with 2–6 sentences (or short bullets) explaining the decisions the diagram cannot convey: sync vs async boundaries, consistency guarantees, ownership of each component, which parts are new vs existing.

If the design touches only one service and the architecture is "this one service", skip the diagram and describe in prose.

### 5. Key sequences

Pick the 1–3 flows that drive the design. For each:

- A short heading (e.g. `#### Placing an order`)
- A one-line framing sentence
- One `sequenceDiagram` Mermaid block styled per MERMAID.md
- 1–3 sentences on retry, timeout, idempotency, or failure handling if relevant

Skip this section entirely (keep the heading with an explanatory sentence) only if the feature has no non-trivial cross-actor flow.

### 6. Data model

Only include a diagram if new tables, new relationships, or non-obvious cardinalities are introduced. When included:

- One-line framing sentence
- One `erDiagram` Mermaid block styled per MERMAID.md, showing only tables relevant to the design, with PK/FK annotations

Follow with prose covering: ownership of each table (which service writes it), retention, any denormalisation or read-model duplication, migration concerns.

If no schema change, keep the heading and write "No schema changes. Reads from <existing tables>."

### 7. API and interface surface

The contracts this feature adds or changes. Group by direction:

```markdown
#### Incoming
- `POST /orders` — <one-line purpose, request shape, response shape>
- <event topic subscribed> — <payload shape>

#### Outgoing
- <event topic published> — <payload shape, producer invariant>
- <downstream service call> — <endpoint, purpose>
```

Keep payload shapes to field-name + type. Full schemas belong in a reference, not here. If the feature adds no new surface, say so.

### 8. Observability and rollout

Two short paragraphs or bulleted:

- **Observability**: the metrics, logs, traces, and alerts a reader needs to tell whether the feature works in production. Name the metric and the threshold where known, `TBD` where not.
- **Rollout**: feature flag name and default, migration order, any backfill, any dark-launch or shadow-read phase. If none, say "Direct deploy, no flag, no migration."

### 9. Risks and open questions

Bulleted. Two flavours:

- **Risks**: things that could break in production and the mitigation. One bullet per risk. Lead with the failure mode, not the mitigation.
- **Open questions**: decisions still to make. One bullet per question. Include who needs to answer it.

If you have zero risks and zero open questions, the doc is probably under-thought. Ask the user before claiming "none".

## Class diagram (optional, sparingly)

Include a `classDiagram` only if the design introduces a non-trivial domain model that multiple readers will need to navigate in code. Place it inside §4 Architecture or §6 Data model, whichever fits. Skip by default.

## Template checks

Before presenting, verify:

- Header has Status, Audience, Last updated, Owner.
- All nine required sections are present in order with exact headings.
- Each Mermaid block uses an allowed type (flowchart, sequenceDiagram, classDiagram, erDiagram) and the palette from MERMAID.md.
- Every `TBD` is intentional, not a forgotten placeholder.
- No section is silently missing. Empty sections carry a one-sentence explanation.
- The file ends without a trailing "Summary" or "Conclusion" section. This template has no such section, do not add one.
