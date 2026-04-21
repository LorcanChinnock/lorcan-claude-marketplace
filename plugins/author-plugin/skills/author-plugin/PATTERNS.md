# PATTERNS.md — writing-quality rules

Style rules for plugin content. Use these during step 4 (generate) and step 5 (self-check).

## Description-as-trigger

The `description` of a skill or subagent is how Claude decides whether to invoke it. It is an index entry, not a summary of the workflow.

**Shape**

> Use when `<third-person triggers>`. Returns `<what the user gets>`.

**Third-person**, because Claude reads these while deciding which capability matches the user's current request.

**Banned phrases** (flag in self-check):
- `dispatches`
- `coordinates`
- `between tasks`
- `orchestrates`
- `then X, then Y, then Z`
- `first I will … then …`

These describe the *inside* of the skill, not its *outside*. They waste tokens and do not help trigger matching.

**Before**

> `description: This skill orchestrates the plugin creation workflow. First it asks questions, then it dispatches sub-agents, then it coordinates the file writes between tasks.`

**After**

> `description: Use when creating a new Claude Code plugin from a plain-English requirement, or when a generated plugin needs review. Scaffolds skills, subagents, hooks, MCP servers, and reference docs.`

## Token budgets

| File | Budget | Why |
|---|---|---|
| `SKILL.md` | **< 500 words** | Loaded every time the skill fires |
| Frequently-loaded reference (`PATTERNS.md` loaded on every rewrite) | **< 200 words** per logical section | Progressive disclosure only works if the hot path is small |
| Heavy reference (loaded on specific branches) | may exceed | Link from SKILL.md; load on demand |
| Agent description | under ~400 chars | Entered into trigger matching |

If SKILL.md is over budget, cut narration. Workflow steps are load-bearing; adjectives are not.

## Naming

- **Verb-first kebab-case**: `author-plugin`, `review-code`, `format-dates`, `pr-description`. Not `plugin-authoring`, `code-review`, `date-formatter`.
- Folders lowercase, hyphenated.
- Uppercase `.md` only for reference docs (`PATTERNS.md`, `SURFACES.md`, `TEMPLATES.md`, `TDD.md`, `REFERENCE.md`). `SKILL.md` and `README.md` follow that convention too.
- Match `^[a-z][a-z0-9-]*$`. No camelCase, no underscores, no leading digits.

## Cross-referencing

Link, don't duplicate. If a rule appears in `PATTERNS.md`, SKILL.md says "apply `PATTERNS.md`" rather than restating the rule.

**Never** use `@PATTERNS.md` links. The `@` prefix force-loads the file into the current turn, defeating progressive disclosure. Plain markdown links (`[PATTERNS.md](PATTERNS.md)`) load the file only when Claude chooses to read it.

## Loophole closers (for skills that enforce discipline)

If the generated skill enforces a constraint (don't commit, don't push, don't auto-revert), close the common evasions explicitly in the skill body:

- "Just this once, I can skip the self-check." → "Self-check is not optional. If you are tempted to skip, run it anyway."
- "This is a simple addition, the full pipeline is overkill." → "Every run uses the full pipeline. Small changes are cheap; skipping steps is how the safety rail gets lost."
- "The user didn't say I couldn't." → "Default-deny for anything mutating: commits, pushes, remote writes. User has to ask in the turn."

Without an explicit counter, plausible-sounding rationalizations win under pressure. Name them; close them.

## House style (lifted from the existing marketplace plugins)

- No emojis unless the user explicitly asks.
- Direct tone. No sycophancy, no "great question!", no "happy to help".
- Plain copulas (`is`, `are`, `has`), not `serves as` / `stands as` / `functions as`.
- Sentence case in headings. Title case in body text is an AI-writing tell.
- One finding per bullet. No bolded-header bullets like `- **Label:** text`.
- Em dashes are a tell — use commas, periods, or parentheses. (This instruction itself has one; that is the house style for hand-written prose, not generated skill content. In generated content, avoid.)
