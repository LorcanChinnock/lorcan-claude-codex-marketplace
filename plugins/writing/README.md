# writing

Writing quality skills covering both editing existing content and creating new documentation.

## Skills

### `humanize-text`

Rewrites prose to remove signs of AI-generated writing.

**Targets:** inflated significance, promotional language, em-dash overuse, AI vocabulary (`leverage`, `robust`, `seamless`, etc.), bolded-header bullets, sycophantic openers, title case in body text.

**Modes:** pasted text (rewrites inline) or file paths (proposes `Edit`s). Loads `references/PATTERNS.md` for dense or long inputs.

---

### `humanize-code`

Renames verbose identifiers, rewrites padded comments and docstrings, and cleans up log/error/user-facing strings.

**Rules:** drop filler nouns (`Manager`, `Handler`, `Util`), cut type echoes (`userList` → `users`), replace Latinate verbs (`utilise` → `use`), cut comments that restate the code, strip AI vocabulary from strings. Never changes behaviour or public API names without confirmation.

---

### `write-doc`

Authors technical documents from scratch with audience-aware plain language and pastel Mermaid diagrams.

**Doc types:** architecture overviews, feature designs, runbooks, getting-started guides, READMEs, tech-debt notes, how-tos, implementation plans, RFCs, and more.

**Process:** asks doc type and audience first, runs targeted clarifying questions one at a time, grounds the draft in the codebase, drafts with audience-translation and humanise passes before printing.

---

## Installation

Install via the Claude marketplace, or reference this directory with `--plugin-dir`.
