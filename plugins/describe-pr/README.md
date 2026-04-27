# describe-pr

Write a conventional-commits title and PR description for the current branch. Reads the raw diff vs base, asks heavily about *why* the change was made (the diff already shows what and how), then writes a humanised body in dropped-subject active voice with a Mermaid diagram where it helps.

## Install

```
/plugin install describe-pr@lorcan-claude-marketplace
```

## Invoke

```
/describe-pr:describe-pr
```

Or ask in conversation ("write a PR description for this branch"). Both routes run the same skill on the current branch against its base.

## What it does

- Reads the raw diff vs base, never per-commit.
- Separates what the diff proves from what only the author knows, then asks. Question budget weighted toward motivation, tradeoffs, and constraints.
- Produces a fixed template: Release note, Summary, Testing steps, Feature flag, Follow-up issues.
- Writes Summary and Testing in dropped-subject active voice ("Added X because Y"). No "I", no "this PR", no "the author". Release note and headings stay neutral.
- Adds a Mermaid diagram for architectural, flow, state, or data-model changes when one adds information.
- Prose follows humaniser rules: no AI vocabulary, no em dashes, no bolded-header bullets, no first-person pronouns, no third-person "the author" or "this PR".

## What it does not do

- Does not open the PR, push, or mutate git state.
- Does not paste the raw diff back into the output.
- Does not write to disk unless you name a path.
