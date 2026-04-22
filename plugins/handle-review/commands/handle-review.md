---
description: Evaluate code review or other critical feedback with technical rigor, then implement or push back.
---

Load the `handle-review` skill and apply its process to the feedback in this conversation.

Arguments (optional):

- `<pr-number>` or `<pr-url>`: if present, pull inline comments, top-level comments, and the raw diff using the read-only commands in `GH.md` before applying the skill.
- Otherwise treat pasted text, a cited file, or prior conversation turns as the feedback set.

Hard rules:

- No performative agreement. Apply the forbidden-phrase list from `PATTERNS.md`.
- Verify each item against the codebase before implementing.
- Push back with technical reasoning when a suggestion is wrong.
- One item at a time, tested.
- Never post replies, merge, approve, or request changes via `gh`.
