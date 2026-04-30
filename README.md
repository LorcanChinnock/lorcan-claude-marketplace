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

- [`describe-pr`](plugins/describe-pr) — write a conventional-commits title and PR description for the current branch from the raw diff vs base. Never pushes, commits, or opens a PR.
- [`humanize`](plugins/humanize) — rewrite text and code to remove signs of AI-generated writing. Prose skill (`humanize-text`) handles inflated significance, promotional language, em-dash overuse, AI vocabulary, bolded-header bullets, sycophantic openers. Code skill (`humanize-code`) renames verbose identifiers, simplifies comments and docstrings, and cleans up log / error / user-facing strings for non-native English readers.
- [`handle-review`](plugins/handle-review) — structured workflow for responding to code review or other critical feedback. Enforces verify-before-implement, reasoned push-back, one-item-at-a-time execution, and no performative agreement.
- [`tech-docs`](plugins/tech-docs) — write technical docs (architecture overviews, feature designs, runbooks, getting-started guides, READMEs, tech-debt notes, how-tos, implementation plans, RFCs, and more). Asks the doc type and audience first, runs targeted clarifying questions one at a time, then drafts in plain language with structured markdown and pastel mermaid diagrams where they help. Output is humanised.

Register each new plugin by adding an entry to the `plugins` array in `.claude-plugin/marketplace.json`.

## Conventions

- **Versioning**: each plugin's `version` lives in its own `.claude-plugin/plugin.json` (not in the marketplace entry). The Claude Code docs note that for relative-path marketplaces the spec prefers the marketplace entry, but `plugin.json` wins silently when both are set and keeping it with the plugin keeps the bump local to the change. Only ever set the version in one place.
- **Bumping**: bump `version` in `plugin.json` whenever you change that plugin's skills, agents, hooks, commands, or other user-facing behavior. Patch for small tweaks, minor for new features, major for breaking changes.
- **Validation**: run `claude plugin validate .` from the repo root before pushing. The marketplace and every plugin should validate cleanly.

## License

[MIT](LICENSE).
