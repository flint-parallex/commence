---
name: delivery-assessment
description: Assesses and validates work already in progress — classifying engineering documents as ADRs or design docs and writing the assessment, structuring and reviewing UAT validation test plans, designing tests from acceptance criteria or from a specification, deciding where a data quality check should live, and writing the test plan for a capability delivered by another team. Use when reviewing an engineering document, writing or reviewing a UAT test plan, proposing tests for a ticket or a data product, deciding whether a check belongs in the shared library or locally, or planning acceptance of a capability from another team. Triggers on phrasing like "is this an ADR or a design doc", "review this design doc", "write the UAT test plan", "what tests do we need for this ticket", "where should this check live", or "we're accepting X from the platform team" without an explicit request.
---

# Delivery Assessment

Assesses and validates work that already exists.

This skill fires **after** a ticket exists and building has started, and **before** the result is trusted in production. `delivery-readiness` gets work to the point where it can be built; this skill establishes whether what came back is what was asked for.

## Scope

**In:** document assessment and ADR/design-doc classification, UAT validation test plans, test design from acceptance criteria and from a specification, check placement, capability test plans and acceptance boundaries.

**Out:** epics, user stories, spikes, decomposition, readiness gating — `delivery-readiness`. Authoring or revising a specification, data model, assertions or runtime monitors — `specification-authoring`. Executive updates, incident reports, release announcements — `delivery-communication`. Consolidating BA material — `ba-requirements-conversion`.

The test: does this artifact **produce** work, or **judge** work that already exists?

## Accountability

Across every capability here:

**Accountable for** a consistent, citation-based assessment, and for naming gaps plainly.

**Not accountable for** making the architectural or design decision. An assessment states what a document does and does not establish; it does not decide the question the document was written to answer.

**Not accountable for** the underlying capability being correct. A test plan establishes what was tested and what was not. Whether the capability works is the providing team's; whether the testing was adequate and clearly reported is ours.

That boundary is what makes an assessment usable. An assessment that also renders a verdict on the decision invites an argument about the verdict, and the assessment stops being read.

## Voice

Every artifact this skill produces is written in the same register.

**Neutral.** State gaps, unmet conditions and blockers as fact, never as a blame narrative. This holds in private outputs as well as published ones — these assessments concern other teams' work, and a document that reads as criticism stops being circulated.

**Citation-based.** An assessment claim points at the passage it came from. An unsourced judgment is an opinion, and it will be argued with rather than acted on.

**Brief.** No padding, no restating the document back, no narrating the process.

**Precise.** Name the section, the table, the condition, the threshold.

## Procedure

Read the reference for the task before starting.

| Task | Reference |
|---|---|
| Decide whether a document is an ADR, a design doc, or both | `classification-criteria.md` |
| Write the assessment | `document-assessment.md` |
| Structure or review a UAT validation test plan | `uat-test-plan.md` |
| Propose tests from a ticket's acceptance criteria, or build a product test suite | `test-design.md` |
| Decide where a data quality check should live | `check-authoring.md` |
| Plan acceptance of a capability delivered by another team | `capability-test-plan.md` |

## Publishing

**This skill writes assessments. It does not render or post them.**

Rendering and posting are centralised in `delivery-communication` — how a platform renders, space and project conventions, field mapping, and the mechanical pre-post check. Centralising it means one place knows each platform's quirks rather than five skills each carrying a stale copy.

When an assessment is ready to go out, hand it over with the destination named: the page or ticket, and the platform. Do not render it here, and do not adapt the wording to fit a platform.

**Where an assessment cannot be rendered without changing what it says, that comes back here.** A table too wide, a structure the platform will not carry — the fix is authoring, not transit. An assessment whose posted version differs from the reviewed version, with nothing recording who changed it, is what this boundary prevents.

## Core principles

**Assess what is there, not what should have been.** A document is judged against what it claims to establish, not against a template. A design doc that does not contain an architectural decision is not deficient — it is a design doc.

**Cite, never characterise.** Point at the passage. "Section 3 states the retention window is 90 days" rather than "the document is unclear about retention."

**Name gaps as gaps, never as failures.** A missing condition is a question for the author. The same information delivered as a deficiency reads as a scorecard, and this is another team's work.

**Tests trace to declared assertions.** Every test carries the ID of the assertion it implements. A test with no assertion behind it is scope that was never declared — see `test-design.md`.

**Never derive a test from observed data.** A test asserts specification. A threshold taken from what the pipeline currently emits is a monitor wearing the wrong label, and it will pass on data that is already wrong.

**A criterion that cannot be expressed as a check is a defect in the criterion**, not a reason to write the test vaguely.

**State what must be true, never which function to call.** Naming an implementation prescribes it, breaks when the library changes, and crosses the what/how line.

**Ask rather than infer.** Thresholds, relationships, implementation targets, agreed conditions. Where the source does not establish something, list it as an open item rather than assuming agreement.

**Write the assessment; hand off the posting.** The artifact leaves here written and unrendered. See Publishing.

## Reference files

- `classification-criteria.md` — the ADR / design doc / combination definitions
- `document-assessment.md` — how to assess a document
- `uat-test-plan.md` — structure and review of a UAT validation test plan
- `test-design.md` — tests from acceptance criteria, and the standing product test suite
- `check-authoring.md` — where a check should live, and how criteria connect to checks
- `capability-test-plan.md` — testing a capability delivered by another team
- `../delivery-communication/references/confluence-formatting.md` — rendering, owned by that skill
- `../capabilities/pipeline-library/reference.md` — what the check library provides and does not
- `../specification-authoring/references/assertion-authoring.md` — the three assertion classes, severity and threshold source

## The line this skill holds

An assessment establishes **what is and is not there**. It does not decide the question, and it does not certify that the thing works.

The test: **if this sentence would settle a decision that belongs to someone else, it does not belong in the assessment.**
