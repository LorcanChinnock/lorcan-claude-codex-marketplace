---
name: handle-review
description: Use when receiving code review or other critical feedback (PR comments, design review, written critique). Enforces verify-before-implement, reasoned push-back, one-item-at-a-time execution, and no performative agreement.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Edit
  - Write
  - AskUserQuestion
  - LSP
---

Follow the provider-neutral core instructions in [../../shared/skills/handle-review.md](../../shared/skills/handle-review.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
