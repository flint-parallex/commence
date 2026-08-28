---
name: specification-authoring
description: Authors and revises canonical Data Solution specifications — purpose, grain, data model, semantics, build sequence, and assertions — verifying declared facts against real tables in the platform catalog, emitting the catalog DDL that carries the same semantics into the warehouse, and projecting the specification into an ODCS data contract. Use when writing or revising a specification, defining a data model or grain for a data product, writing column meanings or catalog descriptions, capturing a build sequence, authoring assertions, generating or reconciling a data contract, or checking a specification for internal consistency. Triggers on phrasing like "write the spec for", "define the data model", "what's the grain", "document this product", "check the spec against the tables", "update the contract", or "is the spec consistent" without an explicit request.
---

# Specification Authoring

Produces the canonical delivery document that everything downstream is built from.

The job is not filling in a template. It is recording intent that cannot be recovered from the data — what the product is for, why it is shaped this way, and what must be true of it — with enough precision that a reader who never spoke to the author can maintain it, and a decomposition can be derived from it without further questions.

## Scope

**In:** specifications, data models, grain and key definitions, column semantics, relationships and their implications, build sequences, assertions in all three classes, catalog DDL and comments, verification of a specification against real tables, internal consistency of a specification, and the ODCS data contract projected from it.

**Out:** tickets, epics, decomposition, readiness assessment, acceptance criteria on tickets. Those belong to `delivery-readiness`. Sprint and stakeholder communication belongs to `delivery-communication`.

The test: does this artifact state **what must be true of the product**, or **what work will be done**?

## Voice

**Business first.** Section 1 is read by people who do not know the schema. If a reader outside the delivery team cannot follow the purpose, rewrite it.

**Precise.** Name the entity, the grain, the column, the cardinality, the threshold. A specification that could describe a different product is not specific enough.

**Neutral.** Gaps, limitations and unresolved questions are stated as fact, never apologised for or minimised. A known unknown that is written down is a scoped risk; the same unknown left implied reads as coverage.

**No implementation, anywhere.** There is no field in the template for a join type, an operation order, a deduplication strategy, materialisation, partitioning, clustering, incremental strategy, orchestration, scheduling or tuning. If you find yourself wanting to write one, it belongs to engineering. State what must be true of the result.

## Core principles

**Ask, never invent.** Entity, grain, keys, consumers, owner, cadence, thresholds, business reasons — if not supplied, ask. An invented specific is worse than a stated gap, because a gap is visible and an invention is not.

**Verify and discover, never source.** Real tables may be queried to test whether a declared fact holds, and to find content no declared rule covers. They may never be queried to decide what the fact should be. Discovery reports a gap and asks; it does not propose the rule that fills it. See `verification.md` — this is the rule most easily broken and the one that does the most damage when it is.

**Survey before writing logic rules.** Parse, derivation and classification rules authored without looking at real source content cover the cases the author imagined. Run discovery first; write the rules against what is actually there.

**The specification is the oracle, not the table.** Where a declared fact and the data disagree, that is a finding to report, not a correction to apply. The data may be wrong. That is frequently the point.

**Intent is elicited, not generated.** Section 5 records why each stage exists. That reason lives with the person who decided it and cannot be inferred from the transformation. Ask; do not supply a plausible business justification.

**Curated inputs only.** Author from curated context — the canonical requirement, decisions with their rationale, the data model. Raw transcripts and original requirement documents contain positions that were floated and withdrawn, and nothing in the text marks which survived. Where raw material is the only source for a statement, attribute it and flag it for confirmation.

**One entity, one specification.** A specification covering two Solutions has organised around a source system instead of around a product. Split it.

**Every hazard becomes an assertion.** A fan-out written as a warning is something a reader can miss. Written as an assertion on uniqueness, it is a gate. See `assertion-authoring.md`.

**The binding column stays blank.** The specification declares the assertion. Engineering records where the check lives. Verify the binding exists at acceptance — never populate it.

**The contract is a projection, never a source.** The ODCS contract is generated from the specification, diffed and replaced — never hand-edited, never consulted as a fact, never filled from a table to make it validate. If the contract needs something the specification does not state, the fix is in the specification. See `data-contract.md`.

**Draft accumulates; gates reconcile.** Drafting is deliberately unstable, and projecting every edit into the contract produces version churn on the one artifact consumers rely on. The `Status` field is the gate. Inconsistency during drafting is not an error; inconsistency at a gate is.

**Freeze before improving.** The template is versioned. Extend it when a real Solution needs something it does not carry, not before.

## Procedure

1. **Establish the entity and grain first.** Everything else is unstable until these are fixed. One entity, one row means exactly what. If two entities are in play, stop — there are two specifications.
2. **Collect the curated inputs.** Canonical requirement, decisions and their reasons, existing data model. Name what is missing rather than working around it.
3. **Author sections 1–4** — definition, service commitments, sources, assets. Delivery windows per `delivery-windows.md`; the processing window is computed and its feasibility assessed, not assumed. Column `Meaning` is the field only this seat can supply; do not leave it to be inferred from the column name.
4. **Verify and survey against the platform** per `verification.md`. Verification tests declared facts and reports findings. Discovery surveys for content no declared rule covers and reports questions. Neither edits the specification, and neither proposes a rule or a threshold.
5. **Elicit the build sequence** per `build-sequence.md`. Required end state, reason, expected effect, dependency — per stage.
6. **Author the assertions** per `assertion-authoring.md`. Three classes, declared separately. Severity on 6a and 6b. Runtime monitors per `runtime-monitors.md`. Referential assertions in both directions per `referential-integrity.md`, and a conservation assertion for every stage in section 5.
7. **Complete sections 7–9** — governance, catalog metadata, open items routed to engineering.
8. **Emit the catalog DDL** per `catalog-ddl.md`, so the semantics reach the warehouse rather than living only in the document.
9. **Sweep for internal consistency** per `consistency-sweep.md`. Full sweep at every status change and before any contract reconciliation; checks 1–5 after an edit to §1 grain or §4 columns, keys or relationships. The sweep reports disagreements between sections and never resolves them.
10. **Reconcile the data contract** per `data-contract.md`. Regenerate from the specification, carry forward what the specification does not own, diff, derive the version bump from §4 and §7, validate against the pinned JSON Schema. Automatic at `Ready` and at any contract-bearing edit once the specification is `Delivered`. Not during `Draft` unless asked.
11. **Assess against `../delivery-readiness/references/definition-of-ready.md`.** Report the three gates and every unmet criterion with its owner. Do not mark a gate passed to close the document.

## What this skill does not do

**It does not decide readiness on the author's behalf.** Completeness is reported; the operator decides whether to proceed.

**It does not write tickets.** A finished specification is the input to `../delivery-readiness/references/decomposition.md`. Do not propose work from here.

**It does not populate a binding, a severity the operator has not stated, or a threshold derived from data.**

**It does not fill a field to avoid leaving it blank.** An unanswered field with a named owner is a better artifact than a plausible guess.

**It does not edit a data contract by hand,** emit a contract field the pinned JSON Schema does not define, or propose a specification change for the purpose of making a contract validate.

**It does not resolve a consistency finding on the author's behalf.** Where two sections disagree, both are reported and the operator decides which is right.

## References

- `specification-template.md` — the canonical template. Copy per Solution; never author freehand
- `verification.md` — what may be checked against real tables, and what may never be sourced from them
- `build-sequence.md` — section 5. Elicitation method and the end-state test
- `assertion-authoring.md` — sections 6a–6c. Hazard-to-assertion conversion, the three classes, threshold source
- `runtime-monitors.md` — section 6c in detail. Measure, evaluation window, calibration, and the alert fields
- `monitor-calibration.md` — deriving observed-history thresholds from approved history, and the monitor SQL
- `referential-integrity.md` — orphan and loss assertions for child, bridge and pipeline processing assets
- `delivery-windows.md` — section 2. Delivery deadlines, processing window, missed-window behaviour, corrections
- `catalog-ddl.md` — emitting comments and constraints so the specification's semantics reach the warehouse
- `consistency-sweep.md` — cross-section invariants, when they run, and the report-never-resolve rule
- `data-contract.md` — projecting the specification into an ODCS contract: mapping, lifecycle gating, reconciliation, version bumps
- `odcs-<version>/` — the vendored Open Data Contract Standard. Section markdown for meaning, JSON Schema for conformance. Pinned; never fetched at runtime
- `../delivery-readiness/references/definition-of-ready.md` — the standard this specification is written against
- `../delivery-communication/references/confluence-formatting.md` — storage format for anything published to Confluence

## The line this skill holds

A specification states **what must be true of the product**. Engineering decides **how it is produced**.

The test: **if the statement would change when the implementation changes, it does not belong in the specification.**

A required end state that reads like SQL has crossed the line. Rewrite it as a statement about the data.
