# tech-docs

Write technical docs that read clearly to a stated audience. The skill asks the doc type and audience first, runs targeted clarifying questions one at a time, then drafts using a type-specific template, plain language, structured markdown, and pastel mermaid diagrams where they help. Output is humanised; the obvious LLM tells are scrubbed before printing.

## Install

```
/plugin install tech-docs@lorcan-claude-marketplace
```

## Invoke

```
/tech-docs:write-doc
```

Or ask in conversation ("write me an architecture overview for the orders domain", "draft a runbook for the payment timeout alert"). Both routes run the same skill.

## What it does

- Always asks the doc type and the intended audience first. Audience drives depth, jargon, and what to assume.
- Asks the rest one question at a time, never a batched numbered list.
- Ships quick references for architecture overview (AO), feature design, runbook, getting started, README, tech-debt note, how-to, implementation plan, and RFC. Off-list types are accepted.
- Drafts in plain language with sentence-case headings, parallel bullets, fenced code blocks, and small targeted tables.
- Adds mermaid diagrams (flowchart, sequence, class, ER) only when they shorten the explanation. Diagrams use a pastel palette, descriptive node names, no `<br>` tags by default, and clear `subgraph` groupings.
- Runs an explicit audience-translation pass before printing: marks every term the named audience may not know, then defines, swaps, or cuts each one.
- Scrubs AI tells before printing: em dashes, AI vocabulary, bolded-header bullets, sycophantic openers, generic upbeat closers, curly quotes, decorative emojis, title case in body text.
- Asks for the output path before writing. Never overwrites without confirmation.

## What it does not do

- Does not publish; no push, no commit, no posting anywhere.
- Does not invent facts. Unknowns are marked `TBD`.
- Does not skip the audience question, even when the prompt hints at it.

## Files

- `skills/write-doc/SKILL.md`: process and self-check.
- `skills/write-doc/STYLE.md`: markdown structure and plain-language rules.
- `skills/write-doc/HUMANIZE.md`: the AI tells to scrub.
- `skills/write-doc/MERMAID.md`: pastel palette and worked examples for the four chart types.
- `skills/write-doc/TEMPLATES.md`: quick-reference templates per doc type, with adaptation notes for non-engineering audiences.
