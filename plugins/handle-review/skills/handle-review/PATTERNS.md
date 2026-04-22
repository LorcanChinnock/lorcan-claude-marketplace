# PATTERNS.md: forbidden phrases, acknowledgement and push-back templates

Load on demand from SKILL.md. Under 200 words for the hot path.

## Forbidden acknowledgement phrases

Scan your own draft before replying. If any match, rewrite.

- `absolutely right`
- `great (point|catch|question)`
- `excellent (feedback|point|question)`
- `you are right` / `you're right` as a standalone opener
- `thanks for` (any gratitude)
- `happy to`
- `let me implement that now` before verification

## Acknowledgement templates (after verifying and fixing)

- `Fixed. <specific change> in <file:line>.`
- `<specific issue>, fixed in <file:line>.`
- `Applied. <what changed and why>.`

## Correction template (after being proven wrong on a push-back)

- `Verified this. You're correct. Fixing.`
- `Checked, the suggestion holds because <reason>. Fixing.`

## Push-back templates

YAGNI / scope:

- `Checked the caller graph, no caller for <thing>. Remove it (YAGNI)? Or is there usage I missed?`

Compatibility:

- `Build target is <X>. The suggested API lands in <Y>. Drop <X> or keep the legacy path?`

Context gap:

- `Current impl exists because <reason, file:line>. Changing it breaks <test/behavior>. What's the underlying outcome?`

Architectural conflict:

- `This contradicts <prior decision, ADR/commit>. Stopping here, want to revisit that decision?`

## Clarification template (multi-item feedback with unclear items)

- `Understood items <N, M, ...>. Need clarification on <item K>, specifically <what is ambiguous>. Holding implementation until resolved.`
