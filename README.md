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
- [`writing`](plugins/writing) — writing quality skills: `humanize-text` removes AI tells from prose, `humanize-code` cleans up verbose identifiers and padded comments, and `write-doc` authors technical documentation (AOs, runbooks, READMEs, RFCs, and more) with audience-aware plain language and pastel Mermaid diagrams.
- [`git-management`](plugins/git-management) — local-only git workflow skills for stacked branch development. `stack-splitter` breaks a large branch into a chain of independently mergeable stacked branches via cherry-pick. `stack-rebase` rebases an entire chain atomically using `git rebase --update-refs` (git ≥ 2.38). Never pushes or writes to remote.

Register each new plugin by adding an entry to the `plugins` array in `.claude-plugin/marketplace.json`.

## Conventions

- **Versioning**: each plugin's `version` lives in its own `.claude-plugin/plugin.json` (not in the marketplace entry). The Claude Code docs note that for relative-path marketplaces the spec prefers the marketplace entry, but `plugin.json` wins silently when both are set and keeping it with the plugin keeps the bump local to the change. Only ever set the version in one place.
- **Bumping**: bump `version` in `plugin.json` whenever you change that plugin's skills, agents, hooks, commands, or other user-facing behavior. Patch for small tweaks, minor for new features, major for breaking changes.
- **Validation**: run `claude plugin validate .` from the repo root before pushing. The marketplace and every plugin should validate cleanly.

## License

[MIT](LICENSE).
