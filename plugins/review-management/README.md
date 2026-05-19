# review-management

PR lifecycle skills covering both ends of the review process: writing PR descriptions and handling incoming code review feedback.

Neither skill pushes, merges, approves, or posts to GitHub. `gh` is used read-only where needed.

## Skills

### `describe-pr`

Writes a conventional-commits title and humanised PR description for the current branch.

**When to use:** You're ready to open or update a PR and want a well-structured description.

**What it does:**
1. Reads the raw diff vs base and recent commit messages
2. Separates what the diff already shows from what only the author knows
3. Asks targeted why-first clarifying questions (motivation, trade-offs, constraints)
4. Writes a conventional-commits title plus a fixed-template body (Release note, Summary, Testing, Feature flag, Follow-ups) in dropped-subject active voice

Output goes to the conversation. Writes to a file only if you explicitly provide a path.

---

### `handle-review`

Processes incoming code review feedback with rigour: verify first, implement second, push back when wrong.

**When to use:** You've received PR comments, a design review, or any written critique and need to work through it systematically.

**What it does:**
1. Reads all feedback before acting
2. Restates and verifies each item against the codebase (LSP-first, then grep)
3. Pushes back with evidence when a suggestion is incorrect or unnecessary
4. Implements correct feedback one item at a time with testing between each
5. Never produces performative agreement ("Great point!", "Thanks!")

---

## Requirements

- `git` in PATH
- `gh` CLI for reading PR context (optional but recommended)

## Installation

Install via the Claude marketplace, or reference this directory with `--plugin-dir`.
