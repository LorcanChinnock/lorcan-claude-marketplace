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

- [`review-code`](plugins/review-code) — rigorous multi-agent review of a GitHub PR or the current branch's raw diff, with confidence-ranked findings.
- [`simplify-code`](plugins/simplify-code) — hub-and-spoke orchestrator for aggressive-but-safe local code simplification, validated against the extracted contract.
- [`describe-pr`](plugins/describe-pr) — generate a conventional-commits title and a structured PR description from the raw diff vs base.

## Layout

```
.claude-plugin/marketplace.json   # marketplace manifest
plugins/<plugin-name>/            # each plugin lives here
  .claude-plugin/plugin.json      # plugin manifest
  skills/<name>/SKILL.md          # user- or model-invoked skills (preferred)
  agents/<name>.md                # specialised sub-agents
  hooks/ .mcp.json                # optional
  commands/<name>.md              # legacy — prefer skills/
```

Register each new plugin by adding an entry to the `plugins` array in `.claude-plugin/marketplace.json`.

## License

[MIT](LICENSE).
