# pr-description

Write a conventional-commits title and PR description for the current branch. Reads the raw diff vs base, asks only what the diff cannot answer, then writes a humanised body with a Mermaid diagram where it helps.

## Install

```
/plugin install pr-description@lorcan-claude-marketplace
```

## Invoke

```
/pr-description
```

Or ask in conversation ("write a PR description for this branch"). Both routes run the same skill on the current branch against its base.

## What it does

- Reads the raw diff vs base, never per-commit.
- Separates what the diff proves from what only the author knows, then asks.
- Produces a fixed template: Release note, Summary, Testing steps, Feature flag, Follow-up issues.
- Adds a Mermaid diagram for architectural, flow, state, or data-model changes when one adds information.
- Prose follows humaniser rules: no AI vocabulary, no em dashes, no bolded-header bullets.

## What it does not do

- Does not open the PR, push, or mutate git state.
- Does not paste the raw diff back into the output.
- Does not write to disk unless you name a path.
