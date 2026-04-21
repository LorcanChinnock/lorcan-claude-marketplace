# pr-description

Generate a conventional-commits title and a cohesive PR description from the raw diff vs the base branch. Gathers context, asks clarifying questions, then writes.

## Install

```
/plugin install pr-description@lorcan-claude-marketplace
```

## Invoke

```
/pr-description
```

Runs on the current branch.

## What it does

- Reads the raw diff vs base — never per-commit.
- Surfaces what it knows vs what it doesn't, and asks before writing.
- Produces a fixed template: Release note, Summary, Testing steps, Feature flag, Follow-up issues.
- Includes Mermaid diagrams for architectural, flow, or data-model changes when they add information.
- Prose follows humaniser rules: no AI vocabulary, no em dashes, no bolded-header bullets.

## What it does not do

- Does not open the PR, push, or mutate git state.
- Does not write to disk unless you ask.
