# handle-review

Evaluate code review or other critical feedback with technical rigor. Verify, push back when wrong, implement one item at a time. No performative agreement.

## Install

```
/plugin install handle-review@lorcan-claude-marketplace
```

## Invoke

```
/handle-review [<pr-number-or-url>]
```

The skill also triggers conversationally when you paste review feedback or ask Claude to address review comments.

## What it does

- Restates each item, verifies against the codebase, decides correct / wrong / needs-context.
- Pushes back with technical reasoning on wrong suggestions (breaks functionality, YAGNI, context gap, stack mismatch, architectural conflict).
- Implements correct fixes one at a time, tests each.
- Acknowledges in the code, not in prose. Forbids `absolutely right`, `great point`, `thanks for`, and similar.
- Ships read-only `gh` helpers for pulling PR context (inline comments, reviews, raw diff).

## What it does not do

- Does not post GitHub comments, merge, approve, or request changes.
- Does not commit or push.
- Does not batch fixes without testing.
- Does not guess on unclear items; surfaces them first.
