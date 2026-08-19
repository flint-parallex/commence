---
name: ba-requirements-conversion
description: Consolidate business analyst requirements — documents, Confluence pages, emails, meeting notes, Slack threads — into a single canonical business requirements document for a data product, recording what was supplied, what was not, and the open questions needed to resolve the gaps with the analyst. Use this skill whenever the user provides BA-supplied material about a dataset or data product and wants it turned into an intake or requirements document. Also use when the user asks what is missing from a requirement, whether something is ready to specify, or what to ask a BA or dataset owner. Trigger on casual phrasing too ("turn Tim's business requirements into a requirements doc", "is this ready", "what should I ask him") — any time BA material needs to become a delivery input, this skill applies.
---

# BA Requirements Conversion

Consolidates scattered business analyst material into one canonical business requirements document, and the open questions that drive resolution with the analyst.

## What this produces

**One consolidated business requirements document** from however many inputs — requirements docs, Confluence pages, emails, meeting notes, chat threads.

It serves two purposes at once:

**Record.** It is the auditable statement of what the business asked for, traced to who said it and when. It is the input to `specification-authoring`.

**Conversation.** It surfaces open questions specific enough for the analyst to answer, so gaps get resolved before specification rather than during it.

The open questions are a deliverable, not a byproduct. Write them to be sent.

**This document is published.** It goes on a page the analyst, their management, and delivery stakeholders can read, and it stays there as the standing record of what was requested. Read `references/published-document-discipline.md` before writing — it governs tone and the ratification lifecycle.

## Where this sits

```
BA material  →  THIS DOCUMENT  →  specification.md  →  work set  →  tickets
                (what business      (what must be
                 asked for)          true of the product)
```

**This is a business document.** It records the need in the analyst's terms. It is not a draft specification and must not become one.

### What this document does not contain

Everything below belongs to `specification-authoring` and is authored later, from this document plus the platform.

| Not here | Belongs to |
|---|---|
| Column lists, logical types, nullability | Specification §4 |
| Primary keys, constraints, declared cardinality | Specification §4 |
| Build sequence, stages, filter chain | Specification §5 |
| Assertions in any of the three classes | Specification §6 |
| Runtime monitors, thresholds, calibration | Specification §6c |
| Catalog DDL, comments, certification status | Specification §8 |
| Compatibility promise, versioning, retirement | Specification §7 |
| Epics, stories, tickets, acceptance criteria | `delivery-readiness` |

**Grain is the one that looks like an exception and is not.** This document records grain as the business stated it — *one row per what*, in business language. It does not record a declared key, a uniqueness constraint, or a composite. Those are modelling decisions made during specification, against real data.

**If a section here starts to look like a data model, stop.** The material has either outrun what the BA supplied, or the work has drifted into specification. Both mean the same correction: record what was said, ask about the rest.

## The governing principle

**The job is translation fidelity, not authorship.**

The BA owns the business need — the what and the why. The PO owns the record of that need and owns the fidelity of the record.

That boundary is easy to cross without noticing. A requirement arrives vague; the document wants precision; the gap gets filled with something plausible. The document now looks complete and reads well — but a requirement authored during conversion is a requirement nobody validated with the business. When it turns out wrong, the error belongs to whoever wrote it rather than whoever should have decided it.

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

`Partial` deserves the most attention. Partial answers read as answers and invite false precision: "recent data" and "the main clients" feel like requirements but cannot be specified from. Record what was actually said, then raise the question.

Every `Partial` and every `Silent` becomes an open question.

## Output structure

Use `references/delivery-document-template.md` for the full structure.

The document runs: sources → business need → requirements analysis → readiness assessment → open questions → assumptions → dependencies → handoff.

That order is deliberate. What arrived comes before what was made of it, so a reader can audit the conversion rather than take it on trust.

`references/example-delivery-document.md` is a worked example. Read it for the shape — particularly for how much is left blank.

## Open questions

These drive the conversation with the analyst, so they must be answerable by him.

For each:
- The question, specific enough to answer
- **Owner:** the BA — always, never the PO
- Date raised, date needed
- Delivery risk if unanswered

**Specificity is what makes them useful.** "Clarify the grain" cannot be answered. "Is this one row per asset, or one row per asset per valuation date?" can.

**Phrase gaps as questions, not verdicts.** "What is the intended grain?" rather than "grain not supplied." The information is identical; one is a person asking and the other is a scorecard on a page other people read. See `references/published-document-discipline.md`.

**Every question must be about readiness, never rightness.** Readiness is whether the requirement can be specified as stated. Rightness is whether the business should want it. Readiness questions are the PO's to raise and are answerable; rightness questions belong to the business, and raising them reads as second-guessing the analyst's judgment.

| Readiness (raise these) | Rightness (do not) |
|---|---|
| "What is the grain — one row per asset, or per asset per date?" | "Should the business really need this field?" |
| "How is 'active' defined for this metric?" | "Is 'active' the right thing to measure?" |
| "What would make this wrong — what should reconcile, and against what?" | "This priority seems wrong." |
| "What's the freshness expectation, and what breaks if it slips?" | "Do consumers actually need this daily?" |

**Ask in business terms, not modelling terms.** The question is what the business needs to be true, not how it will be checked. *"What must this reconcile against?"* is answerable by an analyst; *"what should the referential assertion be?"* is not, and it is not his to answer.

## Readiness assessment

Read `references/readiness-bar.md` and assess each requirement against the six preconditions. Pass/fail with a specific reason. Every failure becomes an open question.

**This bar is not the Definition of Ready.** The readiness bar asks whether the BA supplied enough to write a specification from. The Definition of Ready asks whether a finished specification is complete enough to build from. Different gates, different moments, and a requirement can clear this one and still be far from ready to build.

## Handoff

The document has a destination, and the mapping is fixed:

| In this document | Becomes |
|---|---|
| `Stated` rows in the analysis table | Source material for specification §1–§3 |
| `Ratified` rows | The same, with the ratification date carried across |
| `Open` rows and every open question | Specification §9 open items, with owner and date preserved |
| Readiness failures | Specification §9, or §10 where they bear on a readiness gate |
| Assumptions not promoted | Specification §7 limitations, where they bound approved use |

**Open questions carry across; they are not resolved by the handoff.** An unanswered question does not become a specification gap that someone rediscovers later — it arrives in §9 with its owner and its date intact. That continuity is the audit trail, and it is why no separate tracking system is needed.

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
- `references/example-delivery-document.md` — a worked example
- `references/readiness-bar.md` — the six preconditions for specification
- `references/status-vocabulary.md` — permitted status values
