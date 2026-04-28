# coralogix

Query Coralogix logs/traces/metrics, design alerts, and design dashboards via the Coralogix MCP. Read-only by default — alert mutations require explicit confirmation.

## Install

```
/plugin install coralogix@lorcan-claude-codex-marketplace
```

## Prerequisites

This plugin **does not bundle** a Coralogix MCP server — it assumes one is already configured and connected **under the name `coralogix`**. Skills reference tools as `mcp__coralogix__<tool>`; connecting the server under a different name (e.g. `cx`) will make every tool call fail silently. Required MCP tools:

- Read: `get_logs`, `get_traces`, `get_schemas`, `get_datetime`, `metrics__list`, `metrics__metric_details`, `metrics__range_query`, `read_dataprime_intro_docs`, `read_metrics_guidelines`, `read_rum_log_intro_docs`, `read_rum_sdk_docs`, `get_alert_event_details`, `alert_definition_creation_docs`.
- Write (only used by `design-alert` after explicit confirmation): `manage_alerts`.

If the plugin's MCP tool calls fail with "tool not available", verify the Coralogix MCP is connected (`/mcp`) and the user has the needed API permissions.

## Skills

### `/coralogix:query`

Run ad-hoc Coralogix queries. Auto-triggers on phrases like *"search coralogix logs"*, *"check metrics for <service>"*, *"get the latest errors from coralogix"*. Always grounds itself with `get_schemas` / `metrics__list` before querying to avoid silent empty results. Produces a structured report (what was queried → log/trace/metric findings → synthesis). Read-only.

### `/coralogix:design-alert`

Design a Coralogix alert interactively. Auto-triggers on *"design an alert"*, *"alert when <condition>"*, *"draft an alert for <service>"*. Walks through type, condition, threshold, severity, and notification; validates the filter against real data; produces a reviewable JSON payload. **`manage_alerts` is only called after explicit user confirmation** — e.g. replying `create` to the draft.

### `/coralogix:design-dashboard`

Design a Coralogix dashboard. Auto-triggers on *"design a dashboard"*, *"dashboard for <service>"*, *"what panels should I put on <X>"*. Produces a Markdown dashboard spec (layout, variables, widgets with underlying queries) the user builds in the Coralogix UI. Does **not** produce dashboard JSON — the MCP has no dashboard-creation tool and the JSON schema drifts. Read-only.

## What this plugin does not do

- No parsing-rule management (`manage_parsing_rules` is deliberately out of scope).
- No active-incident investigation — for that, use `incident-rca` if installed.
- No dashboard creation via API (MCP does not expose it).
- No bundled MCP server config — you provide your own.

## Relationship to `incident-rca`

`incident-rca` owns the incident-investigation flow and has its own Coralogix-specific subagent prompt. This plugin handles everyday Coralogix work: ad-hoc queries, alert drafting, dashboard planning. Skills here do not attempt to replace `incident-rca`; if a query turns into active incident triage, switch to that skill.
