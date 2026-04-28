# design-dashboard

Design a Coralogix dashboard and output a Markdown spec the user builds in the UI. The spec is the deliverable — the Coralogix MCP does not support creating dashboards programmatically, and dashboard JSON schemas drift frequently enough that best-effort JSON is not reliable.

## Process

### 1. Clarify the dashboard's job

Ask one consolidated round only. Cover:

- **Purpose** — service overview, incident triage, SLO tracking, capacity planning, customer-facing status, something else.
- **Audience** — on-call engineer in the first 60 seconds of a page, a team lead during a review, leadership quarterly.
- **Primary service(s)** — which applications/subsystems feed this dashboard.
- **Must-answer questions** — 3–5 concrete questions the dashboard must answer at a glance (e.g. "is checkout healthy", "where is latency landing", "are we rate-limited").

If the user gives only a service name with no purpose, default to **service overview** and state the assumption.

### 2. Discover available data

Do not design panels from imagination. Run in parallel:

- `get_schemas` for the target application(s) — which log fields exist.
- `metrics__list` — which metrics exist; note type (counter/gauge/histogram) via `metrics__metric_details` for anything non-obvious.

If the proposed dashboard needs a metric or field that does not exist, surface this in the spec as a gap ("Missing: no RED-methodology error-rate metric exists for this service; widget #4 would need it to be emitted first").

### 3. Pick a layout pattern

Match the purpose to a pattern. See [references/patterns.md](references/patterns.md) for full examples.

| Purpose | Pattern |
|---|---|
| Service overview | RED (Rate, Errors, Duration) + traffic + top errors |
| Incident triage | Signal-at-a-glance: 4 big stat panels, then drill-down rows |
| SLO tracking | Burn-rate over rolling windows + budget consumption |
| Capacity planning | USE (Utilisation, Saturation, Errors) per resource |
| RUM / front-end | Page load, core web vitals, JS error rate by route |

### 4. Draft the spec

Output the spec in the shape below. Keep panel descriptions one or two lines — the query is the source of truth.

```markdown
# Dashboard: <name>

**Purpose**: <one line>
**Audience**: <one line>
**Primary services**: <list>

## Variables (template)
- `$service` — default: <value> — applied to <widgets>
- `$env` — default: prod — applied to <widgets>
- `$window` — default: 1h — applied to <widgets>

## Layout

### Row 1 — At-a-glance (4 stat widgets, full width)
### Row 2 — RED metrics (3 time-series, full width)
### Row 3 — Top errors (table, half) + Error trend (time-series, half)
### Row 4 — Traces (table, full)

## Widgets

### 1. Error rate (stat)
- **Row/col**: 1 / 1
- **Visualisation**: stat, red at >1%, amber >0.1%
- **Query** (PromQL):
  ~~~
  sum(rate(http_requests_total{service=~"$service",status=~"5.."}[5m]))
  / sum(rate(http_requests_total{service=~"$service"}[5m]))
  ~~~
- **Notes**: companion to widget #5 (error trend).

### 2. p99 latency (stat)
- ...

<repeat per widget>

## Gaps / follow-ups
- <missing metric, logs that should be structured, parsing rule needed>

## How to build in Coralogix
1. Dashboards → New dashboard → name it <name>.
2. Add variables listed above (settings → variables).
3. Add rows and widgets in the order listed; paste the query from each widget.
4. Save; share the URL with the audience group.
```

### 5. Gap list

Every dashboard design surfaces gaps. List them explicitly rather than papering over with invented queries:

- Metrics that should exist but do not (call out what the service would need to emit).
- Log fields that need structured parsing (point the user at `manage_parsing_rules` — out of scope for this plugin).
- Widgets deliberately omitted because they would be misleading at this level.

### 6. No JSON

Do not produce Coralogix dashboard JSON unless the user explicitly asks for it and accepts the caveat that the schema is undocumented in the MCP. If they insist, produce a best-effort skeleton, label it as such, and recommend manual verification in the UI.

## Constraints

- read-only. Do not call `manage_alerts` or `manage_parsing_rules`. If the spec surfaces a need for either, note it in the Gaps section and suggest invoking the relevant workflow (`/coralogix:design-alert` for alerts).
- Do not design dashboards for services the user has not named. If the user gives a vague "all our services", ask them to pick one or a small group (≤5) before proceeding — a generic dashboard is a useless dashboard.

## Additional resources

- [references/patterns.md](references/patterns.md) — RED, USE, SLO-burn, incident-triage, and RUM dashboard patterns with example widget lists.
- [references/widget-types.md](references/widget-types.md) — when to use stat vs time-series vs table vs heatmap, plus visualisation pitfalls.
