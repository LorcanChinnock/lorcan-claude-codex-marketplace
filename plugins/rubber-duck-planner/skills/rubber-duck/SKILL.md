---
name: rubber-duck
description: Use when stress-testing a technical idea or implementation plan before committing to it. Acts as a critical sparring partner through four phases — clarify, challenge, alternatives, converge — producing a consolidated plan summary on exit. Pushes back with reasoning. Not a reviewer, not a yes-man.
argument-hint: [idea or plan description; omit to use the current conversation]
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - AskUserQuestion
---

Follow the provider-neutral core instructions in [../../shared/skills/rubber-duck.md](../../shared/skills/rubber-duck.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
