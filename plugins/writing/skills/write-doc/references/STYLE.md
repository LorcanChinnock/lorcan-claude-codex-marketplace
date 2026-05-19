# STYLE.md: markdown structure and plain language

Reference for the write-doc skill. Apply on every draft.

## Plain language

- Prefer the shorter, more common word. "use" over "leverage", "show" over "demonstrate", "help" over "facilitate".
- Define a term the first time you use it, in one short clause. Do not assume the reader knows acronyms specific to your team.
- Translate to the audience the user named. A runbook for on-call has different jargon allowance from a getting-started for a new joiner. The audience-translation pass in `SKILL.md` is mandatory, not a polish step.
- Lead each section with the point, then the detail. The reader should be able to skim H2s and get the gist.
- Cut adjectives and adverbs that do not change meaning. "very", "quite", "really", "extremely" almost never earn their place.

## Voice

- Active voice unless passive genuinely fits ("the request is rejected by the firewall" is fine when the actor is the firewall).
- Address the reader as "you" when giving instructions. Use "we" sparingly, only when describing a team decision the reader is part of.
- Keep tense consistent within a section. Past for what happened, present for how it works now, future for plans.

## Markdown structure

- One H1 at the top, the doc title, sentence case.
- H2 for every major section. Pick the smallest set that covers the template.
- H3 only when an H2 has real subsections. Never use a lone H3.
- Do not stack a heading immediately on top of another heading with no prose between.
- Do not write a one-line restatement of the heading right after the heading. Start with content.

## Lists

- Use a bulleted list when order does not matter, a numbered list when it does (steps, ranked options).
- Three or more items earn a list. Two items usually read better as a sentence with "and".
- Keep bullets parallel: same grammatical shape, similar length.
- No bolded-header bullets (`- **Label:** text`). If the bullet needs a label, restructure as a small table or short subsection.

## Tables

- Use a table when readers will look up rows by a key (config option, error code, role). Do not use a table just to align two short lists.
- Header row in sentence case. Rows lowercase unless they are proper nouns or code.
- Keep cells short. If a cell needs a paragraph, move it to body text and link.

## Code blocks

- Always fence with the language: ```bash, ```python, ```sql, ```yaml, etc. Plain ``` only for output samples or unstructured text.
- Show the smallest example that proves the point. Comments only where they explain something the code cannot.
- Inline code for short literals: filenames, flags, env vars, function names.

## Links

- Link the first mention of an internal page or repo. Do not relink in every paragraph.
- Use descriptive link text, the words you would say out loud, not "click here" or a bare URL in the middle of prose.

## Callouts and admonitions

- Use callouts (`> **Note**`, `> [!WARNING]`, `> [!TIP]`) sparingly, only for a genuine aside the reader should not skip.
- Prefer body text for everything else. A callout that contains the main point of the section is misplaced.
- Match the rendering convention of the destination. GitHub-flavoured `[!NOTE]` blocks render in GitHub but not in plain markdown viewers.

## Diagrams

- Include a mermaid diagram only when it shortens the explanation. A flowchart that restates a four-step list adds noise.
- Place the diagram immediately after the prose it illustrates, not at the top of the section as decoration.
- Caption the diagram in one short sentence underneath if the role is not obvious from context.

## Images and screenshots

- Only when text genuinely cannot carry the information (UI placement, dashboard layout, real graph shape).
- Describe the relevant part in alt text, not the whole image.

## Tone

- Direct, not chatty. No "let's dive in", no "without further ado".
- No sycophancy ("great question"), no upbeat closers ("exciting times ahead").
- No knowledge-cutoff disclaimers. If a fact is unknown, mark it `TBD` and move on.

## Self-check

Before declaring the doc done, walk through these:

- Could a reader from the stated audience read this once and act on it?
- Does every H2 earn its place?
- Is there a paragraph or bullet that only restates what came before?
- Did jargon sneak in without a definition?
- Is every claim traceable to the user, the workspace, or a `TBD`?
