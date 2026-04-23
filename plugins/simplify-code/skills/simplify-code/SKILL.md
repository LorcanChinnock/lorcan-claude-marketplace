---
name: simplify-code
description: Use when the user asks to simplify, tidy, clean up, deduplicate, or remove dead code from local files — branch diff vs base, staged changes, working tree, or specified paths. Produces a confidence-scored proposal report and applies the survivors (proposal only in plan mode).
argument-hint: "[branch | staged | working | <path>...]"
allowed-tools:
  - Agent
---

Dispatch to the `simplify-code` subagent, which runs the contract-first hub-and-spoke simplification pipeline in its own context. Do not run the pipeline yourself — the agent is the orchestrator.

## How to dispatch

1. Interpret the user's argument as the scope:
   - no arg or `branch` → `branch` (diff vs merge-base with default branch)
   - `staged` → staged changes
   - `working` → all uncommitted changes
   - anything else → treat as one or more paths

2. Invoke the agent via the `Agent` tool with `subagent_type: simplify-code`. Pass the scope verbatim in the prompt, e.g.:

   > Run the simplification pipeline with scope: `<arg>`. Follow your full process (preflight → gate → locate → contract → spokes → validate → score → consolidate → report → apply). Print the report before any `Edit`.

3. Relay the agent's report and final status to the user exactly as returned. Do not summarise away the file-scoped revert hint.

## What you (the skill orchestrator) must not do

- Do not extract the contract, dispatch spokes, or score proposals in the main conversation. That is the agent's job, and running it here defeats the context isolation.
- Do not apply edits yourself. The agent applies them; if the agent is refused (plan mode), the report is the deliverable.
- Do not expand the scope beyond what the user specified.
