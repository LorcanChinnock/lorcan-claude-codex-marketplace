# HUMANIZE.md: scrub these before printing

Reference for the write-doc skill. The point is to remove the obvious tells of LLM-generated prose.

## How to run the scan

Run the literal scan twice: once before writing each section, once on the full draft. If the second scan finds anything, run a third. Fix silently between passes; do not narrate.

For drafts over 500 words, use a Humanize skill if one is installed

## Self-check literals

- **AI vocabulary**: leverage, seamless, seamlessly, delve, pivotal, showcase, robust, comprehensive, tapestry, interplay, intricate, garner, foster, enhance, crucial, valuable, underscore, underscores, testament, vibrant, groundbreaking, renowned, boasts.
- **Em dashes** (`—`): any occurrence fails. Replace with a comma, period, colon, or parentheses; restructure if none fit.
- **Bolded-header bullets**: `- **Label:** text`. Either drop the bold and rephrase, or move to a small table or subsection.
- **Sycophantic openers**: "great question", "certainly", "of course", "happy to help", "hope that makes sense".
- **Generic upbeat closers**: "the future looks bright", "exciting times ahead", "journey toward excellence".
- **Curly quotes** (`“ ” ‘ ’`): straight quotes only.
- **Decorative emojis** on headings or bullets. Drop them.
- **Title case in body text** ("A Pivotal Moment"): use sentence case.

## Patterns that read AI

- **Significance inflation**: "marks a pivotal moment", "stands as a testament", "underscores the importance of". Cut. Let the fact speak.
- **Promotional language**: "vibrant", "rich", "groundbreaking", "nestled", "in the heart of", "must-visit". Cut.
- **Copula avoidance**: "serves as", "stands as", "represents", "functions as". Replace with `is`, `are`, `has`.
- **Forced rule of three**: triplets that pad rather than enumerate ("innovation, inspiration, and industry insights"). Keep one or two real items.
- **Trailing -ing clauses**: "highlighting the importance of", "ensuring smooth operation". Cut, or split into a real sentence.
- **Negative parallelism**: "It's not just X, it's Y." Pick the actual claim and state it.
- **Vague attribution**: "experts say", "industry reports". Either name the source or drop the claim.
- **Persuasive authority tropes**: "the real question is", "at its core", "fundamentally". Cut.
- **Signposting**: "let's dive in", "here's what you need to know". Cut.
- **Fragmented headers**: H2 followed by a one-line restatement before real content. Delete the restatement.
- **Filler phrases**: "in order to" → "to"; "due to the fact that" → "because"; "at this point in time" → "now"; "has the ability to" → "can".

## Voice

- Lead with motivation before mechanism.
- Mix short and long sentences.
- Plain copulas (`is`, `are`, `has`) over decorative verbs.
- Technical prose stays technical. Wire-service blandness is also an AI tell, so do not strip every distinctive verb.

## What humanising does not mean

- It does not mean removing facts to sound casual.
- It does not mean adding folksy asides.
- It does not mean rewriting the structure. The doc still follows the template.
- It does not mean dropping precision. Numbers, version strings, command flags stay exact.
