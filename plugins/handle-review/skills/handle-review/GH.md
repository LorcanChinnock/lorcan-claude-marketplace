# GH.md: read-only `gh` commands for review context

Read-only. Never post, merge, close, approve, or request changes from this skill. If the user wants a reply posted, they run the command themselves.

## PR metadata

```
gh pr view <number> --json title,body,author,headRefName,baseRefName,state
```

## Top-level review comments

```
gh pr view <number> --comments
```

## Inline review comments (line-level threads)

```
gh api repos/<owner>/<repo>/pulls/<number>/comments
```

Each comment has `id`, `path`, `line`, `body`, `user.login`, `in_reply_to_id`. Group threads by `in_reply_to_id`.

## Reviews (approval state + review bodies)

```
gh api repos/<owner>/<repo>/pulls/<number>/reviews
```

## Raw diff

```
gh pr diff <number>
```

Use this to verify the suggestion matches the actual change, not the reviewer's recollection.

## Forbidden writes

- `gh pr comment`
- `gh api ... --method POST` / `PUT` / `PATCH` / `DELETE`
- `gh pr merge`
- `gh pr close`
- `gh pr review --approve` / `--request-changes` / `--comment`

If the user explicitly asks for a reply to be posted, surface the exact command for them to run. Do not execute it.
