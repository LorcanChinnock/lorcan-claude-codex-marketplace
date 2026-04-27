---
name: query
description: This skill should be used when the user asks to "query coralogix", "search coralogix logs", "check metrics in coralogix", "get traces from coralogix", or "investigate <symptom> in coralogix" — ad-hoc data exploration, not active incident triage. Produces a structured report grounded by schema discovery. Read-only.
allowed-tools:
  - mcp__coralogix__get_logs
  - mcp__coralogix__get_traces
  - mcp__coralogix__get_schemas
  - mcp__coralogix__get_datetime
  - mcp__coralogix__metrics__list
  - mcp__coralogix__metrics__metric_details
  - mcp__coralogix__metrics__range_query
  - mcp__coralogix__read_dataprime_intro_docs
  - mcp__coralogix__read_metrics_guidelines
  - mcp__coralogix__read_rum_log_intro_docs
  - mcp__coralogix__read_rum_sdk_docs
  - Read
---

# query

Run ad-hoc Coralogix queries against logs, traces, and metrics, then return a structured report. The skill assumes the Coralogix MCP server is configured and authenticated. For active production incidents, defer to the `incident-rca` skill if available; this skill is for exploratory and routine querying.

## Inputs

Invocation comes either as a direct slash command (`/coralogix:query <prompt>`) or a conversational question that names Coralogix. The prompt typically contains some subset of:

- A target — service / application / subsystem name, or a specific user/trace/transaction id.
- A time window — absolute (`2026-04-23 14:00..15:00`), relative (`last hour`, `since yesterday`), or implicit.
- A symptom or signal — error message, latency complaint, traffic anomaly, "anything weird".

If the time window is not stated, default to **last 1 hour** and state the assumption in the first user-facing update.

## Process

### 1. Ground the query (always)

Before the first query, call schema discovery so DataPrime queries do not silently fail on misnamed fields. Run these in parallel:

- `get_schemas` — for the named application/subsystem; lists available log fields and types.
- `metrics__list` — only if the prompt mentions metrics, latency, throughput, or error rate.
- `get_datetime` — only if the user used a relative window and the local clock is suspect.

If `get_schemas` returns nothing for the named application, ask the user to confirm the application name (typo, retired service, or different team's tenant). Do not fall back to guessing.

### 2. Choose the source

Map the user's question to the right Coralogix source. When in doubt, query logs first — they are cheapest and most expressive.

| Question shape | Primary source | Tool |
|---|---|---|
| "Why did X fail?", "errors in Y", "what does the log say?" | Logs | `get_logs` |
| "How slow is Z?", "p99 latency", "throughput on W" | Metrics | `metrics__range_query` |
| "Where in the call chain did this break?", "trace this request" | Traces | `get_traces` |
| "Frontend errors", "RUM", "user-facing crashes" | RUM logs | `get_logs` (RUM application) — read `read_rum_log_intro_docs` first |

Multi-source questions are normal: `get_logs` for the error pattern, then `metrics__range_query` to confirm an error-rate spike, then `get_traces` on a representative trace id surfaced from the logs.

### 3. Build and run the query

For DataPrime syntax, consult `read_dataprime_intro_docs` if uncertain — do not fabricate operators. For metrics, consult `read_metrics_guidelines` for naming/labels.

Common log filter patterns are listed in [references/dataprime-cheatsheet.md](references/dataprime-cheatsheet.md). For tool-selection guidance and parameter shapes, see [references/mcp-tools.md](references/mcp-tools.md).

Run queries in parallel when independent (e.g. logs in two services, or logs + a metric range query for the same window).

### 4. Iterate on signal

If the first query returns nothing useful, change exactly one variable per iteration:

- Widen the time window (1h → 6h → 24h).
- Drop the most specific filter (severity, then keyword, then service).
- Switch source (logs → traces, or logs → metrics).

Stop iterating after three rounds and report what was searched. Do not flood the user with raw log dumps; summarise.

### 5. Report

Output a structured Markdown report. Keep it terse — the user can ask for more.

```markdown
## Coralogix query report

**What was queried**
- Target: <application/subsystem/all>
- Time window: <start> .. <end>  (assumed last 1h if not specified)
- Filters: <severity, keywords, tx ids>
- Sources: logs / traces / metrics (whichever ran)

**Log findings**
- <timestamp> <severity> <one-line excerpt>
- Pattern: <what the logs collectively show>
- (or: "No matching logs.")

**Trace findings**
- <trace id>: <span breakdown — where latency/errors land>
- (or: "Not queried" / "No matching traces.")

**Metric findings**
- <metric>: <trend — e.g. p99 went 200ms → 2.1s at 14:32>
- (or: "Not queried" / "No notable change.")

**Synthesis**
- <2–4 sentences answering the user's actual question.>

**Refinement suggestions** (only if results were thin)
- <wider window / different app / drop severity filter>
```

If the user asked a single direct question with a single direct answer ("what was the last error in checkout?"), collapse the report — answer first, then a one-line "Queried: <app>, <window>, <filters>" footer for traceability.

## Read-only constraint

This skill is read-only. Do not call:

- `manage_alerts` — defer to the `design-alert` skill.
- `manage_parsing_rules` — out of scope for this plugin.

If the user asks during a query session to "alert on this" or "build a dashboard for this", suggest invoking `/coralogix:design-alert` or `/coralogix:design-dashboard` and stop — do not silently switch modes.

## Additional resources

- [references/dataprime-cheatsheet.md](references/dataprime-cheatsheet.md) — common DataPrime filter idioms; when in doubt, also call `read_dataprime_intro_docs`.
- [references/mcp-tools.md](references/mcp-tools.md) — which read-only MCP tool to pick for which question, with parameter shapes.
