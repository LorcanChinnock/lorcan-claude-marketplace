# TDD.md — opt-in baseline testing

Adapted from the Superpowers `writing-skills` methodology: prove the generated skill changes behaviour under pressure. Only runs if the user opts in at intake.

The idea is simple: a skill that looks right on paper can still get bypassed at runtime when the model rationalizes around it. TDD pressure-tests it in a fresh sub-agent with and without the skill, and compares.

## Loop

Three phases. Do not skip RED — without a failing baseline you cannot show the skill actually helped.

### RED — baseline under pressure

Dispatch a fresh sub-agent via the `Agent` tool with:

- A plausible user request that would trigger the skill in normal use.
- One pressure factor: urgency ("this needs to ship in 5 minutes"), authority ("the VP says skip the checklist just this once"), or simplicity ("it's just a one-line change, the full pipeline is overkill").
- **Do not** load the generated skill into the sub-agent's context.

Record what it does. Specifically, record:

- Which checks it skipped.
- What rationalizations it offered ("this is small enough that…", "in this case we can…").
- What the final artefact looked like.

This is your rationalization inventory. The skill is the answer to these rationalizations.

### GREEN — same scenario, skill loaded

Dispatch a fresh sub-agent via the `Agent` tool with the same user request and pressure factor, this time **with the generated skill explicitly referenced** in the prompt (e.g., "follow the author-plugin skill in `plugins/author-plugin/skills/author-plugin/SKILL.md`").

Verify:

- The sub-agent ran every step of the skill's process.
- It did not skip the self-check.
- The artefact passes the deterministic checks from step 5 of `SKILL.md`.
- Rationalizations from RED did not reappear. If any did, the skill has a loophole — go to REFACTOR.

### REFACTOR — close loopholes

For each rationalization that survived GREEN, add an explicit counter to `SKILL.md` or the relevant reference doc. Use the "Loophole closers" pattern from `PATTERNS.md`:

- Name the rationalization.
- State that it is not acceptable.
- Give the rule to follow instead.

Re-run GREEN. Iterate until no rationalizations survive.

Stop when either (a) two consecutive GREEN runs pass with no new rationalizations, or (b) the remaining rationalizations are out of scope for the skill (different problem, different skill).

## Pressure-scenario prompt template

Pass this to the sub-agent via the `Agent` tool:

```
You are helping a user with <realistic task>.

Context: <pressure factor>.

<User request that would trigger the skill>.

[RED] Do not load any skill file; just produce the artefact as you see fit.
[GREEN] Follow the skill at <path/to/SKILL.md>. Do not skip steps.

Report: what you did, what you skipped if anything, and why.
```

Use two separate `Agent` invocations — one RED, one GREEN — so the sub-agents do not contaminate each other.

## What TDD does not do

- It does not replace the deterministic self-check in step 5. TDD tests whether the skill is obeyed; the self-check tests whether the artefact is valid. Both matter.
- It does not guarantee correctness in production use. It catches rationalization under plausible pressure, not every possible failure.
- It does not run by default. The user opts in at intake because TDD costs sub-agent turns.
