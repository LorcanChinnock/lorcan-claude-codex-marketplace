---
name: design-dashboard
description: This skill should be used when the user asks to "design a coralogix dashboard", "build a dashboard for <service>", or "what panels should I put on <X>". Produces a Markdown dashboard spec (layout, variables, widgets with underlying queries) that the user builds in the Coralogix UI — not JSON, since the MCP has no dashboard-creation tool. Read-only.
allowed-tools:
  - mcp__coralogix__get_schemas
  - mcp__coralogix__metrics__list
  - mcp__coralogix__metrics__metric_details
  - mcp__coralogix__metrics__range_query
  - mcp__coralogix__get_logs
  - mcp__coralogix__read_dataprime_intro_docs
  - mcp__coralogix__read_metrics_guidelines
  - mcp__coralogix__read_rum_log_intro_docs
  - Read
---

Follow the provider-neutral core instructions in [../../shared/skills/design-dashboard.md](../../shared/skills/design-dashboard.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
