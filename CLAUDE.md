# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal Claude Code plugin marketplace. Content is markdown and JSON — there is no build, no test runner, no lint. "Validation" means JSON/YAML parses cleanly and the frontmatter follows the rubric in `plugins/author-plugin/skills/author-plugin/PATTERNS.md`.

## Layout

```
.claude-plugin/marketplace.json       # marketplace manifest (registers every plugin)
plugins/<name>/
  .claude-plugin/plugin.json          # plugin manifest
  skills/<name>/SKILL.md              # entry point (user- or model-invoked)
  skills/<name>/*.md                  # uppercase reference docs, loaded on demand
  agents/<name>.md                    # optional sub-agents
  README.md
```

Registering a new plugin means adding an entry to the `plugins` array in `.claude-plugin/marketplace.json`. Homepage template: `https://github.com/LorcanChinnock/lorcan-claude-marketplace/tree/main/plugins/<name>`. Plugins start at version `0.1.0`.

## Testing a plugin locally

```
/plugin marketplace add /Users/lorcan/dev/lorcan-claude-marketplace
/plugin install <plugin-name>@lorcan-claude-marketplace
```

After editing a plugin, refresh with `/plugin marketplace update lorcan-claude-marketplace`. Plugin skills are namespaced — invoke as `/<plugin>:<skill>`, not `/<skill>`.

## Surface choice

Authoritative guide: `plugins/author-plugin/skills/author-plugin/SURFACES.md`. Short version:

- **Skill** (default) — user-invoked workflow, conversational or `/<plugin>:<skill>`.
- **Subagent** — fresh context, restricted toolset, or parallel independent spokes.
- **Hook** — reactive on an event (PreToolUse, Stop, etc.). The harness runs it, not Claude.
- **MCP server** — bridges an external system over stdio JSON-RPC. Plugins ship stdio only.

Default to the simpler surface. A skill that calls a subagent beats a subagent that calls a skill. Commands (`commands/<name>.md`) and skills were merged in the official docs — skills are the format going forward. For a user-only entry point that Claude should not trigger autonomously, add `disable-model-invocation: true` to the skill frontmatter rather than reaching for a command file.

## House style (applies to all plugin content)

Rules from `PATTERNS.md`. Check before writing or editing:

- `SKILL.md` < 500 words. Heavy content goes to uppercase `.md` siblings, linked with plain markdown (**never** `@PATTERNS.md` — the `@` prefix force-loads and defeats progressive disclosure).
- `description` is a trigger, not a summary. Shape: `Use when <third-person triggers>. Returns <what the user gets>.` Banned phrases: `orchestrates`, `coordinates`, `dispatches`, `between tasks`, `first … then …`.
- Verb-first kebab-case names (`review-code`, not `code-review`). Match `^[a-z][a-z0-9-]*$`.
- Uppercase `.md` for reference docs (`PATTERNS.md`, `SURFACES.md`, `REFERENCE.md`) and for `SKILL.md`/`README.md`.
- No emojis. Plain copulas (`is`/`are`), not `serves as`/`functions as`. Sentence case in headings. No bolded-header bullets (`- **Label:** text`). Avoid em dashes in generated content.

## Frontmatter gotchas

- Skill `allowed-tools`: YAML list.
- Subagent `tools`: **comma-separated string**, not a YAML list.
- Subagent forbidden fields (Claude Code rejects them in plugin agents): `hooks`, `mcpServers`, `permissionMode`. Put those in the user's `.claude/settings.json`.
- `model: inherit` is the right subagent default; name the model (`haiku`/`sonnet`/`opus`) only when cost matters.

## Authoring and reviewing plugins

Use the `/author-plugin:author-plugin` skill to scaffold new plugins — it applies the rubric, invokes the fresh-context `review-plugin` subagent, and proposes the `marketplace.json` diff. Do not bypass it when creating a new plugin from scratch; manual authoring drifts from the rubric.

## Git conventions

- Conventional Commits (`feat:`, `fix:`, `docs:`, `refactor:`) — see `git log`.
- Diffs are reviewed vs `main` using the raw diff, not individual commits.
- Never commit without explicit request. Never push without explicit request.
