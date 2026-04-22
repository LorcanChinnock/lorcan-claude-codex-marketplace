# STYLE.md — prose rules for all docgen skills

All docgen output goes through two passes: draft to the doc-type TEMPLATE, then rewrite through humanise rules. The rules below are the baseline. They are applied by invoking the `humanize-text` skill after the draft is written, plus the doc-specific rules in this file.

## Baseline humanisation

Canonical source is the `humanize-text` skill. The draft must be passed through it before the doc is considered finished. If that skill is unavailable, apply the same rules inline:

- No em dashes. Replace with commas or restructure.
- No AI vocabulary: leverage, seamlessly, delve, pivotal, showcase, robust, comprehensive, tapestry, interplay, intricate, garner, foster, enhance, crucial, key (as adjective), valuable, underscore, underscores, testament, vibrant, groundbreaking, renowned, boasts.
- No significance inflation: "testament to", "pivotal moment", "highlights", "reflects broader", "marks a shift", "shaping the", "evolving landscape".
- No promotional padding: "in the heart of", "nestled", "breathtaking".
- No bolded-header bullets (`- **Label:** text`). Use plain bullets or short prose.
- No superficial -ing tails ("ensuring X", "reflecting Y", "showcasing Z").
- No negative parallelism ("not just X, it's Y").
- No copula avoidance ("serves as", "stands as", "functions as"). Use plain "is".
- No sycophantic openers ("great question", "certainly", "of course").
- No filler: "in order to" → "to"; "due to the fact that" → "because"; "at this point in time" → "now".

## Docgen-specific rules

These apply on top of humanise rules and are not covered by humanize-text.

- **Lead with why, not what.** Every section leads with the motivation, constraint, or decision that the reader needs to hold in their head. Mechanics come second.
- **Specific over vague.** Name concrete services, tables, endpoints, flags, numbers. "The orders service calls the ledger via gRPC on port 8443" beats "the services communicate".
- **No "this doc" or "this document".** State what is being described directly.
- **No "we" or "our" unless speaking for the team making the decision.** Prefer "the orders service" over "our orders service" in descriptive prose.
- **Sentence case in prose.** Keep the template's headings exactly as written. Do not title-case body copy.
- **Short sentences. Vary length.** Two long sentences in a row without a short one is a rewrite trigger.
- **No emojis.** Not in prose, not in diagram labels.
- **Numbers as digits.** "3 services", not "three services". Except at the start of a sentence.
- **If a diagram says it, do not repeat it in prose.** Write one sentence above the diagram naming what it shows. Do not narrate the diagram below it.
- **No future-tense promises the doc cannot keep.** "The system will scale to 10× load" is a claim, not a description. Either provide the benchmark or remove it.

## Style checks

Before presenting the finished doc, scan for:

- Zero em dashes anywhere.
- Zero banned AI vocabulary (see list above).
- Zero bolded-header bullets.
- Zero "this doc", "this document", "this design".
- Every section leads with motivation before mechanism.
- Every diagram has exactly one-line framing above it, no narration below.
- Every metric or number is real or marked `TBD`, never a placeholder like `N ms`.
