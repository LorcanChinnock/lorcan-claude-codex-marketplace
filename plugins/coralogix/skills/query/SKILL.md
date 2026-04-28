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

Follow the provider-neutral core instructions in [../../shared/skills/query.md](../../shared/skills/query.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
