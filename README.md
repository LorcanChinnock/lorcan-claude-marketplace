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

Register each new plugin by adding an entry to the `plugins` array in `.claude-plugin/marketplace.json`.

## License

[MIT](LICENSE).
