---
name: design-alert
description: This skill should be used when the user asks to "design a coralogix alert", "draft an alert when <condition>", "alert on <error pattern>", or "set up a threshold alert on <metric>". Walks through alert type, condition, threshold, severity, notifications via targeted questions and produces a reviewable JSON payload. Read-only by default — `manage_alerts` is only called after the user explicitly confirms the draft.
allowed-tools:
  - mcp__coralogix__alert_definition_creation_docs
  - mcp__coralogix__get_schemas
  - mcp__coralogix__get_logs
  - mcp__coralogix__metrics__list
  - mcp__coralogix__metrics__metric_details
  - mcp__coralogix__metrics__range_query
  - mcp__coralogix__read_dataprime_intro_docs
  - mcp__coralogix__read_metrics_guidelines
  - mcp__coralogix__manage_alerts
  - Read
  - Write
---

# design-alert

Design a Coralogix alert interactively, produce a reviewable JSON payload, and only create/update it via `manage_alerts` after the user explicitly confirms. Works for log-based, metric-based, and ratio/flow alerts.

## Safety contract

This skill is read-by-default. Specifically:

- `manage_alerts` is NEVER called in the same turn as the initial request. The skill first produces a draft JSON and asks for explicit confirmation.
- If the user asks to "create and apply immediately", still produce the draft first, state the payload, and require one confirming response (e.g. "yes, create it") before calling `manage_alerts`.
- Revisions are OK without re-confirmation if the user steers mid-draft — confirmation gate applies only at the moment of the write.

## Process

### 1. Load the authoritative schema

Call `alert_definition_creation_docs` at the start of every alert-design session. The alert-definition schema changes over time; do not rely on cached knowledge. Note the fields required for the alert type the user wants.

### 2. Clarify the alert

Determine the alert type and the five required dimensions. Ask at most one round of consolidated questions — do not interrogate. If the user's initial prompt already answers some, skip those.

**Five dimensions:**

1. **Type** — log threshold, metric threshold, ratio (A/B), unique-count, new-value, flow, or time-relative. If unsure, see [references/alert-types.md](references/alert-types.md).
2. **Condition** — the DataPrime filter (log) or PromQL expression (metric) that defines the signal. Must be valid — validate via step 3.
3. **Threshold & window** — trigger condition (e.g. `>100 events in 5 min`, `p99 > 1s for 10 min`).
4. **Severity** — info / warning / error / critical. Maps to notification routing downstream.
5. **Notification target & runbook** — webhook/Slack/email destination (names, not full config) and a runbook URL if one exists.

If the user cannot name a runbook, ask whether to add a placeholder TODO or proceed without. Do not fabricate one.

### 3. Validate the signal before drafting

Run the proposed filter/metric against live data before shaping it into an alert:

- Log alerts: `get_logs` with the filter and a recent window to confirm it matches what the user expects — no zero-result filters, no filters matching thousands of unrelated lines.
- Metric alerts: `metrics__range_query` over the last 24h to confirm the metric exists, has the expected shape, and the proposed threshold is not obviously always-on or never-firing.
- If `get_schemas` / `metrics__list` have not been run this session, run them first.

Report the validation result in one line ("Filter matched 37 events in the last 6h; looks sane") before drafting.

### 4. Draft the alert JSON

Produce the alert definition per the schema returned by `alert_definition_creation_docs`. Keep the draft in a single fenced JSON block so the user can read and copy it.

Always include:
- A human-readable name (format: `<severity>: <service> — <what>`, e.g. `warning: checkout — elevated 5xx`).
- A description field explaining when this fires and what to do (1–3 sentences, includes runbook URL or `TODO: add runbook`).
- Labels/tags: team, service, severity at minimum.

See [references/alert-checklist.md](references/alert-checklist.md) before presenting the draft — catches the common mistakes (too-tight threshold, too-loose filter, missing dedup window).

### 5. Present & confirm

Output in this shape:

```markdown
## Alert draft: <name>

**Summary**: 1–2 sentences on what this alerts on and why.

**Checklist**:
- [x] Filter validated against recent data (matched N events)
- [x] Threshold is not always-on or never-firing
- [x] Runbook link present (or explicitly TODO)
- [x] Dedup/group-by configured
- [ ] <anything open>

**Payload**:
~~~json
<full payload>
~~~

**Next**: Reply `create` to call manage_alerts, `revise <change>` to iterate, or `stop` to keep the draft only.
```

### 6. Write (only on explicit confirmation)

When the user replies `create` or an unambiguous equivalent ("yes, create it", "apply it"), call `manage_alerts` with the drafted payload. Report back:

- The alert id returned by the API.
- A one-line confirmation of what was created.
- The Coralogix UI URL if it can be derived.

If `manage_alerts` returns an error, surface the raw error verbatim — do not retry silently. Offer to revise.

### 7. Updating existing alerts

If the user asks to "change the threshold on the checkout 5xx alert" or similar:

1. Ask the user for the alert id (or name). If `manage_alerts` supports read/list operations, use it to fetch the current definition; otherwise ask the user to paste the current payload from the Coralogix UI. `get_alert_event_details` returns details of a *firing event*, not the definition — do not use it for this step.
2. Show the diff (before/after) as a small Markdown table.
3. Apply the same confirmation gate before calling `manage_alerts`.

## Additional resources

- [references/alert-types.md](references/alert-types.md) — when to use log threshold vs metric vs ratio vs flow, with concrete examples.
- [references/alert-checklist.md](references/alert-checklist.md) — pre-commit checks: too-sensitive, too-insensitive, missing dedup, orphan runbook.
