---
name: "humanize-text"
description: "Use when the user asks to humanise, de-AI, soften, naturalise, or edit obviously LLM-sounding prose, or pastes AI output for review. Rewrites text to remove signs of AI-generated writing \u2014 inflated significance, promotional language, em-dash overuse, AI vocabulary, bolded-header bullets, sycophantic openers, title case in body text, and related tells."
---
Follow the provider-neutral core instructions in [../../../shared/skills/humanize-text.md](../../../shared/skills/humanize-text.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
