---
name: describe-pr
description: Use when the user asks for a PR title or description, or runs /describe-pr. Reads raw diff vs base, asks heavily about why the change was made (the diff already shows what and how), then writes a conventional-commits title plus a fixed-template body (Release note, Summary, Testing, Feature flag, Follow-ups) in dropped-subject active voice. Prose follows humaniser rules.
allowed-tools:
  - Bash
  - Read
  - Write
---

Follow the provider-neutral core instructions in [../../shared/skills/describe-pr.md](../../shared/skills/describe-pr.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
