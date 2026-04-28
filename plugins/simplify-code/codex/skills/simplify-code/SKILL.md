---
name: "simplify-code"
description: "Use when the user asks to simplify, tidy, clean up, deduplicate, or remove dead code from local files \u2014 branch diff vs base, staged changes, working tree, or specified paths. Produces a confidence-scored proposal report and applies the survivors (proposal only in plan mode)."
argument-hint: "\"[branch | staged | working | <path>...]\""
---
Follow the provider-neutral core instructions in [../../../shared/skills/simplify-code.md](../../../shared/skills/simplify-code.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
