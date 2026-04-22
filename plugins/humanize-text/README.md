# humanize-text

Rewrite text so it stops sounding AI-generated. Catches inflated significance, promotional language, em-dash overuse, AI vocabulary, bolded-header bullets, and the other patterns that give LLM prose away.

## Install

```
/plugin install humanize-text@lorcan-claude-marketplace
```

## Invoke

```
/humanize-text
```

Paste the text inline, or point at a file path. The skill asks what scope it should work on if the request is ambiguous.

## What it does

- Scans input against a 31-pattern catalogue based on Wikipedia's "Signs of AI writing" guide.
- Rewrites the prose in a single pass, preserving meaning and any domain terms you flagged.
- Runs a deterministic self-check for the highest-offender patterns (em dashes, banned vocabulary, bolded-header bullets, curly quotes, sycophantic openers) and fixes silently before printing.
- Can calibrate to a writing sample you supply, for longer inputs.

## What it does not do

- Does not invent facts or prune technical content into blandness.
- Does not write to disk unless you point it at a file path and approve the edit.
- Does not post, push, or otherwise mutate anything outside the conversation.
