---
name: "query"
description: "This skill should be used when the user asks to \"query coralogix\", \"search coralogix logs\", \"check metrics in coralogix\", \"get traces from coralogix\", or \"investigate <symptom> in coralogix\" \u2014 ad-hoc data exploration, not active incident triage. Produces a structured report grounded by schema discovery. Read-only."
---
Follow the provider-neutral core instructions in [../../../shared/skills/query.md](../../../shared/skills/query.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
