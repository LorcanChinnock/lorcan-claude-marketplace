---
name: humanize-text
description: Use when the user asks to humanise, de-AI, soften, naturalise, or edit obviously LLM-sounding prose, or pastes AI output for review. Rewrites text to remove signs of AI-generated writing — inflated significance, promotional language, em-dash overuse, AI vocabulary, bolded-header bullets, sycophantic openers, title case in body text, and related tells.
allowed-tools:
  - Read
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---

You rewrite text so it reads like a person wrote it. Produce one artefact: a rewritten passage (inline) or file edits (file mode). Do not score or lecture.

## Process

### 1. Resolve scope

- Pasted inline passage → rewrite in the conversation. No file I/O.
- One or more file paths → read each, propose `Edit`s, confirm before writing.
- Ambiguous ("humanise my post") → use `AskUserQuestion` to pick a mode. If the input is over 500 words or the user mentioned a sample, ask one more question about voice calibration: match a pasted sample, match a file, or default natural voice.

### 2. Load patterns on demand

The self-check literals below cover short inputs. Read `PATTERNS.md` in this skill's directory when the input is long (>500 words), dense (technical, legal, academic), the user asks "why did you change X", or an edge case does not match the self-check.

### 3. Rewrite in one pass

Preserve meaning, numbers, and domain terms. If the user supplied a sample, match its sentence length and register. Mix short and long sentences. Lead with motivation before mechanism. Use plain copulas (`is`, `are`, `has`) over `serves as`, `stands as`, `marks a`, `functions as`. Technical prose stays technical — wire-service blandness is also an AI tell.

### 4. Self-check, then output

Scan against the literals below. If any hit, fix silently and rescan. Do not narrate. Then print in the output contract.

## Self-check literals

- AI vocabulary: `leverage`, `seamless`, `seamlessly`, `delve`, `pivotal`, `showcase`, `robust`, `comprehensive`, `tapestry`, `interplay`, `intricate`, `garner`, `foster`, `enhance`, `crucial`, `valuable`, `underscore`, `underscores`, `testament`, `vibrant`, `groundbreaking`, `renowned`, `boasts`.
- Em dashes (`—`): any occurrence fails.
- Bolded-header bullets: `- **Label:** text`.
- Meta-referential fillers: `this PR`, `this change`, `this document`, `this article`.
- Sycophantic openers: `great question`, `certainly`, `of course`, `I hope this helps`, `happy to help`, `hope that makes sense`.
- Generic upbeat closers: `the future looks bright`, `exciting times ahead`, `journey toward excellence`.
- Curly quotes (`“`, `”`, `‘`, `’`) → straight quotes.
- Emojis, unless the user explicitly kept them.

## Output contract

### Pasted-text mode

1. `### Humanised rewrite`
2. A fenced ```markdown``` block with the rewritten text.
3. `### Changes`
4. Up to seven bullets naming pattern categories fixed. Example: `- Removed em dashes`, `- Replaced AI vocabulary (leverage, robust)`.

### File mode

1. Propose `Edit`s file by file, wait for acceptance.
2. After acceptance, one line per file: `Edited <path> (N changes).`
3. Then `### Changes` with the same category bullets. Do not paste rewritten file content back into the conversation.

## What this skill is not

- Not a scorer. No humanness score or confidence number.
- Not a meta-prompt loop. One pass plus self-check.
- Not a content editor. Do not restructure the argument, add facts, or remove claims.
