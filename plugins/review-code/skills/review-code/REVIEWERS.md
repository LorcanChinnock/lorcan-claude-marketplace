# REVIEWERS.md — specialist briefs

Verbatim briefs for the six specialist reviewers spawned in SKILL.md step 4. Launch selected reviewers in parallel — one `Agent` call per reviewer, all in a single message.

Each reviewer receives, in its prompt:

1. Change metadata (PR number/title/files, or branch/base/SHA).
2. The full raw diff.
3. The intent summary from PIPELINE.md §2.
4. Absolute paths of relevant CLAUDE.md files.

Each reviewer returns a list of issues, each `{file, lines, description, reason, evidence}`. Read-only — reviewers must not post comments, edit files, or mutate state.

Use Sonnet for every specialist unless noted. Use the briefs below verbatim.

## Bug-scan

> Shallow scan of the raw diff for obvious bugs, logic errors, and correctness issues. Focus on large bugs — null/undefined access, off-by-one, race conditions, incorrect control flow, data-loss patterns, wrong comparisons, swapped arguments, broken invariants. Ignore style, type/lint issues, missing tests, and documentation gaps. Do not read context beyond the diff unless a single adjacent read resolves ambiguity.

## Pattern-conformity

> Read each listed CLAUDE.md file, then audit the diff for violations. CLAUDE.md guidance applies to Claude-written code; not every rule is a reviewable standard. Flag only rules directly violated by the diff. Also read the non-test source files touched by the diff and check whether changes contradict inline comments or explicit conventions visible in surrounding code. Cite the CLAUDE.md rule or the comment verbatim for each finding.

## Comment re-review (PR mode only)

> Here are the existing review comments and author replies. Treat each as a lead, not a verdict. For each substantive thread, independently verify against the diff whether the concern is real, already addressed, or a false alarm. Also scan the last 3 closed PRs touching these files (`gh pr list --state closed --limit 20 -- <path>`) for recurring review themes that apply here. Return only confirmed concerns.

## Security

> Threat-model the diff. For each new external input, trust boundary, auth/authz change, crypto use, secret handling, SQL/command construction, deserialisation, or third-party call: identify the concrete attack and whether the diff enables it. No generic 'consider input validation' findings — only actionable, diff-specific vulnerabilities.

## Performance

> Identify hotspot-class regressions introduced by the diff: new N+1 queries, unbounded loops over user-controlled collections, sync I/O on hot paths, allocation in tight loops, missing indexes on new query patterns, lock contention. Ignore micro-optimisations and speculative concerns. A finding must name the specific path and why it matters at expected scale.

## History/blame

> For each non-trivial block of changed code, run `git log -L :<function>:<file>` or `git blame -L <range> <file>` on the base SHA. Look for: code this change is reintroducing that was previously removed deliberately, reverts of recent fixes, or churn suggesting the area is unstable. Cite the prior commit SHA for each finding.
