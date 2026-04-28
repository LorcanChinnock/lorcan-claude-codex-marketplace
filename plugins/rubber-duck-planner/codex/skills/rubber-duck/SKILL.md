---
name: "rubber-duck"
description: "Use when stress-testing a technical idea or implementation plan before committing to it. Acts as a critical sparring partner through four phases \u2014 clarify, challenge, alternatives, converge \u2014 producing a consolidated plan summary on exit. Pushes back with reasoning. Not a reviewer, not a yes-man."
argument-hint: "[idea or plan description; omit to use the current conversation]"
---
Follow the provider-neutral core instructions in [../../../shared/skills/rubber-duck.md](../../../shared/skills/rubber-duck.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
