# author-plugin

Generate a Claude Code plugin from a plain-English requirement. Picks the right surfaces, scaffolds the files, checks them against a deterministic rubric, returns a fresh-context reviewer verdict, and proposes a `marketplace.json` diff.

## Install

```
/plugin install author-plugin@lorcan-claude-marketplace
```

## Invoke

Ask for it in conversation (the skill auto-matches on description), or invoke explicitly:

```
/author-plugin:author-plugin
```

Or pass a free-form requirement inline: `/author-plugin:author-plugin a plugin that formats dates`. The skill asks a short batched intake if the requirement is under-specified.

## What it does

- Preflights for a name collision in `plugins/`.
- Asks one batched intake covering problem, triggers, integrations, delegation, and TDD opt-in.
- Chooses surfaces from `SURFACES.md` — skill by default (skills cover the slash-command entry point too), subagents for fresh-context work, hooks only for reactive automation, MCP only for external stdio bridges.
- Writes `plugin.json`, `README.md`, and every chosen surface from `TEMPLATES.md`.
- Runs a deterministic self-check: JSON/YAML parse, name regex, forbidden agent fields (`hooks`/`mcpServers`/`permissionMode`), description-as-trigger rule, word budgets.
- Proposes a `marketplace.json` diff and writes it on your confirmation.
- Runs the `review-plugin` sub-agent in fresh context for an unbiased `KEEP` / `REVISE` / `BLOCK` verdict.
- Optional TDD loop (see `TDD.md`) if you opt in at intake.
- Prints a final report with files created, reviewer verdict, and a suggested commit message.

## What it does not do

- Does not publish plugins (no `npm`, no `git push`, no PR creation).
- Does not run generated plugins end-to-end.
- Does not auto-commit. The final report prints a suggested commit message only.
- Does not ship hooks or an MCP server of its own — the generator has no reactive trigger or external integration.
