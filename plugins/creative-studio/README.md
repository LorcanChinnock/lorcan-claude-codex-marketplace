# creative-studio

Creative design skills for ideation and direction.

## Skills

### `creative-director`

Acts as a senior UX/creative director. Give it a design brief — product, brand, UI system, campaign, visual identity — and it produces opinionated, research-led design direction.

**What it does:**

1. Clarifies the brief if needed (one question at a time)
2. Runs live web searches to identify current AI design tropes to avoid (specific, named patterns from editorial critique)
3. Runs live web searches for fresh references relevant to the brief — domain-specific, plus oblique/adjacent inspiration
4. Develops three strategically distinct design concepts, each named, reference-linked, and explicit about what it rejects
5. Recommends one direction with a specific reason grounded in the research or the brief
6. Proposes concrete next steps

**Why live research matters:** Design trends shift fast. AI-generated design output is saturating the space. Baking in a list of "current tropes to avoid" would make it stale by the time it's used. Every session the skill runs fresh searches so the tropes and references reflect what's actually happening now.

**Triggers on:** "design a UI for X", "act as creative director", "design concepts for X", "brand identity for X", "give me design direction for X", "visual language for X", and similar.

**Tools used:** WebSearch, WebFetch (for live research at invocation), AskUserQuestion (brief clarification only).

---

## Installation

Install via the Claude marketplace:

```
/plugin install creative-studio@lorcan-claude-marketplace
```

Or reference locally with `--plugin-dir`:

```
cc --plugin-dir /path/to/lorcan-claude-marketplace/plugins/creative-studio
```
