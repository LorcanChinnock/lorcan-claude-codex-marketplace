# TEMPLATE.md — title and body format

Fixed artefact shape for describe-pr output. Read this on step 4 before writing.

## Title: conventional commits

Format: `<type>(<optional scope>): <subject>`

- `type` is one of: `feat` (new user-visible capability), `fix` (bug fix), `perf` (measurable perf change, no behaviour change), `refactor` (internal restructure, no behaviour change, no perf claim), `docs`, `test`, `build`, `ci`, `chore`, `revert`.
- `scope` is optional. The affected package, module, or subsystem. Lowercase, short.
- `subject` is imperative mood, lowercase first letter, no trailing period.
- Total length including prefix ≤ 72 chars.
- Append `!` after the type/scope for a breaking change: `feat(api)!: remove v1 auth header`.

Examples:

- `feat(billing): add usage-based pricing tier`
- `fix(auth): prevent refresh-token replay on logout`
- `perf(ingest): batch writes to reduce p95 from 340ms to 90ms`

## Body: required sections in this order

Keep the headings exactly as written. Replace each placeholder with real content, or keep the heading with `None.` if truly empty.

```markdown
<1–3 sentences leading with motivation, then mechanism. Do not write "this PR" or "this change".>

## Release note
<ticket ID + one-line description of the change. Formats, in order of preference:
- `[TICKET-ID](url): <one-line description of the change>` — when a ticket URL is known (from the branch name + a configured issue tracker, or from the open PR body).
- `TICKET-ID: <one-line description of the change>` — when the branch yields a ticket ID but no URL is available.
- `<one-line description of the change>` — only when the branch has no recognisable ticket (e.g. no `ABC-123`-shaped token) and the user did not supply one.
Description is imperative, sentence case, ends with a period. Do not write "Implemented new feature." as a placeholder — if the user said "just write it" and no ticket is known, write the real one-line description from the diff.>

## Summary
<What another developer needs to know to review or maintain this. Lead with why. 2–6 short sentences or a short bulleted list of decisions and tradeoffs. Skip what the diff already shows.>

## Testing steps
<Concrete steps a reviewer can run, or a description of automated test coverage. If the change is not testable, say why.>

## Feature flag
<Flag name + default, or "No.">

## Follow-up issues
<Bulleted list of deferred cleanup or issue numbers, or "None.">
```

Optional sections may go below Follow-up issues when they earn their place: `## Rollout`, `## Migration`, `## Screenshots`, `## Benchmarks`. Nothing goes above the required ones.

## Mermaid diagrams

Include one only when the change is non-trivial in one of these shapes:

- Architecture or service topology → `flowchart LR`
- Request or data flow → `sequenceDiagram`
- State machine additions or transitions → `stateDiagram-v2`
- Schema or data-model changes → `erDiagram` or `classDiagram`

Skip diagrams for small, local, or purely internal refactors. A diagram that restates one function's behaviour is clutter. If you include one, put it inside `## Summary` or a dedicated optional section, and write one sentence above it naming what it shows.

## Template checks

Before printing, verify:

- Title is ≤ 72 chars, conventional-commits shaped, imperative mood, lowercase first letter of subject, no trailing period.
- All five required sections are present in order with exact headings.
- Optional sections (if any) sit below Follow-up issues, never above.
- Mermaid block (if present) parses as valid Mermaid and adds information beyond the prose.
- If you claimed a metric, it is a real number from the conversation, not a placeholder.
- Release note matches one of the three formats above: ticket-with-link, ticket-only, or plain description. The default "Implemented new feature." placeholder never appears in the output.
