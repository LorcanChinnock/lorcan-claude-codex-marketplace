---
name: "review-code"
description: "Use when reviewing the current branch's diff vs its base, reviewing a GitHub PR by number or URL, or auditing proposed changes before merge. Produces a local Markdown report with confidence-ranked findings and permalinks. Never posts to GitHub, never mutates files."
---
Follow the provider-neutral core instructions in [../../../shared/skills/review-code.md](../../../shared/skills/review-code.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
