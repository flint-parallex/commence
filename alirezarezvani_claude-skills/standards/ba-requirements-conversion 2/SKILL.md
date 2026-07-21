---
name: ba-requirements-conversion
description: Consolidate business analyst requirements — documents, Confluence pages, emails, meeting notes, Slack threads — into a single delivery document for a data product, with epics, user stories, acceptance criteria, and the open questions needed to resolve gaps with the analyst. Use this skill whenever the user provides BA-supplied material about a dataset or data product and wants it turned into delivery requirements, an intake or delivery document or epics. Also use when the user asks what is missing from a requirement, whether something is ready to build, what to ask a BA or dataset owner, or how to consolidate scattered requirements into one place. Trigger on casual phrasing too ("turn Tim's business requirements into delivery docs", "is this ready", "what should I ask him") — any time BA material needs to become a delivery artifact, this skill applies.
---

# BA Requirements Conversion

Consolidates scattered business analyst material into one delivery document that carries the epic and story pipeline, and the open questions that drive resolution with the analyst.

## What this produces

**One consolidated delivery document** from however many inputs — requirements docs, Confluence pages, emails, meeting notes, chat threads.

It serves two purposes at once:

**Delivery.** It is the source for epics and their underlying user stories, so the business value the analyst described actually gets built.

**Conversation.** It surfaces open questions specific enough for the analyst to answer, so quality gets resolved before delivery rather than during it.

The open questions are a deliverable, not a byproduct. Write them to be sent.

**This document is published.** It goes on a page the analyst, their management, and delivery stakeholders can read, and it stays there as the standing record of what was requested and what is being built. Read `references/published-document-discipline.md` before writing — it governs tone and the ratification lifecycle, and both differ from what an internal working document would need.

## The governing principle

**The job is translation fidelity, not authorship.**

The BA owns the business need — the what and the why. The PO owns translation into delivery artifacts and owns the fidelity of that translation.

That boundary is easy to cross without noticing. A requirement arrives vague; the delivery artifact needs precision; the gap gets filled with something plausible. The artifact now looks complete and reads well — but a requirement authored during translation is a requirement nobody validated with the business. When it turns out wrong, the error belongs to whoever wrote it rather than whoever should have decided it.

So: **when the source does not answer something, say so and ask. Never infer, never smooth over, never write the plausible version.**

The value of this skill is in what it refuses to fill in. A document with many gaps flagged is correct output when the sources were thin — it is an accurate picture of what was actually supplied.

## Working from multiple sources

Inputs arrive scattered and rarely agree perfectly. Handle this explicitly rather than silently merging.

**Inventory the sources first.** Title, author, date, type, link. Build this before analyzing anything — it is the provenance backbone, and a reader must be able to trace any requirement back to what said it.

**Attribute every requirement to its source.** Note which document and which date.

**Surface conflicts; do not resolve them.** Where two sources disagree, record both, note which is more recent, and raise it as an open question. Silently picking the newer one is a decision the analyst should be making — recency is not the same as correctness, and the older statement may have been the considered one.

**Distinguish stated from implied.** A requirement written in a formal document carries different weight than one mentioned in passing in a meeting. Note the difference where it matters.

**Watch for supersession.** Later material may replace earlier material without saying so. Flag suspected supersession rather than assuming it.

## Analyzing the inputs

Read `references/what-the-ba-supplies.md`. That inventory is the analysis lens — the material gets read against it, and each item recorded as **Stated**, **Partial**, or **Silent**.

`Partial` deserves the most attention. Partial answers read as answers and invite false precision: "recent data" and "the main clients" feel like requirements but cannot be built from. Record what was actually said, then raise the question.

Every `Partial` and every `Silent` becomes an open question.

## Output structure

Use `references/delivery-document-template.md` for the full structure.

The document runs: sources → business need → requirements analysis → readiness assessment → epics and stories → open questions → assumptions → dependencies.

That order is deliberate. What arrived comes before what was made of it, so a reader can audit the conversion rather than take it on trust.

## Epics and stories

Derive these from the consolidated requirements, not from raw source prose.

Structure as epic → stories, sized to fit a sprint. Each epic should map to business value the analyst actually described — if an epic cannot be traced to something in the sources, it was invented.

**Acceptance criteria must be expressible as automated data checks** wherever a story touches data. Not "the data should be accurate" but a check that can run: row counts in range, no nulls in a key column, referential integrity against a named source, a reconciliation that balances.

**A story whose AC cannot be made testable from the supplied material does not get written.** It becomes an open question instead.

This is where the never-fill rule is most tempting to break. A vague AC feels unfinished and the instinct is to sharpen it — but an AC sharpened during ticket writing is an AC the PO authored. Same boundary crossing as inventing a requirement, one layer downstream and harder to spot.

Apply INVEST as a readiness lens. **Testable, Estimable, Small** are the three that fail most often on data work.

**Where the supplied material does not support epics at all**, say so plainly and produce the analysis and open questions without them. A document that stops at open questions is the correct output for material that is not yet ready to build from.

## Open questions

These drive the conversation with the analyst, so they must be answerable by him.

For each:
- The question, specific enough to answer
- **Owner:** the BA — always, never the PO
- Date raised, date needed
- Delivery risk if unanswered

**Specificity is what makes them useful.** "Clarify the grain" cannot be answered. "Is this one row per asset, or one row per asset per valuation date?" can.

**Phrase gaps as questions, not verdicts.** "What is the intended grain?" rather than "grain not supplied." The information is identical; one is a person asking and the other is a scorecard on a page other people read. See `references/published-document-discipline.md`.

**Every question must be about readiness, never rightness.** Readiness is whether the requirement can be built as stated. Rightness is whether the business should want it. Readiness questions are the PO's to raise and are answerable; rightness questions belong to the business, and raising them reads as second-guessing the analyst's judgment.

| Readiness (raise these) | Rightness (do not) |
|---|---|
| "What is the grain — one row per asset, or per asset per date?" | "Should the business really need this field?" |
| "How is 'active' defined for this metric?" | "Is 'active' the right thing to measure?" |
| "This isn't testable as written — what's the pass/fail condition?" | "This priority seems wrong." |
| "What's the freshness expectation?" | "Do consumers actually need this daily?" |

## Readiness assessment

Read `references/readiness-bar.md` and assess each requirement against the six data preconditions. Pass/fail with a specific reason. Every failure becomes an open question.

## Work that predates the process

Sometimes the product is already built and the task is documenting what happened, not gating what is coming. Say so and stop — no readiness assessment, no open questions. Status is `Documented — pre-process`.

There is no intake to record for work that predates the intake process, and backfilling requirements documentation for an already-built product means authoring requirements nobody supplied. Same boundary crossing, pointed backwards. Leave those sections absent with a note that the work predates the process. The absence is accurate record.

Describe this neutrally. Work built before a standard existed is ordinary, not deficient, and framing it as remediation invites a conversation nobody needs to have.

## External artifacts

Where schema summaries, data overviews, or specification outputs already exist from other systems, **reference them — do not restate them.**

Restating another system's output into this document breaks its provenance and makes it ambiguous who produced what. Link and cite instead.

## Status vocabulary

When output includes an intake state or delivery status, read `references/status-vocabulary.md` and use only the values defined there.

The values describe where the work is, not the quality of what was supplied. Do not invent status labels, and do not substitute a value that assesses the incoming requirement — that reads as a verdict on the analyst on a page other people see.

## Reference files

- `references/published-document-discipline.md` — tone and ratification lifecycle for a published page
- `references/what-the-ba-supplies.md` — the analysis lens: what the BA owes
- `references/delivery-document-template.md` — output structure
- `references/readiness-bar.md` — the six data readiness preconditions
- `references/status-vocabulary.md` — permitted status values
