# Coralogix MCP read-only tool selection

Map question shape → tool → key parameters.

## Logs — `get_logs`

Use for: error patterns, audit trails, exception messages, "what did <service> do at <time>".

Key parameters:
- DataPrime filter (see [dataprime-cheatsheet.md](dataprime-cheatsheet.md))
- Time window (start/end, ISO-8601)
- Result limit — start at 50–100; raise only if needed.

Tip: filter on `$m.severity >= error` first. Most "what's wrong" questions only need errors and warnings.

## Traces — `get_traces`

Use for: "where in the call chain did this break", request-scoped latency analysis, distributed-system debugging.

Key parameters:
- Trace id, transaction id, or service name
- Time window

Tip: get a trace id from a log line first (`$d.trace_id`), then pivot. Searching traces blindly is rarely productive.

## Metrics

### `metrics__list`
Use for: discovering what metrics exist for a service. Run once per session.

### `metrics__metric_details`
Use for: understanding a metric's labels, type (counter/gauge/histogram), and unit before querying.

### `metrics__range_query`
Use for: "how did <metric> change over <window>", before/during/after comparisons, p95/p99 latency.

Key parameters:
- PromQL expression (e.g. `histogram_quantile(0.99, rate(http_request_duration_bucket[5m]))`)
- Time window and step

Tip: query the same metric for the window before the symptom, the window during, and the window after — the comparison is what makes the answer.

## Schemas — `get_schemas`

Use for: discovering log fields available for an application before writing a DataPrime filter. Cheap; run liberally at the start of a session.

## Documentation

- `read_dataprime_intro_docs` — DataPrime syntax reference. Call when uncertain about an operator.
- `read_metrics_guidelines` — naming conventions, label cardinality rules.
- `read_rum_log_intro_docs` — front-end (Real User Monitoring) log shape.
- `read_rum_sdk_docs` — RUM SDK details; only if the question is about instrumentation.

## Datetime — `get_datetime`

Use for: resolving "now" / "yesterday" / relative windows when the local interpretation is suspect, or aligning windows across timezones.

## Out of scope for the query skill

- `manage_alerts`, `manage_parsing_rules` — write tools, never used here.
- `list_incidents`, `get_incident_details`, `get_alert_event_details` — incident-management surface; defer to `incident-rca` for active incidents.
