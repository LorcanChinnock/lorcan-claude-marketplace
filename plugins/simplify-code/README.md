# simplify-code

Hub-and-spoke code simplifier for local code. The `/simplify-code` skill is a thin dispatcher; the pipeline runs inside the `simplify-code` agent so its orchestration context stays out of your main conversation. The agent extracts the functional and non-functional contract first, dispatches parallel simplifier sub-agents, validates every proposal against the contract, scores, then applies the survivors (or reports only in plan mode).

## Install

```
/plugin install simplify-code@lorcan-claude-marketplace
```

## Invoke

```
/simplify-code:simplify-code                 # branch mode (default): diff vs merge-base with default branch
/simplify-code:simplify-code staged          # staged changes
/simplify-code:simplify-code working         # all uncommitted changes
/simplify-code:simplify-code path/to/file.ts # whole-file mode, one or more paths
```

The skill also auto-triggers when the user asks in conversation to simplify, tidy, clean up, deduplicate, or remove dead code from local files. In either case the skill hands off to the `simplify-code` agent, which runs the pipeline in its own context and returns the report and final status.

## What it does

- Extracts the code's public API shape, observable behaviour, and implicit test invariants as a contract.
- Tiers the work by scope size and dispatches spokes in parallel: Dead-weight, Redundancy, Modernization (verified via `context7` MCP), Control-flow, Boundary-audit.
- Validates every proposal against the contract, scores 0–100, drops anything under 75.
- Prints a proposal report, then applies survivors bottom-up per file.
- Runs the repo's test command once if one is unambiguous; never auto-reverts on failure.

## What it does not do

- Does not commit, push, or mutate remote state.
- Does not introduce new dependencies, abstractions, or architectural changes.
- Does not run destructive git commands. Revert hint is always file-scoped (`git checkout <sha> -- <files>`).
- Does not guess modern idioms from training data. If `context7` is unavailable, the Modernization spoke is skipped.
- Does not operate outside the resolved scope — unrelated dirty work is never at risk.
