---
name: "feature-design"
description: "Use when the user asks for a feature design document, architectural design doc, or runs /docgen:feature-design. Drives a structured questionnaire (status, audience, stage, scope, plus open questions on problem, systems, constraints, path), drafts to a fixed template with Mermaid diagrams where they help, and humanises the prose via humanize-text."
argument-hint: "\"[optional: feature name or short context]\""
---
Follow the provider-neutral core instructions in [../../../shared/skills/feature-design.md](../../../shared/skills/feature-design.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
