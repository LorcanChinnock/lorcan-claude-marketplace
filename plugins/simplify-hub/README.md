# simplify-hub

Hub-and-spoke code simplifier for local code. Extracts the functional + non-functional contract first, dispatches specialised spokes in parallel, validates every proposal against the contract, then applies the survivors.

## Install

```
/plugin install simplify-hub@lorcan-claude-marketplace
```

## Invoke

The plugin registers a `simplify-hub` agent. Pass a scope argument:

- `branch` — current branch diff vs its base (default).
- `staged` — staged changes only.
- `working` — all uncommitted changes.
- One or more file or directory paths.

In plan mode, the proposal report is the deliverable. In run mode, surviving proposals are applied.

## What it does

- Contract first, cuts after — no simplification without understanding.
- Confidence-weighted filtering — proposals under 75 are dropped.
- Specialised spokes: dead-weight, redundancy, modernization, control-flow, boundary audit.
- Modernization is evidence-based — uses the `context7` MCP to verify idioms against current docs.

## What it does not do

- No remote operations.
- No changes outside the requested scope.
- No proposals that would violate the extracted contract.
