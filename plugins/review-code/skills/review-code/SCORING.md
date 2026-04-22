# SCORING.md — score, filter, output

Operational detail for SKILL.md steps 5–7.

## §rubric — Score each issue

For every issue returned in step 4, spawn a Haiku `Agent` in parallel with: the full issue, the diff, the CLAUDE.md paths. Prompt verbatim:

> Score 0–100 for whether this is a real, actionable issue on code the author modified in this change.
>
> - **0**: False positive under light scrutiny, or pre-existing issue not introduced by this change.
> - **25**: Might be real, unverified. If stylistic, not explicitly in a relevant CLAUDE.md.
> - **50**: Verified real, but a nit or rare in practice. Low importance relative to the change.
> - **75**: Verified, likely to bite in practice, or directly named in a relevant CLAUDE.md. Current approach insufficient.
> - **100**: Directly confirmed by evidence, will happen frequently.
>
> If the issue cites a CLAUDE.md rule, open the file and confirm the rule is actually there and applies. If not, score ≤25.
>
> Return `{score: <int>, rationale: <one sentence>}`.

## §filter — Drop and re-gate

- Drop every issue with `score < 80`. Non-negotiable.
- If zero survive, still produce the report ("no issues"). Do not invent findings.
- **PR mode**: re-run the eligibility gate. If the PR closed or went draft during review, abort with a note.
- **Local mode**: re-check `git rev-parse HEAD`. If HEAD moved since step 1, flag the race in the report — permalinks may now point at stale code.

## §permalinks — Strict rules

- Use the full 40-char SHA recorded in step 1 (`headRefOid` in PR mode, `git rev-parse HEAD` in local mode). Never `HEAD` or a short SHA.
- Include at least 1 line of context before and after each cited line: line 42 → `L41-L43`.
- Format: `https://github.com/<owner>/<repo>/blob/<sha>/<path>#L<start>-L<end>`.
- **Local mode caveat**: if the commit is not yet pushed, the permalink will 404 until push. Note this once in the report header when in local mode; do not annotate per-issue.
- Non-GitHub remotes: fall back to `<path>:<start>-<end>` citations.

## §output — Exact report format

Print the report to the conversation. Never call `gh pr comment`, `gh pr review`, or any mutating `gh` / `git` command. If the caller explicitly asked for a file, use `Write`; otherwise print only.

### Standard case

```
### Code review — <repo> [PR #<num>: <title> | branch <name> vs <base>]

**Intent.** <one-sentence goal from PIPELINE.md §2>
**Design shape.** <1–3 bullets>
**Reviewers run.** <list> — <one-line rationale for the chosen tier>
[**Uncommitted changes present** — not reviewed.]    <!-- local mode only, if applicable -->

Found <N> issues (confidence ≥ 80, <M> filtered):

1. [confidence: <n>] <brief description>. <Reason: CLAUDE.md / bug / security / perf / history / comment-verified>.
   Evidence: "<short quote or cite>"
   https://github.com/<owner>/<repo>/blob/<full-sha>/<path>#L<start>-L<end>

2. ...
```

### Zero-finding case

```
### Code review — <repo> [PR #<num>: <title> | branch <name> vs <base>]

**Intent.** <...>
**Reviewers run.** <list>

No issues above confidence 80. Checked <reviewers> against diff vs <base>.
```

## §false-positives — Drop, do not report

- Pre-existing issues on lines the author did not modify.
- Things a linter / typechecker / compiler catches — CI handles those.
- Missing tests / docs / general "code quality" unless CLAUDE.md names the rule.
- Style nits not named in a relevant CLAUDE.md.
- Behaviour changes that are clearly the point of the change.
- Issues on code the author edited where the concern is about an unchanged property of the surrounding function.
- Suggestions a senior engineer would not raise in review.
