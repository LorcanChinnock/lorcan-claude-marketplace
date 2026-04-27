# STYLE.md — prose rules

Read this on step 4 before writing. Every sentence in the body is read by humans, it must not sound AI-generated.

## Rewrite triggers

Rewrite anything matching these patterns:

- Significance inflation: remove "testament to", "pivotal moment", "underscores", "highlights", "reflects broader", "marks a shift", "shaping the", "evolving landscape".
- Promotional language: remove "boasts", "vibrant", "groundbreaking", "renowned", "breathtaking", "nestled", "in the heart of".
- Superficial -ing phrases: cut trailing participial clauses added for fake depth, for example "ensuring X", "reflecting Y", "showcasing Z", "contributing to W".
- AI vocabulary: replace "leverage", "seamlessly", "delve", "pivotal", "showcase", "robust", "comprehensive", "tapestry", "interplay", "intricate", "garner", "foster", "enhance", "crucial", "key" (as adjective), "valuable".
- Vague attribution: replace "experts argue", "observers have noted", "industry reports suggest" with specific sources, or remove.
- Negative parallelism: cut "not just X, it's Y" and "not merely X, it's Y".
- Rule of three: if three items are forced to sound comprehensive, cut to what is actually needed.
- Copula avoidance: replace "serves as", "stands as", "functions as", "marks a" with plain "is" or "are".
- Em dashes: replace `—` with a comma, or restructure. No em dashes anywhere in the output.
- Bolded-header bullets: never write `- **Label:** text`. Use plain prose or plain bullets.
- Filler phrases: "in order to" → "to", "due to the fact that" → "because", "at this point in time" → "now", "has the ability to" → "can".
- Excessive hedging: cut "could potentially possibly", "it might be argued that".
- Generic upbeat endings: cut "the future looks bright", "exciting times ahead", "journey toward excellence".
- Sycophantic openers: cut "great question", "certainly", "of course", "I hope this helps".

## Additional style rules

- Lead with why, not what. Motivation first, mechanics second. The diff already shows mechanics — prose that restates them is wasted.
- Dropped-subject active voice in narrative sections. Summary and Testing read as a teammate's spoken brief with the "I" left off. "Added X because Y." Not "I added X because Y", not "the author", "the developer", "the PR author", and not passive voice ("X was added"). Release note and headings stay neutral.
- Write like a teammate explaining at a desk, not a press release. If a sentence would feel wrong said out loud to a colleague, rewrite it.
- Every sentence earns its place. If a heading or the diff already says it, do not repeat it in prose.
- Specific over vague. "p95 dropped from 340ms to 90ms" beats "improves performance".
- Short sentences. Vary length.
- Sentence case in prose. Keep template headings exactly as written.
- Do not write "I", "this PR", "this change", "the author", or "the developer". State what changed and why.
- No emojis.

## Style checks

Before printing, scan the full output for:

- Zero em dashes.
- Zero banned AI vocabulary: leverage, seamless, seamlessly, delve, pivotal, showcase, robust, comprehensive, tapestry, interplay, intricate, garner, foster, enhance, crucial, key (as adjective), valuable, underscore, underscores, testament, vibrant, groundbreaking, renowned, boasts.
- Zero bolded-header bullets (`- **Label:** text`).
- Zero "I", "this PR", "this change", "the author", or "the developer".
- Summary and Testing use dropped-subject active voice. No first-person pronouns, no third-person references to whoever wrote the change, no passive voice ("X was added").
- Motivation appears before mechanism in the lead sentence and in Summary. The Summary's first sentence is a why-statement, not a what-statement.
- No sentence that would feel wrong said aloud to a teammate.
