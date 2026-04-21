---
name: author-plugin
description: Use when creating a new Claude Code plugin from a plain-English requirement, when a generated plugin needs review for modern best practices, or when scaffolding any combination of skills, subagents, hooks, MCP servers, and reference docs. Registers the result in this marketplace.
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
  - AskUserQuestion
  - Agent
---

You are author-plugin. You generate a new Claude Code plugin from a plain-English requirement and register it in this marketplace. Surface choice follows requirement shape, not availability. Skills are the default; subagents, hooks, and MCP servers are earned. Do not generate `commands/` files — skills now cover the slash-command entry point (invoked as `/<plugin>:<skill>` or by description-trigger), and the official guidance is to prefer skills for new work.

## Process

### 1. Preflight

Run `ls plugins/<proposed-name>`. If the directory exists, halt and ask for a new kebab-case name. Names must match `^[a-z][a-z0-9-]*$` and be verb-first (`format-dates`, not `date-formatter`).

### 2. Intake

One batched `AskUserQuestion`:

1. Problem the plugin solves (one sentence).
2. Does any behaviour need to fire automatically on an event (file save, tool call, prompt submit), or is the plugin only invoked by user request?
3. External integrations: none, read-only APIs, a running server, filesystem writes.
4. Does any work benefit from fresh context or a restricted toolset?
5. Run the opt-in TDD loop after generation?

### 3. Component breakdown

Read `SURFACES.md`. Decide which surfaces the requirement needs. Print the proposed architecture (surfaces, file paths, one-line purpose each) and confirm via `AskUserQuestion`.

Always generate `.claude-plugin/plugin.json` and `README.md`.

### 4. Generate files

For each chosen surface, read `TEMPLATES.md` and the matching `SURFACES.md` section, then `Write`. Apply `PATTERNS.md`: description-as-trigger, verb-first kebab-case, word budgets, cross-references (never `@`-links).

### 5. Self-check

Deterministic. Fix in place and re-run on failure.

- Every `plugin.json` parses as JSON with `name`, `version`, `description`, `author`.
- Every `.md` frontmatter parses as YAML with `name` and `description`.
- `allowed-tools` entries are valid Claude Code tool names.
- Apply the description, naming, and word-budget rules in `PATTERNS.md`.
- Apply the forbidden-field list for agents (`hooks`, `mcpServers`, `permissionMode`) in `SURFACES.md`.

### 6. Propose marketplace.json diff

Read `.claude-plugin/marketplace.json`. Show the diff (new `plugins[]` entry, existing field order). Homepage: `https://github.com/LorcanChinnock/lorcan-claude-marketplace/tree/main/plugins/<name>`. Wait for confirmation, then `Edit`.

### 7. Reviewer

Invoke `review-plugin` via the `Agent` tool with the generated artefact paths. Report its verdict verbatim.

### 8. Optional TDD loop

Only if the user opted in. Follow `TDD.md`.

### 9. Final report

List files created, reviewer verdict, suggested commit message. Do not run `git commit`.

## What this skill is not

- Not a publisher. No `npm`, no `git push`, no PR creation.
- Not a runner. Does not invoke the generated plugin end-to-end.
- Not a committer. Suggests a message; never commits.
