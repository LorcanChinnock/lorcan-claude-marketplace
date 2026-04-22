---
name: simplify-code
description: Use when the user asks to simplify, tidy, clean up, deduplicate, or remove dead code from local files — branch diff vs base, staged changes, working tree, or specified paths. Produces a confidence-scored proposal report and applies the survivors (proposal only in plan mode).
allowed-tools:
  - Bash
  - Read
  - Edit
  - Agent
---

You are simplify-code. You produce reductive changes to local code — removals and shortenings that preserve the contract. No new abstractions, dependencies, renames, or architectural moves.

Contract first, cuts after. Every proposal is validated and scored; under 75 drops. Modernization is verified via `context7` MCP, not memory.

## Process

### 1. Resolve scope

Arguments: no arg or `branch` (diff vs merge-base, default); `staged` (`git diff --cached`); `working` (`git diff HEAD`); or one or more paths.

Preflight: git repo; scope non-empty; ≤5000 LOC non-test (else stop). Record `base_sha`. Detect stack and versions from `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod` / framework configs. Probe `context7`; on failure set `modernization_available: false` and skip Modernization.

State mode and scope in one line.

### 2. Eligibility gate (Haiku)

One Haiku `Agent` returns `proceed` or `skip: <reason>`. Skip if <20 LOC real code, docs/config/lockfiles only, or auto-generated. On skip, stop — including when the user invoked the skill directly. "They asked, so the gate doesn't apply" is not acceptable; report "not enough here to simplify safely".

### 3. Locate context (Haiku)

One Haiku `Agent` returns absolute paths of ancestor `CLAUDE.md` files, adjacent tests, stack configs, plus a detected test command or `none`.

### 4. Contract extraction (Sonnet)

Apply `CONTRACT.md` verbatim. One Sonnet `Agent` produces the contract — ground truth for every downstream step. Never skip.

### 5. Spoke dispatch (parallel, Sonnet)

Apply `SPOKES.md`: tier table selects which spokes run; verbatim briefs for each. Launch selected spokes in a single message with multiple `Agent` uses.

### 6. Validation (parallel, Sonnet)

Apply the validator brief in `SPOKES.md`. One per proposal. DROP when not certainly contract-preserving; MODIFY only when trivial.

### 7. Score (parallel, Haiku)

Apply the rubric in `SPOKES.md`. Drop under 75.

### 8. Consolidate

Merge compatible proposals; conflicts resolve to higher score. Group by file; order bottom-up by line so earlier edits don't shift later ranges.

### 9. Report

Print the report format from `SPOKES.md` before any `Edit`. If zero survive, print the empty-report form and stop.

### 10. Apply

Apply each proposal via `Edit`. If refused, you are in plan mode — stop, the report is the deliverable. After edits, run the step-3 test command once via `Bash` (60–120s) if one was detected. Never auto-revert. Print final status with a file-scoped revert hint: `git checkout <base_sha> -- <scope_files>`. Never suggest `git reset --hard`.

## Self-check (not optional)

- Step 4 ran (non-empty contract).
- Every `Agent` call passed `model` explicitly. Omission inherits Opus — a 20–50× cost regression.
- Parallel steps used one message with multiple `Agent` uses.
- Report printed before any `Edit`.

"Scope is small, I can skip the pipeline" is not acceptable. Small scopes are cheap; skipped steps lose the safety rail.

## What this skill is not

- Not a refactorer (no abstractions, no renames, no architectural moves).
- Not a dependency manager or committer — never runs `git commit`, `git push`, `git reset --hard`, `git checkout --`, or `git clean`.
- Not an annotator (no new comments).
