---
name: "describe-pr"
description: "Use when the user asks for a PR title or description, or runs /describe-pr. Reads raw diff vs base, asks heavily about why the change was made (the diff already shows what and how), then writes a conventional-commits title plus a fixed-template body (Release note, Summary, Testing, Feature flag, Follow-ups) in dropped-subject active voice. Prose follows humaniser rules."
---
Follow the provider-neutral core instructions in [../../../shared/skills/describe-pr.md](../../../shared/skills/describe-pr.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
