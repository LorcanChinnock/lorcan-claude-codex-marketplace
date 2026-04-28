---
name: "handle-review"
description: "Use when receiving code review or other critical feedback (PR comments, design review, written critique). Enforces verify-before-implement, reasoned push-back, one-item-at-a-time execution, and no performative agreement."
---
Follow the provider-neutral core instructions in [../../../shared/skills/handle-review.md](../../../shared/skills/handle-review.md).

Codex mapping: use local file tools for file read/write capabilities, `exec_command` for shell capability, `request_user_input` where available for the question tool, `spawn_agent` for the delegation/subagent capability, and language-aware navigation tools when available. If a core asks for delegation but Codex cannot delegate in the current mode, run the stages sequentially and disclose that delegation was unavailable.
