# review-code

Rigorous code review for local branch diffs or GitHub PRs. Produces a local Markdown report with confidence-ranked findings and permalinks. Never posts to GitHub, never mutates files.

## Install

```
/plugin install review-code@lorcan-claude-marketplace
```

## Invoke

The plugin registers a `review-code` skill. Trigger it conversationally:

- "Review my current branch."
- "Review PR 123."
- "Review this PR: https://github.com/owner/repo/pull/123"
- "Audit the changes before I merge."

Arguments (optional):

- PR number or URL → **PR mode** (uses `gh` CLI).
- `local` → **local mode** (current branch raw diff vs its base).
- `current` or no argument → **auto-detect** (open PR ? PR mode : local mode).

## What it does

- Raw diff vs merge-base, never per-commit walks.
- Understands intent before critiquing.
- Gates trivial / bot / draft changes before spending reviewer budget.
- Dispatches specialist reviewers in parallel (bug-scan, pattern-conformity, comment re-review, security, performance, history/blame), scaled to change size.
- Scores every finding 0–100 via parallel Haiku scorers; anything under 80 is dropped.
- Silent pass is acceptable — no speculative nits.

## What it does not do

- No GitHub writes (comments, reviews, approvals).
- No file edits.
- No per-commit review.
- No automatic fixes or suggestions beyond the written report.
