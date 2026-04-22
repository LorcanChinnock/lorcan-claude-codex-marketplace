# critique-heuristics.md: extended failure modes to probe

Load on demand from SKILL.md when running the Challenge phase. Extends (does not replace) the baseline four defined in SKILL.md: unstated assumptions, edge cases & failure modes, scope creep, simpler alternatives.

## Correctness dimensions

- Empty input, zero items, null, missing field.
- Max input, large payload, high concurrency.
- Two callers hitting the path simultaneously — race? Lock contention?
- Partial failure — step 3 of 5 fails, what state is the system in?
- Retry safety — is the operation idempotent?
- Time / clock / timezone assumptions.

## Complexity smells

- New abstraction with a single caller — YAGNI?
- New config knob — who flips it, when, and why? If "just in case", drop it.
- New dependency — could stdlib or existing code do the job?
- Cross-cutting change touching many files — is the abstraction in the wrong place?
- Manual step buried in the plan — can it be automated, or is it a trap for future maintainers?

## Scope creep

- Any component solving a problem that was not stated?
- Could half the plan ship first and still be valuable?
- Is there a "while we're at it" item that belongs in a separate change?

## Reversibility

- If this ships and turns out wrong, how hard to undo?
- Schema / data migration — is there a rollback path, or is it one-way?
- Public API surface — are we committing before we are sure?

## Hidden costs

- Operational — who monitors it, who gets paged, where is the dashboard?
- Performance — added latency, memory, IO, cold-start cost?
- Testing — how would you prove it works in production, not just in unit tests?
- Security — new attack surface? New trust boundary? New secret to manage?

## Alignment with existing code

- Is there already a pattern for this in the codebase? Grep before inventing.
- Does this contradict a prior decision? Check git log / commit messages / ADRs.
- Does the naming match the local convention?

## Push-back templates

- `Assumption: you're assuming <X>. Have you verified it holds when <concrete case>?`
- `Edge case: what happens when <specific scenario>? The plan does not address it.`
- `Simpler: <alternative>. Tradeoff: <what you lose>. Why not this?`
- `Scope: <component> is orthogonal to the stated problem. Split it out?`
- `Reversibility: if <X> turns out wrong, rollback costs <Y>. Is that acceptable?`
- `Duplication: <existing thing, file:line> already does this. Extend it instead?`
- `Dependency: <new thing> adds <cost>. <existing thing> can do <fraction> of it. Worth the rest?`

