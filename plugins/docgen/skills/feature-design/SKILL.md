---
name: feature-design
description: Use when the user asks for a feature design document, architectural design doc, or runs /docgen:feature-design. Drives a structured questionnaire (status, audience, stage, scope, plus open questions on problem, systems, constraints, path), drafts to a fixed template with Mermaid diagrams where they help, and humanises the prose via humanize-text.
argument-hint: "[optional: feature name or short context]"
allowed-tools:
  - Read
  - Write
  - AskUserQuestion
  - Bash
  - Skill
---

Follow the provider-neutral core instructions in [../../shared/skills/feature-design.md](../../shared/skills/feature-design.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
