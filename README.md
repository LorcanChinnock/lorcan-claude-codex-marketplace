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

- [`review-management`](plugins/review-management) — PR lifecycle skills: `describe-pr` writes a conventional-commits title and humanised PR body from the raw diff vs base; `handle-review` processes incoming code review feedback with verify-before-implement, reasoned push-back, one item at a time, and no performative agreement. Neither skill pushes, merges, or posts to GitHub.
- [`humanize`](plugins/humanize) — rewrite text and code to remove signs of AI-generated writing. Prose skill (`humanize-text`) handles inflated significance, promotional language, em-dash overuse, AI vocabulary, bolded-header bullets, sycophantic openers. Code skill (`humanize-code`) renames verbose identifiers, simplifies comments and docstrings, and cleans up log / error / user-facing strings for non-native English readers.
- [`tech-docs`](plugins/tech-docs) — write technical docs (architecture overviews, feature designs, runbooks, getting-started guides, READMEs, tech-debt notes, how-tos, implementation plans, RFCs, and more). Asks the doc type and audience first, runs targeted clarifying questions one at a time, then drafts in plain language with structured markdown and pastel mermaid diagrams where they help. Output is humanised.
- [`git-management`](plugins/git-management) — local-only git workflow skills for stacked branch development. `stack-splitter` breaks a large branch into a chain of independently mergeable stacked branches via cherry-pick. `stack-rebase` rebases an entire chain atomically using `git rebase --update-refs` (git ≥ 2.38). Never pushes or writes to remote.

Register each new plugin by adding an entry to the `plugins` array in `.claude-plugin/marketplace.json`.

## Conventions

- **Versioning**: each plugin's `version` lives in its own `.claude-plugin/plugin.json` (not in the marketplace entry). The Claude Code docs note that for relative-path marketplaces the spec prefers the marketplace entry, but `plugin.json` wins silently when both are set and keeping it with the plugin keeps the bump local to the change. Only ever set the version in one place.
- **Bumping**: bump `version` in `plugin.json` whenever you change that plugin's skills, agents, hooks, commands, or other user-facing behavior. Patch for small tweaks, minor for new features, major for breaking changes.
- **Validation**: run `claude plugin validate .` from the repo root before pushing. The marketplace and every plugin should validate cleanly.

## License

[MIT](LICENSE).
