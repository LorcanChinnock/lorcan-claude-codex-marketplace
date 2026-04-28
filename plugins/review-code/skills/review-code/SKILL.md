---
name: review-code
description: Use when reviewing the current branch's diff vs its base, reviewing a GitHub PR by number or URL, or auditing proposed changes before merge. Produces a local Markdown report with confidence-ranked findings and permalinks. Never posts to GitHub, never mutates files.
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - Agent
  - Write
  - LSP
---

Follow the provider-neutral core instructions in [../../shared/skills/review-code.md](../../shared/skills/review-code.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
