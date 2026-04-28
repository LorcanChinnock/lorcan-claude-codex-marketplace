---
name: simplify-code
description: Use when the user asks to simplify, tidy, clean up, deduplicate, or remove dead code from local files — branch diff vs base, staged changes, working tree, or specified paths. Produces a confidence-scored proposal report and applies the survivors (proposal only in plan mode).
argument-hint: "[branch | staged | working | <path>...]"
allowed-tools:
  - Agent
---

Follow the provider-neutral core instructions in [../../shared/skills/simplify-code.md](../../shared/skills/simplify-code.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
