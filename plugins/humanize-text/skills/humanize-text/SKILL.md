---
name: humanize-text
description: Use when the user asks to humanise, de-AI, soften, naturalise, or edit obviously LLM-sounding prose, or pastes AI output for review. Rewrites text to remove signs of AI-generated writing — inflated significance, promotional language, em-dash overuse, AI vocabulary, bolded-header bullets, sycophantic openers, title case in body text, and related tells.
allowed-tools:
  - Read
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---

Follow the provider-neutral core instructions in [../../shared/skills/humanize-text.md](../../shared/skills/humanize-text.md).

Claude mapping: use the listed `allowed-tools` as the concrete implementations of the core capabilities. `AskUserQuestion` is the question tool, `Agent` is the delegation/subagent capability, `Read`/`Edit`/`Write` are file capabilities, `Bash` is the shell capability, and `LSP` is language-aware navigation.
