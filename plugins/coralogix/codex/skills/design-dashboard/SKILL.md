---
name: "design-dashboard"
description: "This skill should be used when the user asks to \"design a coralogix dashboard\", \"build a dashboard for <service>\", or \"what panels should I put on <X>\". Produces a Markdown dashboard spec (layout, variables, widgets with underlying queries) that the user builds in the Coralogix UI \u2014 not JSON, since the MCP has no dashboard-creation tool. Read-only."
---
Follow the provider-neutral core instructions in [../../../shared/skills/design-dashboard.md](../../../shared/skills/design-dashboard.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
