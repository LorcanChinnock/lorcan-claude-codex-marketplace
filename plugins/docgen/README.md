# docgen

Generate software engineering documentation in Markdown. Runs a short questionnaire, drafts to a fixed template, adds Mermaid diagrams where they help, and humanises the prose.

Currently supports **Feature Design** docs. More doc types land as siblings under `skills/`.

## Install

```
/plugin install docgen@lorcan-claude-codex-marketplace
```

## Invoke

```
/docgen:feature-design [optional feature name]
```

Or ask in conversation ("write a feature design doc for the checkout rewrite"). Both routes run the same skill.

## What it does

- Asks four closed questions (status, audience, stage, scope) via AskUserQuestion.
- Asks five open questions (name, problem, systems, constraints, output path).
- Drafts to a fixed nine-section template: Context, Goals and non-goals, User-facing behaviour, Architecture, Key sequences, Data model, API surface, Observability and rollout, Risks and open questions.
- Adds Mermaid diagrams where they carry information the prose cannot, styled with a consistent pastel palette that imports cleanly into Excalidraw.
- Invokes the `humanize-text` skill on the draft to remove AI-sounding prose.
- Writes the file to the path you choose. Does not commit, push, or mutate git state.

## Shared style and diagram rules

Two files at the plugin root are shared across all doc types:

- [STYLE.md](STYLE.md) — prose rules on top of humanisation.
- [MERMAID.md](MERMAID.md) — allowed diagram types (flowchart, sequence, class, ER), pastel palette, Excalidraw import tips.

Each doc type adds its own `TEMPLATE.md` next to the `SKILL.md`.

## Dependencies

- [`humanize-text`](../humanize-text) plugin, same marketplace. If missing, the skill falls back to applying the humanisation rules inline from STYLE.md.

## What it does not do

- Does not read the codebase to infer architecture. If you want a design doc to reflect existing code, ask Claude to explore first and then run this skill.
- Does not commit or push the generated file.
- Does not support doc types other than Feature Design yet.

## Adding a new doc type

1. Create `skills/<doc-type>/SKILL.md` following the same six-step process pattern as `feature-design`.
2. Add `skills/<doc-type>/TEMPLATE.md` with the section structure for that doc type.
3. Reuse the shared [STYLE.md](STYLE.md) and [MERMAID.md](MERMAID.md) by linking to them with `../../` relative paths.
4. Keep the humanise step (step 5 in the SKILL).
