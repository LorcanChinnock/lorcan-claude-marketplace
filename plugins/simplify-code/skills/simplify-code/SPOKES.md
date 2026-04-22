# SPOKES.md — spoke briefs, validator, scoring, report format

Loaded by steps 5–9 of SKILL.md. Each section below is load-bearing.

## Tier table (step 5 spoke selection)

| Tier | Signals | Spokes to run |
|---|---|---|
| Trivial | ≤50 LOC changed, 1–2 files | Dead-weight only |
| Small | ≤200 LOC, localised | Dead-weight, Redundancy, Control-flow |
| Medium | ≤1000 LOC, or touches config/framework code | All except Boundary-audit (unless the contract names a trust boundary) |
| Large | >1000 LOC, or spans multiple modules | All five |

Additional rules:
- Skip Modernization if `modernization_available: false` (context7 probe failed).
- Add Boundary-audit at any tier when the contract has `observable_behaviour` entries naming user input, external APIs, filesystem, network, deserialization, IPC, or subprocess output.
- State the chosen tier and spoke list in a one-line status update.

Each spoke receives: scope files, scope regions, contract (from step 4), ancestor `CLAUDE.md` paths, stack info. Each returns a list of proposals as `{file, lines, current, proposed, rationale, spoke}`. Spokes are read-only — no edits, no writes.

## Spoke briefs (pass verbatim)

### Dead-weight (Sonnet)

> Find code that exists but does nothing useful: unused imports, variables, or parameters; impossible-case handling (null checks on values the type system guarantees non-null, try/catch around code that cannot throw, fallbacks for unreachable branches); single-use abstractions (helpers called once, interfaces with one implementation, factories wrapping a constructor); speculative features not currently called; defensive validation on trusted internal calls. Do not flag validation at real trust boundaries — that is the Boundary-audit spoke's job. For each finding, return the current code and the direct replacement.

### Redundancy (Sonnet)

> Find duplication and derivable state: copy-pasted blocks; near-duplicates differing only in a parameter; repeated computations in the same scope; state stored that could be derived from other state; the same condition checked multiple times. Propose the simplest consolidation. Do not invent an abstraction unless three or more call sites justify it — two copies is fine.

### Modernization (Sonnet, uses context7 MCP)

> Identify idioms that can be shortened using current-version features of the detected stack. Examples: Promise chains → async/await, class components → hooks, manual loops → map/filter/reduce where clearer, callback APIs → async, older Python patterns → structural pattern matching / walrus / f-strings where clearer. Before proposing each framework or library change, use `context7` (`resolve-library-id` then `query-docs`) to verify the idiom is current best practice for the detected version — not deprecated, not experimental, not upgrade-only. Cite the library and version checked for each proposal. Do not propose changes that require a framework, runtime, or language-version upgrade. If `context7` returns no answer for a proposal, drop that proposal rather than guessing from memory.

### Control-flow (Sonnet)

> Flatten nesting without changing behaviour: nested conditions → guard clauses with early returns; `else` after `return` → drop the `else`; deep ternaries → clearer if/else; multi-line boolean gymnastics → a named intermediate or equivalent simpler form; switch/match that could be a lookup. Goal is obviousness — no clever one-liners.

### Boundary-audit (Sonnet)

> Distinguish real trust boundaries (user input, external APIs, filesystem, network, deserialization, IPC, subprocess output) from internal calls (functions called only by known internal code within the same module, package, or service). Paranoid validation on internal calls is dead weight — remove it. Real boundary checks stay. For each finding, classify `INTERNAL-REMOVE` or `BOUNDARY-KEEP` and justify based on where the data actually comes from.

## Validator brief (step 6, pass verbatim — one Sonnet `Agent` per proposal, in parallel)

> Given this simplification proposal and the extracted contract, does the proposed code preserve every element of the contract?
>
> Check:
> - Public API shape: signature, exceptions, return semantics identical?
> - Observable side effects: same events fired, same state mutated, same order?
> - Error paths: same failures surface the same way to callers?
> - Non-functional constraints from the contract preserved (streaming, concurrency, perf class)?
> - Test expectations from adjacent tests still satisfied?
>
> Return `{verdict: KEEP | DROP | MODIFY, reason: <one sentence>, modified_proposal?: <revised patch>}`.
>
> Prefer DROP over MODIFY unless the modification is trivial and clearly preserves the contract. If you are not certain the contract holds, DROP.

Drop every DROP. Replace MODIFY proposals with their `modified_proposal`.

## Scoring rubric (step 7, pass verbatim — one Haiku `Agent` per proposal, in parallel)

> Score 0–100 for whether this simplification is a clear net improvement with no behavioural risk.
>
> - 0: Proposed code is actually more complex, or changes behaviour.
> - 25: Marginal — stylistic preference, not clearly better.
> - 50: Real improvement, but small and local.
> - 75: Clear simplification — multiple lines removed or meaningfully clarified, no behavioural risk, or a current-idiom modernization confirmed by docs.
> - 100: Large simplification (whole block collapsed, abstraction removed, boilerplate eliminated) with the validator's KEEP verdict.
>
> Return `{score, rationale}`.

Drop under 75. The bar is higher than code review because survivors will be applied.

## Report format (step 9)

Print this exactly before any `Edit`:

```
### Code simplification — <scope description> (base <short-sha>)

Contract summary:
- <bullet>
- <bullet>

Spokes run: <list>
[Uncommitted unrelated changes present — will intermingle with applied changes.]
[Tests detected: `<command>`.]

Proposals (confidence ≥75, <M> filtered out):

1. [confidence: <n>] <path>:<start>-<end> — <brief description>
   Reason: <dead-weight | redundancy | modernization (<lib>@<ver>) | control-flow | boundary>
   Before:
       <current code, indented>
   After:
       <proposed code, indented>

2. ...
```

If zero proposals survive filtering:

```
### Code simplification — <scope description>

Spokes run: <list>

No simplifications above confidence 75. Nothing to apply.
```

Final status line after step 10:

```
Applied <N> simplifications across <F> files. Tests: <passed | failed | not run>.
Revert with: git checkout <base_sha> -- <space-separated scope_files>
Review with: git diff <base_sha> -- <space-separated scope_files>
```

The revert command is always file-scoped — never `git reset --hard`, which would also wipe unrelated uncommitted work.

## What not to propose (drop at the spoke level, never surface)

- API signature changes (param names or positions, return shape, thrown or raised types).
- Introducing new dependencies.
- Framework, runtime, or language-version upgrades.
- Architectural refactors (module splits, renames, layer reorganization).
- Collapsing error handling that adjacent tests rely on.
- "Cleverness" that reduces line count but hurts readability.
- Style nits not backed by a `CLAUDE.md` rule or a verified current idiom.
- Collapsing two similar blocks into a parameterised abstraction (three or more call sites minimum).
- New comments — this is a simplifier, not an annotator.

## Reminders for the orchestrator

- Every `Agent` call must pass `model` explicitly. Haiku for steps 2, 3, 7; Sonnet for steps 4, 5, 6.
- Parallel steps (5, 6, 7) must be one message with multiple `Agent` uses. Sequential dispatch when parallel is possible is a bug.
- If total spoke findings exceed ~40, spokes are nit-picking. Tighten scoring and note the anomaly in the report header.
- If `context7` is unavailable when Modernization needs it, the spoke drops proposals rather than guessing. Note the skip in the report.
