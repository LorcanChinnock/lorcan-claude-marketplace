# PIPELINE.md — resolve, gate, summarise, tier

Operational detail for SKILL.md steps 1–3. Keep this file next to SKILL.md and read on entry.

## §1 — Resolve inputs and raw diff

### PR mode

Run in parallel:

```bash
gh pr view <PR> --json number,title,state,isDraft,author,baseRefName,headRefName,headRefOid,baseRefOid,url,body,files,additions,deletions,changedFiles,labels
gh pr diff <PR>                              # raw unified diff, full, no truncation
gh pr view <PR> --json comments,reviews      # existing discussion
```

Record: `headRefOid` (full 40-char SHA — required for permalinks), `baseRefName`, repo `owner/name`, `additions + deletions`, `changedFiles`.

### Local mode

Run in parallel:

```bash
git rev-parse --show-toplevel
git rev-parse HEAD                           # full SHA for permalinks
git symbolic-ref --short refs/remotes/origin/HEAD 2>/dev/null || echo "origin/main"
git rev-parse --abbrev-ref HEAD              # current branch
git config --get remote.origin.url           # owner/repo for permalinks
git status --porcelain                       # note uncommitted; do not review them
```

Then determine base: strip `origin/` prefix from the default ref; fall back in order to `main`, `master`, `develop`. Run:

```bash
git merge-base <base> HEAD
git diff <merge-base>...HEAD                 # raw diff, full, no truncation
git diff <merge-base>...HEAD --stat          # for size gating
git diff <merge-base>...HEAD --name-only     # for CLAUDE.md location
```

If `git status --porcelain` is non-empty, note in the report header that uncommitted changes exist and were not reviewed. Permalinks require a commit — do not attempt to review uncommitted work.

Parse `remote.origin.url` into `owner/repo`. Support both `git@github.com:owner/repo.git` and `https://github.com/owner/repo.git`. If the remote is not GitHub, drop permalinks and use `<path>:<line>` citations instead.

### Auto-detect (`current` or no arg)

Run `gh pr view --json number` on the current branch. If it returns a PR, use PR mode. Otherwise local mode.

### Halt conditions

- `gh` not authenticated while in PR mode → stop and report.
- Not inside a git repo while in local mode → stop and report.
- Do not fall back to speculation.

## §2 — Three Haiku pre-steps in parallel

Spawn three `Agent` calls in a single message. All use Haiku for cost.

### Eligibility gate

Given the change metadata, return `proceed` or `skip: <reason>`. Skip if:

- **PR mode**: `state != OPEN`, `isDraft == true`, author is a bot (`[bot]` suffix), or labels + diff indicate pure automation (`dependabot`, `renovate`, `automated` on lockfile-only changes).
- **Either mode**: diff is trivially safe — under ~10 lines of README/comment-only change, version bump, typo fix.

If `skip`, stop and report the reason. Do not run reviewers.

### Locate CLAUDE.md

Given the list of changed file paths, return absolute paths (not contents) of:

- Root `CLAUDE.md` of the repo.
- Any `CLAUDE.md` in ancestor directories of each changed file.

Downstream reviewers and the scorer receive these paths. Do not read contents here.

### Summarise intent

Given title + body (PR mode) or branch name + `git log --format=%s <base>..HEAD` (local mode), plus the raw diff, return:

- **Goal**: one sentence — what is this change trying to achieve.
- **Design shape**: 2–4 bullets — the approach (new module, refactor pattern, behaviour change).
- **Scope**: affected subsystems.
- **Risk surface**: 1–3 bullets — what could plausibly break.

Pass this summary to every downstream reviewer so they review against intent, not a vacuum.

## §3 — Size and complexity tier

| Tier | Signals | Reviewers |
|---|---|---|
| Trivial | ≤50 LOC, 1–2 files, no risk surface | Bug-scan only |
| Small | ≤200 LOC, localised | Bug-scan, Pattern-conformity |
| Medium | ≤1000 LOC, or touches a risk area | Bug-scan, Pattern-conformity, Comment re-review *(PR mode only)*, + Security OR Performance if risk surface calls for it |
| Large | >1000 LOC, or touches auth / data / concurrency / infra | All: Bug-scan, Pattern-conformity, Comment re-review *(PR mode only)*, Security, Performance, History/blame |

Err on fewer reviewers. Escalate only if findings warrant. State the chosen tier and a one-line rationale before dispatch.

## §4 — Orchestrator guardrails

- Parallelise every step that can be parallel. Sequential dispatch when parallel is possible is a bug.
- Between steps, give one short status line to the user. Not a narration of every tool call.
- Never mutate state: no `git commit`, no `gh pr comment`, no file edits outside an explicit user-requested save.
- If a reviewer returns malformed output, re-prompt it once. If still malformed, drop its output and note it in the report.
- If total reviewer findings exceed 30, tighten scoring and report the anomaly — reviewers are probably nit-picking.
- Do not default to agreement with existing PR comments. If you disagree after verification, say so.
