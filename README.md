# lorcan-claude-codex-marketplace

Personal Claude Code and Codex compatible plugin marketplace.

## Claude Code install

Add the marketplace:

```
/plugin marketplace add LorcanChinnock/lorcan-claude-codex-marketplace
```

Then browse and install plugins:

```
/plugin
```

Or install a specific plugin directly:

```
/plugin install <plugin-name>@lorcan-claude-codex-marketplace
```

Update:

```
/plugin marketplace update lorcan-claude-codex-marketplace
```

For local development, clone and point Claude at the working copy instead of GitHub:

```
git clone https://github.com/LorcanChinnock/lorcan-claude-codex-marketplace.git
/plugin marketplace add /absolute/path/to/lorcan-claude-codex-marketplace
```

## Codex install

Add the marketplace:

```
codex plugin marketplace add LorcanChinnock/lorcan-claude-codex-marketplace
```

Then install or enable plugins through Codex's plugin UI or marketplace flow.

For local development, point Codex at the working copy:

```
codex plugin marketplace add /absolute/path/to/lorcan-claude-codex-marketplace
```

## Plugins

- [`review-code`](plugins/review-code) — review a local branch diff or a GitHub PR; produces a local Markdown report with confidence-ranked findings and permalinks. Never posts to GitHub, never mutates files.
- [`simplify-code`](plugins/simplify-code) — simplify, tidy, clean up, deduplicate, or remove dead code from local files. Produces a confidence-scored proposal report and applies the survivors (proposal only in plan mode).
- [`describe-pr`](plugins/describe-pr) — write a conventional-commits title and PR description for the current branch from the raw diff vs base. Never pushes, commits, or opens a PR.
- [`humanize-text`](plugins/humanize-text) — rewrite text to remove signs of AI-generated writing: inflated significance, promotional language, em-dash overuse, AI vocabulary, bolded-header bullets, sycophantic openers.
- [`handle-review`](plugins/handle-review) — structured workflow for responding to code review or other critical feedback. Enforces verify-before-implement, reasoned push-back, one-item-at-a-time execution, and no performative agreement.
- [`docgen`](plugins/docgen) — generate software engineering documentation in Markdown. Runs a structured questionnaire, drafts to a fixed template, adds Mermaid diagrams where they help, and humanises the prose. Currently supports Feature Design docs; more doc types drop in as sibling skills.
- [`rubber-duck-planner`](plugins/rubber-duck-planner) — stress-test a technical idea or implementation plan. Acts as a critical sparring partner through four phases (clarify, challenge, alternatives, converge) and produces a consolidated plan summary on exit.
- [`coralogix`](plugins/coralogix) — query Coralogix logs/traces/metrics, design alerts, and design dashboards via the Coralogix MCP. Read-only by default; alert mutations require explicit confirmation.

Register each new plugin in both marketplaces:

- Claude: add an entry to `.claude-plugin/marketplace.json`.
- Codex: add an entry to `.agents/plugins/marketplace.json`.

## Conventions

- **Shared cores**: put provider-neutral behavior in `plugins/<name>/shared/`. Do not name runtime tools there; describe capabilities such as file read/write, shell, question tool, delegation/subagent capability, and language-aware navigation.
- **Runtime wrappers**: keep Claude wrappers in `skills/<skill>/SKILL.md` and Codex wrappers in `codex/skills/<skill>/SKILL.md`. Wrappers stay thin: frontmatter plus the runtime-specific mapping to the shared core.
- **Versioning**: each plugin's version lives in both `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json`. User-facing behavior changes bump both manifests for the affected plugin.
- **Bumping**: patch for small tweaks, minor for new features, major for breaking changes.
- **Validation**: run `claude plugin validate .` and `codex plugin marketplace add /absolute/path/to/lorcan-claude-codex-marketplace` before pushing. Confirm Codex sees all eight plugins.

## License

[MIT](LICENSE).
