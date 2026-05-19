# TEMPLATES.md: quick reference for common doc types

Reference for the write-doc skill. The list is not closed; accept anything the user names. For an off-list type, find the closest entry below and adapt, or improvise a structure that fits the spirit of the request.

For each type: a one-line purpose, the audience it usually targets, the questions to ask before drafting, the section skeleton, and a one-line note on how to adapt the structure if the audience is non-engineering.

## Glossary used in these templates

A few terms appear across templates and may be unfamiliar to non-engineering readers. Define them on first use in any draft, or substitute the plain term in brackets.

- **Vertical slice**: a thin end-to-end piece of a system that delivers one user-facing capability. (Plain term: a single piece of functionality from input to output.)
- **Principal**: a senior engineer who reviews architecture decisions across teams. (Plain term: senior reviewer.)
- **AO**: architecture overview, the doc described in the first section below. Spell it out on first use.
- **On-call**: the rotation responsible for responding to alerts out of hours. (Plain term: the team member on duty.)
- **Blast radius**: how much else breaks if this part fails. (Plain term: scope of impact.)

## Architecture overview (AO)

**Purpose**: technical overview of a domain. Logical and physical shape, data flows, dependencies, infrastructure.

**Typical audience**: engineers joining the team, engineers in adjacent teams, senior reviewers of the domain.

**Ask before drafting**:
- What is the domain or product area? One sentence.
- Which services or components are inside the boundary? Outside?
- What are the key data flows? (Request paths, async events, batch jobs.)
- Which datastores does it own? Which does it read from?
- Which infrastructure pieces matter? (Hosting, queues, caches, schedulers.)
- Which links into ticketing, on-call, dashboards, runbooks should appear at the top?

**Sections**:
1. Overview (one paragraph: what this domain does and who uses it)
2. Logical view (flowchart, components and their relationships)
3. Physical view (flowchart, hosting, regions, network boundaries, if relevant)
4. Data flows (sequence or flowchart, one per significant flow)
5. Datastores (ER diagram or short table, owned vs read-only)
6. Dependencies (table: name, kind, where it lives, who owns it)
7. Operations (links: runbooks, dashboards, on-call rotation)
8. Open questions / TBD

**Non-engineering audience**: lead with the user outcome (what this domain lets people do), keep the logical view, drop the physical view and datastore detail, and link to the engineering version.

## Feature design

**Purpose**: subpage of an architecture overview. Describes how one piece of functionality works, from input to output.

**Typical audience**: engineers on the team, reviewers of the feature, future maintainers.

**Ask before drafting**:
- What is the feature, in one sentence?
- What problem does it solve, and for whom?
- What did you consider and reject? Why did this approach win?
- Which components or services does it touch?
- What does the request or event flow look like end to end?
- What data does it read or write? Any schema changes?
- What is out of scope?

**Sections**:
1. Summary (one short paragraph, motivation first)
2. Goals and non-goals
3. Design (the actual approach; flowchart or sequence diagram is usually warranted)
4. Data model changes (ER diagram or schema table, only if there are any)
5. Alternatives considered (one short paragraph each, then "rejected because…")
6. Risks and mitigations
7. Rollout (flag, stages, metrics to watch)
8. Open questions

**Non-engineering audience**: replace the "Design" and "Data model" sections with a one-paragraph plain description of how a user moves through the feature. Keep goals, non-goals, and risks. Drop alternatives and rollout.

## Runbook

**Purpose**: steps to debug or recover from a known situation. Optimised for someone reading at 3 a.m. on call.

**Typical audience**: on-call responders, support engineers.

**Ask before drafting**:
- What is the symptom? (What does the alert or report look like?)
- What does the responder need to confirm before acting?
- What are the most likely causes, ranked by frequency?
- What is the safe first action? When is it safe to escalate?
- Which dashboards, queries, or commands are involved?
- Who owns this, and who do you escalate to?

**Sections**:
1. When to use this runbook (the symptom; be exact, including alert names)
2. Quick links (dashboard, log query, on-call rotation, the related service)
3. Triage (numbered list: confirm the symptom, scope the impact)
4. Likely causes and fixes (one subsection per cause, ordered by frequency, each with verification and fix steps)
5. Escalation (who, when, what to include in the handoff)
6. After the incident (what to capture for the postmortem)

**Non-engineering audience**: a runbook for non-engineers is rare. If asked, write it as a how-to article instead.

## Getting started

**Purpose**: new joiner from zero to first useful contribution.

**Typical audience**: someone joining the team this week.

**Ask before drafting**:
- What does "first useful contribution" mean for this team? (Run the app locally? Ship a tiny PR? Pair on an on-call shift?)
- What access do they need, and from whom?
- Which tools are mandatory? Which are optional?
- What are the team's working agreements (standup, code review, on-call, communications)?
- Who is the buddy or first point of contact?

**Sections**:
1. Welcome (one short paragraph: what the team does, who the reader will work with)
2. Day one (accounts, access, hardware, channels; checklist)
3. Local setup (numbered steps, copy-pasteable commands)
4. First contribution (a real, small, scoped task)
5. How we work (standup cadence, review norms, on-call expectations)
6. Where to ask questions (channels, docs, people)
7. Further reading (links to architecture overview, key feature designs, runbooks)

**Non-engineering audience**: drop "Local setup" and "First contribution" as written. Replace with the equivalent ramp-up: tools to install, first meeting to attend, first artefact to read or produce.

## README

**Purpose**: the first file a reader sees when opening the repo. Gets them productive fast. (These rules assume internal closed-source READMEs, not public open-source projects, unless the user says otherwise.)

**Typical audience**: another engineer at the company opening the repo for the first time.

**Ask before drafting**:
- What does this repo contain? One sentence.
- Who owns it? (Team, channel, on-call.)
- How do you run it locally?
- How do you run the tests?
- How does it deploy?
- Is there a related architecture overview or runbook to link?

**Sections**:
1. Title and one-line description
2. Owners (team, channel, on-call link)
3. What it does (one short paragraph)
4. Local development (prerequisites, install, run, test; copy-pasteable)
5. Architecture (one paragraph plus a small flowchart, or a link to the architecture overview)
6. Deployment (where, how, who can trigger it)
7. Related docs (architecture overview, runbooks, dashboards)

Skip badges, contributor sections, and licence boilerplate unless the user asks.

**Non-engineering audience**: lead with what the repo produces (an app, a job, a library) and who uses the output. Move local development and deployment into a single "for engineers" section at the bottom, or drop entirely.

## Tech debt note

**Purpose**: a small page describing one piece of known debt, so future readers find it before they hit it.

**Typical audience**: engineers working in or near the affected code.

**Ask before drafting**:
- What is the debt, in one sentence?
- Where does it live? (Files, modules, services.)
- Why is it there? (What was the constraint that produced it?)
- What is the impact today? (Slowness, bugs, blocked work.)
- What would "fixed" look like?
- Is there a ticket?

**Sections**:
1. Summary (one paragraph)
2. Where it lives (file paths, code links)
3. Why it exists (history)
4. Impact (concrete, not hypothetical)
5. What "fixed" looks like
6. Tracking (ticket or "no ticket yet")

**Non-engineering audience**: keep "Summary", "Impact", and "What fixed looks like". Drop the file paths.

## How-to article

**Purpose**: ad-hoc guide for one specific task ("how to rotate the X secret", "how to backfill Y").

**Typical audience**: someone who has the task in front of them right now.

**Ask before drafting**:
- What task does this cover, exactly? (Be specific: "rotate the Postgres replica password", not "rotate secrets".)
- Who is allowed to do this?
- What are the prerequisites?
- What are the steps, in order?
- How do you verify it worked?
- What rolls it back if it goes wrong?

**Sections**:
1. What this covers (one sentence) and who can do it
2. Prerequisites (access, tools, time window)
3. Steps (numbered, with the exact commands)
4. Verification (how to confirm success)
5. Rollback (if applicable)
6. Related (links to runbooks, the system, the owning team)

**Non-engineering audience**: replace command-line steps with the equivalent UI clicks. Keep prerequisites, verification, and rollback as plain checks.

## Implementation plan

**Purpose**: proposal for a piece of functionality. Like a feature design, but framed as a plan to build it.

**Typical audience**: reviewers (tech lead, senior reviewer), the engineer who will execute.

**Ask before drafting**:
- What is the piece of functionality? (One sentence.)
- Why now?
- What is the proposed approach?
- How does it break into shippable chunks? (Each chunk should be reviewable on its own.)
- What is the rough order of work?
- What could go wrong, and what is the fallback?

**Sections**:
1. Summary (one paragraph: what and why)
2. Approach (the design; flowchart or sequence usually helps)
3. Plan (numbered, each chunk small enough to ship in a PR)
4. Risks
5. Success criteria (the observable checks that say "done")
6. Open questions

**Non-engineering audience**: replace "Plan" with a milestone view (dates and what is delivered at each one). Keep summary, risks, and success criteria.

## RFC

**Purpose**: earlier than an implementation plan. Proposes an approach and asks for feedback before commitment.

**Typical audience**: the team and adjacent teams who will weigh in.

**Ask before drafting**:
- What is the problem? (Be precise. RFCs that solve a vague problem get vague feedback.)
- What options have you considered?
- For each option, what are the pros and cons?
- Which option do you currently lean toward, and why?
- Who do you specifically want feedback from?

**Sections**:
1. Problem (one short paragraph)
2. Constraints (anything that bounds the solution)
3. Options considered (one subsection per option, each with pros, cons, and a one-line summary at the top)
4. Recommendation (which option, and why); leave room for the team to disagree
5. Open questions
6. Reviewers (named, who you want input from)

**Non-engineering audience**: keep problem, constraints, options, and recommendation. Move technical detail inside each option to a sub-bullet so the surface stays readable.

## Off-list types

When the user names a doc type that is not above:

1. Ask once whether one of the listed types is close enough to use as a base.
2. If not, improvise. The pattern that works for almost anything is: one-paragraph summary, the body in a few H2s, a self-check at the end ("does this answer the question I started with?").

Always still ask the audience first. Always still apply STYLE.md and HUMANIZE.md, including the audience-translation pass.
