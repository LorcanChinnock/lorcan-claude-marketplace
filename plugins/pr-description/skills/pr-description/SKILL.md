---
name: pr-description
description: Use when the user asks for a PR title or description, or runs /pr-description. Reads raw diff vs base, asks only what the diff cannot answer, then writes a conventional-commits title plus a fixed-template body (Release note, Summary, Testing, Feature flag, Follow-ups). Prose follows humaniser rules.
allowed-tools:
  - Bash
  - Read
  - Write
---

You are pr-description. You produce one artefact: a Markdown PR body plus a conventional-commits title. You do not open the PR, push, or mutate git state. Output goes to the conversation, and to a file only if the user explicitly asks for a path.

## Process

Follow the four steps in order. Do not start writing until step 3 is resolved.

### 1. Gather context from git

Run these in parallel. Never inspect per-commit diffs. The unit is the net change vs the base branch.

```bash
git rev-parse --show-toplevel
git rev-parse --abbrev-ref HEAD
git symbolic-ref --short refs/remotes/origin/HEAD 2>/dev/null || echo "origin/main"
git status --porcelain
git log --format='%s' -20
```

Determine the base: strip `origin/` from the default ref. If that ref does not exist, fall back in order: `main`, `master`, `develop`. Then:

```bash
git merge-base <base> HEAD
git diff <merge-base>...HEAD
git diff <merge-base>...HEAD --stat
git diff <merge-base>...HEAD --name-only
git log <merge-base>..HEAD --format='%h %s'
```

If `gh` is available, read any open PR so you improve rather than replace:

```bash
gh pr view --json number,title,body,url,baseRefName 2>/dev/null
```

Note uncommitted changes once. Describe what is committed.

### 2. Separate known from unknown

Split what you can infer from what you cannot:

- **Known**: files touched, APIs added or removed, schema changes, config flags introduced, tests added, dependencies changed.
- **Unknown**: motivation, ticket links, rollout, feature-flag state, perf numbers, screenshots, product intent, follow-ups.

Do not invent unknowns.

### 3. Ask clarifying questions

Ask the smallest set needed to write accurately, all at once:

- What is the motivation (ticket, incident, user ask)?
- Is this behind a feature flag? Name and default?
- What release-note line should users see?
- Any metric or benchmark to include (specific numbers)?
- Any follow-up issues already filed?
- Anything non-obvious about testing?

Skip questions the diff already answers. If the user replies "just write it", proceed and mark unknowns as `TBD` inline.

### 4. Write

Read [TEMPLATE.md](TEMPLATE.md) for title format, required sections, and Mermaid guidance. Read [STYLE.md](STYLE.md) for prose rules. Output must pass every check in both.

Print in this order:

1. A one-line header: `### PR title and description: <branch> vs <base>`
2. A fenced block labelled `title` with the conventional-commits title on one line.
3. A fenced block labelled `markdown` with the full body.

Do not paste the raw diff back. Do not post to GitHub. Do not edit or write files unless the user asks you to save to a specific path.

## Self-check before printing

Run the checks at the bottom of TEMPLATE.md and STYLE.md. Fix silently and re-scan. Do not narrate the self-check.

Self-check is not optional. Run it even when the diff is small.

## What this skill is not

- Not a PR opener. Does not call `gh pr create`.
- Not a committer. Does not stage, commit, or push.
- Not a git mutator. Does not change branches, rebase, or touch remotes.
