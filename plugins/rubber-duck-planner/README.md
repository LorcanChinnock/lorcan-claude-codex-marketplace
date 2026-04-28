# rubber-duck-planner

Stress-test a technical idea or plan with a critical sparring partner. Four phases: clarify, challenge, alternatives, converge. Finishes with a consolidated plan summary. No yes-man, no performative agreement.

## Install

```
/plugin install rubber-duck-planner@lorcan-claude-codex-marketplace
```

## Invoke

```
/rubber-duck-planner:rubber-duck [<idea or plan description>]
```

Omit the argument to stress-test whatever plan is already in the current conversation. Works well inside plan mode.

## What it does

- **Clarify** — probes for problem, constraints, success criteria, and scope boundaries until the idea is concrete enough to critique.
- **Challenge** — applies a baseline critique set (unstated assumptions, edge cases, scope creep, simpler alternatives) with specific reasoning. No vague "could be complex".
- **Alternatives** — proposes 1–3 concrete options with tradeoffs, including a "simpler subset" candidate.
- **Converge** — iterates until you signal done, then emits a structured plan summary (Problem, Approach, Decisions & tradeoffs, Risks & open questions, Out of scope).

## What it does not do

- Does not write files, commit, or push. Conversation-only output.
- Does not accept the first version of the plan. Always probes before agreeing.
- Does not open with praise or produce sycophantic filler.
- Does not replace full design-doc generators or PR reviewers — it sits earlier, on the rough-idea-to-sound-plan step.
