# Humanizer pattern catalogue

Reference for the humanizer skill. Read this when the compressed table in `SKILL.md` is not enough: long inputs, edge cases, or when the user asks why a specific change was made.

Patterns are grouped by type. Numbering matches the compressed catalogue in `SKILL.md`.

## Content patterns

### 1. Significance inflation

**Trigger words:** stands/serves as, is a testament/reminder, a vital/significant/crucial/pivotal/key role/moment, underscores/highlights its importance, reflects broader, symbolising its ongoing/enduring/lasting, contributing to, setting the stage for, marking/shaping the, represents/marks a shift, key turning point, evolving landscape, focal point, indelible mark, deeply rooted.

**Why it reads AI:** LLMs puff up importance by telling the reader something is important instead of letting the fact speak.

Before:
> The Statistical Institute of Catalonia was officially established in 1989, marking a pivotal moment in the evolution of regional statistics in Spain. This initiative was part of a broader movement across Spain to decentralise administrative functions and enhance regional governance.

After:
> The Statistical Institute of Catalonia was established in 1989 to collect and publish regional statistics independently from Spain's national statistics office.

### 2. Notability padding

**Trigger shape:** lists of media outlets that covered something, or "active social media presence with over X followers", with no quote or claim attached.

Before:
> Her views have been cited in The New York Times, BBC, Financial Times, and The Hindu. She maintains an active social media presence with over 500,000 followers.

After:
> In a 2024 New York Times interview, she argued that AI regulation should focus on outcomes rather than methods.

### 3. Superficial -ing clauses

**Trigger shape:** trailing participle clauses tacked on for fake depth, such as "highlighting...", "underscoring...", "ensuring...", "reflecting...", "contributing to...", "cultivating...", "showcasing...".

Before:
> The temple's colour palette of blue, green, and gold resonates with the region's natural beauty, symbolising Texas bluebonnets, the Gulf of Mexico, and the diverse Texan landscapes, reflecting the community's deep connection to the land.

After:
> The temple uses blue, green, and gold. The architect chose these to reference local bluebonnets and the Gulf coast.

### 4. Promotional language

**Trigger words:** boasts a, vibrant, rich (figurative), profound, enhancing its, showcasing, exemplifies, commitment to, natural beauty, nestled, in the heart of, groundbreaking, renowned, breathtaking, must-visit, stunning.

Before:
> Nestled within the breathtaking region of Gonder in Ethiopia, Alamata Raya Kobo stands as a vibrant town with a rich cultural heritage and stunning natural beauty.

After:
> Alamata Raya Kobo is a town in the Gonder region of Ethiopia, known for its weekly market and 18th-century church.

### 5. Vague attribution

**Trigger shape:** "Industry reports", "Observers have cited", "Experts argue", "Some critics argue", "several sources suggest", without a specific source.

Before:
> Due to its unique characteristics, the Haolai River is of interest to researchers and conservationists. Experts believe it plays a crucial role in the regional ecosystem.

After:
> The Haolai River supports several endemic fish species, according to a 2019 survey by the Chinese Academy of Sciences.

### 6. Formulaic "Challenges" or "Legacy" sections

**Trigger shape:** "Despite its X, faces several challenges...", "Despite these challenges, continues to thrive", "Future Outlook", "Challenges and Legacy".

Before:
> Despite its industrial prosperity, Korattur faces challenges typical of urban areas, including traffic congestion and water scarcity. Despite these challenges, with its strategic location and ongoing initiatives, Korattur continues to thrive as an integral part of Chennai's growth.

After:
> Traffic congestion increased after 2015 when three new IT parks opened. The municipal corporation began a stormwater drainage project in 2022 to address recurring floods.

## Language and grammar patterns

### 7. AI vocabulary

**High-frequency offenders:** additionally, align with, crucial, delve, emphasising, enduring, enhance, fostering, garner, highlight (verb), interplay, intricate, intricacies, key (adjective), landscape (abstract), pivotal, showcase, tapestry, testament, underscore (verb), valuable, vibrant, leverage, seamless, seamlessly, robust, comprehensive, boasts.

**Replacements:** use, smooth, explore, important, show, strong, full, mix, detailed, gather, grow, improve, key, useful, emphasise, reminder. Usually the simplest fix is to cut the word entirely.

Before:
> Additionally, a distinctive feature of Somali cuisine is the incorporation of camel meat. An enduring testament to Italian colonial influence is the widespread adoption of pasta in the local culinary landscape, showcasing how these dishes have integrated into the traditional diet.

After:
> Somali cuisine also includes camel meat, which is considered a delicacy. Pasta dishes, introduced during Italian colonisation, remain common, especially in the south.

### 8. Copula avoidance

**Trigger words:** serves as, stands as, marks, represents, functions as, boasts, features, offers.

Before:
> Gallery 825 serves as LAAA's exhibition space for contemporary art. The gallery features four separate spaces and boasts over 3,000 square feet.

After:
> Gallery 825 is LAAA's exhibition space for contemporary art. The gallery has four rooms totalling 3,000 square feet.

### 9. Negative parallelism and tailing negations

**Trigger shape:** "Not only X but Y", "It's not just X, it's Y", "It's not merely X, it's Y", or a comma-attached "no guessing", "no wasted motion" on the end of a sentence.

Before:
> It's not just about the beat riding under the vocals; it's part of the aggression and atmosphere. It's not merely a song, it's a statement.

After:
> The heavy beat adds to the aggressive tone.

Before (tailing negation):
> The options come from the selected item, no guessing.

After:
> The options come from the selected item, so the user does not need to guess.

### 10. Rule of three

LLMs force ideas into triplets to sound thorough.

Before:
> The event features keynote sessions, panel discussions, and networking opportunities. Attendees can expect innovation, inspiration, and industry insights.

After:
> The event includes talks and panels, with time between sessions for informal conversation.

### 11. Elegant variation (synonym cycling)

Before:
> The protagonist faces many challenges. The main character must overcome obstacles. The central figure eventually triumphs. The hero returns home.

After:
> The protagonist faces many challenges but eventually triumphs and returns home.

### 12. False ranges

"From X to Y" constructions where X and Y are not on a scale.

Before:
> Our journey through the universe has taken us from the singularity of the Big Bang to the grand cosmic web, from the birth and death of stars to the enigmatic dance of dark matter.

After:
> The book covers the Big Bang, star formation, and current theories about dark matter.

### 13. Passive voice and subjectless fragments

Before:
> No configuration file needed. The results are preserved automatically.

After:
> You do not need a configuration file. The system preserves the results automatically.

## Style patterns

### 14. Em dash overuse

Em dashes (`—`) are not wrong. LLMs overuse them because they mimic punchy sales writing. Replace with commas, periods, or parentheses; restructure if neither fits.

Before:
> The term is primarily promoted by Dutch institutions—not by the people themselves. You don't say "Netherlands, Europe" as an address—yet this mislabelling continues—even in official documents.

After:
> The term is primarily promoted by Dutch institutions, not by the people themselves. "Netherlands, Europe" is not a real address, yet this mislabelling continues in official documents.

### 15. Mechanical boldface

Bolding acronyms or proper nouns on first mention, uniformly.

Before:
> It blends **OKRs (Objectives and Key Results)**, **KPIs (Key Performance Indicators)**, and visual strategy tools such as the **Business Model Canvas (BMC)** and **Balanced Scorecard (BSC)**.

After:
> It blends OKRs, KPIs, and visual strategy tools like the Business Model Canvas and Balanced Scorecard.

### 16. Bolded-header bullets

Before:
> - **User Experience:** The user experience has been significantly improved with a new interface.
> - **Performance:** Performance has been enhanced through optimised algorithms.
> - **Security:** Security has been strengthened with end-to-end encryption.

After:
> The update adds a new interface, speeds up load times through optimised algorithms, and adds end-to-end encryption.

### 17. Title case in headings

Before:
> ## Strategic Negotiations And Global Partnerships

After:
> ## Strategic negotiations and global partnerships

### 18. Emojis

Decorative emojis on headings or bullets are a giveaway. Leave them only if the user is writing content where they clearly belong (chat, social) and kept them deliberately.

Before:
> 🚀 **Launch Phase:** The product launches in Q3
> 💡 **Key Insight:** Users prefer simplicity
> ✅ **Next Steps:** Schedule follow-up meeting

After:
> The product launches in Q3. User research showed a preference for simplicity. Next step: schedule a follow-up meeting.

### 19. Curly quotes

ChatGPT produces curly quotes (`“ ”`). Most editorial contexts want straight quotes (`" "`). Replace.

Before:
> He said “the project is on track” but others disagreed.

After:
> He said "the project is on track" but others disagreed.

## Communication patterns

### 20. Chatbot artifacts

Before:
> Here is an overview of the French Revolution. I hope this helps! Let me know if you'd like me to expand on any section.

After:
> The French Revolution began in 1789, when financial crisis and food shortages led to widespread unrest.

### 21. Knowledge-cutoff disclaimers

Before:
> While specific details about the company's founding are not extensively documented in readily available sources, it appears to have been established sometime in the 1990s.

After:
> The company was founded in 1994, according to its registration documents.

### 22. Sycophancy

Before:
> Great question! You're absolutely right that this is a complex topic. That's an excellent point about the economic factors.

After:
> The economic factors you mentioned are relevant here.

## Filler, hedging, and framing

### 23. Filler phrases

- "In order to achieve this goal" → "To achieve this"
- "Due to the fact that it was raining" → "Because it was raining"
- "At this point in time" → "Now"
- "In the event that you need help" → "If you need help"
- "The system has the ability to process" → "The system can process"
- "It is important to note that the data shows" → "The data shows"

### 24. Excessive hedging

Before:
> It could potentially possibly be argued that the policy might have some effect on outcomes.

After:
> The policy may affect outcomes.

### 25. Generic upbeat conclusions

Before:
> The future looks bright for the company. Exciting times lie ahead as they continue their journey toward excellence. This represents a major step in the right direction.

After:
> The company plans to open two more locations next year.

### 26. Hyphenated word-pair overuse

LLMs hyphenate common modifiers uniformly ("cross-functional", "data-driven", "client-facing"). Humans are inconsistent. Drop the hyphen on everyday compound modifiers; keep it on technical or less-common ones where it genuinely disambiguates.

Before:
> The cross-functional team delivered a high-quality, data-driven report on our client-facing tools. Their decision-making process was well-known for being thorough and detail-oriented.

After:
> The cross functional team delivered a high quality, data driven report on our client facing tools. Their decision making process was known for being thorough and detail oriented.

### 27. Persuasive authority tropes

"The real question is...", "at its core...", "what really matters...", "fundamentally...", "the deeper issue...", "the heart of the matter".

Before:
> The real question is whether teams can adapt. At its core, what really matters is organisational readiness.

After:
> Whether teams can adapt depends on whether the organisation is ready to change its habits.

### 28. Signposting

"Let's dive in", "here's what you need to know", "let's explore", "without further ado".

Before:
> Let's dive into how caching works in Next.js. Here's what you need to know.

After:
> Next.js caches at several layers: request memoisation, the data cache, and the router cache.

### 29. Fragmented headers

A heading followed by a one-line sentence that restates the heading before the real content starts.

Before:
> ## Performance
>
> Speed matters.
>
> When users hit a slow page, they leave.

After:
> ## Performance
>
> When users hit a slow page, they leave.

### 30. Title case in body text

Title-cased phrases inside running prose ("A Pivotal Moment In Technology"), not just in headings. Sentence case.

Before:
> The release marks A Pivotal Moment In Technology for the team.

After:
> The release is the first time the team has shipped a public API.

### 31. Anglicised sycophancy

Milder than American "Great question!", but the same shape: cheerful closers appended to neutral prose. "Happy to help!", "Hope that makes sense!", "Does that clarify things?", "Let me know if you have any questions!"

Delete without replacement.

## Full worked example

**Before (AI-sounding):**
> Great question! Here is an essay on this topic. I hope this helps!
>
> AI-assisted coding serves as an enduring testament to the transformative potential of large language models, marking a pivotal moment in the evolution of software development. In today's rapidly evolving technological landscape, these groundbreaking tools—nestled at the intersection of research and practice—are reshaping how engineers ideate, iterate, and deliver, underscoring their vital role in modern workflows.
>
> At its core, the value proposition is clear: streamlining processes, enhancing collaboration, and fostering alignment. It's not just about autocomplete; it's about unlocking creativity at scale, ensuring that organisations can remain agile while delivering seamless, intuitive, and powerful experiences to users. The tool serves as a catalyst. The assistant functions as a partner. The system stands as a foundation for innovation.
>
> - 💡 **Speed:** Code generation is significantly faster, reducing friction and empowering developers.
> - 🚀 **Quality:** Output quality has been enhanced through improved training, contributing to higher standards.
> - ✅ **Adoption:** Usage continues to grow, reflecting broader industry trends.
>
> In conclusion, the future looks bright. Exciting times lie ahead as we continue this journey toward excellence. Let me know if you'd like me to expand on any section!

**After:**
> AI coding assistants speed up the boring parts. Not architecture. Definitely not debugging subtle bugs.
>
> They are good at boilerplate: config files, test scaffolding, repetitive refactors. They are also good at sounding right while being wrong. I have accepted suggestions that compiled, passed lint, and still missed the point because I stopped paying attention.
>
> People I talk to land in two camps. Some use it like autocomplete for chores and review every line. Others disable it after it keeps suggesting patterns they do not want. Both seem reasonable to me.
>
> The productivity numbers are slippery. GitHub says Copilot users accept 30% of suggestions, but acceptance is not correctness, and correctness is not value. If you do not have tests, you are mostly guessing.

**Changes summary (for the `### Changes` bullets):**
- Removed sycophantic openers and chatbot artifacts
- Removed significance inflation (testament, pivotal, evolving landscape)
- Removed promotional language (groundbreaking, nestled, seamless)
- Removed AI vocabulary (leverage, enhance, foster, showcase, underscore)
- Replaced copula avoidance with is/are/has
- Cut negative parallelism ("not just X, it's Y") and forced triplets
- Removed em dashes, emojis, bolded-header bullets
- Removed generic upbeat closer

## Source

This catalogue draws on [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), maintained by WikiProject AI Cleanup. The entries for title case in body text and anglicised sycophancy are additions specific to this skill.
