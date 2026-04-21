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

## Layout

```
.claude-plugin/marketplace.json   # marketplace manifest
plugins/<plugin-name>/            # each plugin lives here
  .claude-plugin/plugin.json      # plugin manifest
  commands/ agents/ skills/ hooks/ .mcp.json   # optional
```

Register each new plugin by adding an entry to the `plugins` array in `.claude-plugin/marketplace.json`.
