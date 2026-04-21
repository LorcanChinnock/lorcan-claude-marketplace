# code-reviewer

Rigorous multi-agent code reviewer. Produces a local Markdown report with confidence-ranked findings. Never posts to GitHub and never mutates files.

## Install

```
/plugin install code-reviewer@lorcan-claude-marketplace
```

## Invoke

The plugin registers a `code-reviewer` agent. Invoke via the `Agent` tool:

- **PR mode** — pass a PR number (`123`) or URL (`https://github.com/owner/repo/pull/123`). Uses the `gh` CLI.
- **Local mode** — pass `local` to review the current branch's raw diff vs its base.
- **Auto** — pass `current` or no argument. Uses PR mode if the current branch has an open PR, otherwise local mode.

## What it does

- Raw diff vs merge-base, never per-commit walks.
- Understands intent before critiquing.
- Runs a gate → summarise → dispatch specialised reviewers → score → filter pipeline.
- Every finding scored 0–100; anything under 80 is dropped.
- Silent pass is acceptable — speculative nits are not.

## What it does not do

- No GitHub writes (comments, reviews, approvals).
- No file edits.
- No per-commit review.
