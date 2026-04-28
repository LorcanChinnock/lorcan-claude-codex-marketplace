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

Follow the provider-neutral core instructions in [../../shared/skills/design-alert.md](../../shared/skills/design-alert.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
