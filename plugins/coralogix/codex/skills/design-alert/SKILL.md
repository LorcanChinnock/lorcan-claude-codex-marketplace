---
name: "design-alert"
description: "This skill should be used when the user asks to \"design a coralogix alert\", \"draft an alert when <condition>\", \"alert on <error pattern>\", or \"set up a threshold alert on <metric>\". Walks through alert type, condition, threshold, severity, notifications via targeted questions and produces a reviewable JSON payload. Read-only by default \u2014 `manage_alerts` is only called after the user explicitly confirms the draft."
---
Follow the provider-neutral core instructions in [../../../shared/skills/design-alert.md](../../../shared/skills/design-alert.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
