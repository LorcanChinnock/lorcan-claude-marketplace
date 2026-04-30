# humanize

Rewrite text and code so they stop sounding AI-generated and read clearly for a non-native English reader. Two skills: `humanize-text` for prose, `humanize-code` for code.

## Install

```
/plugin install humanize@lorcan-claude-marketplace
```

## Skills

### `humanize-text` — prose

Invoke:

```
/humanize-text
```

Paste the text inline, or point at a file path. The skill asks what scope it should work on if the request is ambiguous.

What it does:

- Scans input against a 31-pattern catalogue based on Wikipedia's "Signs of AI writing" guide.
- Rewrites the prose in a single pass, preserving meaning and any domain terms you flagged.
- Runs a deterministic self-check for the highest-offender patterns (em dashes, banned vocabulary, bolded-header bullets, curly quotes, sycophantic openers) and fixes silently before printing.
- Can calibrate to a writing sample you supply, for longer inputs.

### `humanize-code` — code

Invoke:

```
/humanize-code
```

Point it at a file, set of files, a symbol to rename across the repo, a git diff, or paste a snippet inline.

What it does:

- Renames verbose or fancy identifiers (variables, parameters, functions, methods, classes, types) into short, concrete names a non-native English reader can follow. Drops filler nouns (`Manager`, `Handler`, `Helper`), type echoes (`userList`, `isEnabledFlag`), and Latinate verbs (`utilise`, `instantiate`).
- Rewrites comments and docstrings into plain active sentences. Cuts comments that restate the code; keeps comments that explain why.
- Simplifies log, error, and user-facing strings so they say what failed and to what, without empty politeness or AI vocabulary.
- Keeps very well-known technical terms (`url`, `json`, `regex`, `auth`, `cache`, language and framework names) where they read more clearly than spelled-out alternatives.
- In normal mode, edits in place file by file. In plan mode, returns a report (proposed renames, before/after comment and string rewrites, and any risks like public API hits).

## What neither skill does

- Does not invent facts, prune technical content into blandness, or change code behaviour.
- Does not write to disk unless you point it at a file path and approve the edit.
- Does not post, push, or otherwise mutate anything outside the conversation.
