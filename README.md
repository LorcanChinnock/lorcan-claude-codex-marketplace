# lorcan-claude-marketplace

Personal [Claude Code](https://docs.claude.com/en/docs/claude-code) plugin marketplace.

## Install

Add the marketplace:

```
/plugin marketplace add LorcanChinnock/lorcan-claude-marketplace
```

Then browse and install plugins:

```
/plugin
```

Or install a specific plugin directly:

```
/plugin install <plugin-name>@lorcan-claude-marketplace
```

## Update

```
/plugin marketplace update lorcan-claude-marketplace
```

## Local development

Clone and point Claude at the working copy instead of GitHub:

```
git clone https://github.com/LorcanChinnock/lorcan-claude-marketplace.git
/plugin marketplace add /absolute/path/to/lorcan-claude-marketplace
```

## Plugins

- [`review-code`](plugins/review-code) — review a local branch diff or a GitHub PR; produces a local Markdown report with confidence-ranked findings and permalinks. Never posts to GitHub, never mutates files.
- [`simplify-code`](plugins/simplify-code) — simplify, tidy, clean up, deduplicate, or remove dead code from local files. Produces a confidence-scored proposal report and applies the survivors (proposal only in plan mode).
- [`describe-pr`](plugins/describe-pr) — write a conventional-commits title and PR description for the current branch from the raw diff vs base. Never pushes, commits, or opens a PR.
- [`humanize-text`](plugins/humanize-text) — rewrite text to remove signs of AI-generated writing: inflated significance, promotional language, em-dash overuse, AI vocabulary, bolded-header bullets, sycophantic openers.
- [`handle-review`](plugins/handle-review) — structured workflow for responding to code review or other critical feedback. Enforces verify-before-implement, reasoned push-back, one-item-at-a-time execution, and no performative agreement.
- [`docgen`](plugins/docgen) — generate software engineering documentation in Markdown. Runs a structured questionnaire, drafts to a fixed template, adds Mermaid diagrams where they help, and humanises the prose. Currently supports Feature Design docs; more doc types drop in as sibling skills.
- [`rubber-duck-planner`](plugins/rubber-duck-planner) — stress-test a technical idea or implementation plan. Acts as a critical sparring partner through four phases (clarify, challenge, alternatives, converge) and produces a consolidated plan summary on exit.

Register each new plugin by adding an entry to the `plugins` array in `.claude-plugin/marketplace.json`.

## Conventions

- **Versioning**: each plugin's `version` lives in its own `.claude-plugin/plugin.json` (not in the marketplace entry). The Claude Code docs note that for relative-path marketplaces the spec prefers the marketplace entry, but `plugin.json` wins silently when both are set and keeping it with the plugin keeps the bump local to the change. Only ever set the version in one place.
- **Bumping**: bump `version` in `plugin.json` whenever you change that plugin's skills, agents, hooks, commands, or other user-facing behavior. Patch for small tweaks, minor for new features, major for breaking changes.
- **Validation**: run `claude plugin validate .` from the repo root before pushing. The marketplace and every plugin should validate cleanly.

## License

[MIT](LICENSE).
