# CONTRACT.md — contract-extraction brief

Loaded by step 4 of SKILL.md. Pass this verbatim to a single Sonnet `Agent`, with the scope files and any adjacent tests the step-3 locator returned.

## Brief (pass verbatim)

> Read the scope files and any adjacent tests. Extract the functional and non-functional contract of this code — what any simplification must preserve.
>
> - Public API shape: exported functions, signatures, return types, thrown or raised errors.
> - Observable behaviour: side effects, state mutations, event ordering, idempotency, external calls.
> - Non-functional constraints: performance characteristics visible in code or tests ("must stream", "must not block", O(n) memory), concurrency model, error-handling protocol.
> - Implicit invariants: patterns the tests imply even if undocumented (which error type is thrown on which input, ordering assumptions, retry semantics).
>
> Be specific. "Handles large inputs" is useless. "Streams to stdout without buffering the whole response, per `tests/streaming_test.py:42`" is usable. Cite `file:line` wherever possible.
>
> Return a structured contract in this shape:
>
> ```yaml
> public_api:
>   - name: <fn or export>
>     signature: <sig>
>     raises: [<error type>, ...]
>     returns: <description>
> observable_behaviour:
>   - <bullet, cite file:line>
> non_functional:
>   - <bullet, cite file:line>
> implicit_invariants:
>   - <bullet, cite test file:line>
> ```
>
> If a section has no entries, return the key with an empty list. Do not invent entries to fill a section.

## Rules for the orchestrator

- Skipping step 4 breaks the validator's ground truth — never skip, even for tiny scopes.
- The contract is passed to every spoke (step 5) and every validator (step 6). If it is empty or generic, spokes produce noise and validators rubber-stamp it. Re-run with better context rather than proceeding.
- Model is `sonnet`, explicitly. Do not inherit from the orchestrator.
