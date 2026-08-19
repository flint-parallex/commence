# Delivery Document Template

The consolidated output. One document, however many inputs.

Use this structure. Sections may be marked not-applicable, but do not reorder or omit them — the order lets a reader audit the conversion rather than take it on trust: what arrived, then what was made of it.

**This is a business document.** It records the need in the analyst's terms. Columns, types, keys, constraints, build sequence, assertions, monitors and DDL do not appear here — they are authored later in the specification. If a section starts to look like a data model, the work has drifted; record what was said and ask about the rest.

---

# [PRODUCT NAME] — Delivery Requirements

## Change log

| Date | Change | Driven by |
|---|---|---|

A reader arriving mid-delivery must be able to tell what state the document is in and what has moved since they last looked.

## 1. Sources

| Source | Author | Date | Type | Link |
|---|---|---|---|---|

Every input: documents, Confluence pages, emails, meeting notes, chat threads.

Build this first. It is the provenance backbone — any requirement below must be traceable to a row here.

**Conflicts and supersession:** where sources disagree or where later material appears to replace earlier material, note it here and raise it as an open question. Do not resolve it silently. Recency is not correctness, and the older statement may have been the considered one.

## 2. Business need

What the product is for, who consumes it, what decisions it supports — in the analyst's framing.

Keep this close to the sources' own language. This section records what was supplied, not an improvement on it.

## 3. Requirements analysis

Analysed against `what-the-ba-supplies.md`.

Record what the business stated, in business terms. `Delivery translation` restates the business need precisely enough to specify from — it is not a modelling decision.

| Item | As supplied | Source | Delivery translation | Status | Provenance |
|---|---|---|---|---|---|
| Business need | | | | | |
| Semantics | | | | | |
| Grain | | | | | |
| Historization | | | | | |
| Source and lineage | | | | | |
| Quality expectations | | | | | |
| Freshness / SLA | | | | | |
| Consumers and downstream use | | | | | |
| Dependencies | | | | | |

**Status:** `Stated` / `Partial` / `Silent`

**Provenance:** `As supplied` / `Ratified [date]` / `Open`

Leave "delivery translation" empty where nothing was supplied. An empty cell next to `Silent` is correct; a filled one is invention.

Cite the specific source per row. This is what makes the document auditable and what lets the analyst see exactly which of his material produced which requirement.

As questions are answered, update the row and mark it `Ratified [date]` — do not silently absorb an answer as though the requirement had always said that. A reader needs to know whether a requirement came from the original material or from a later conversation.

## 4. Readiness assessment

Against the six preconditions in `readiness-bar.md`. Pass/fail with specific reason.

Then: is each requirement Testable, Estimable, and Small enough to enter a sprint?

## 5. Open questions

Numbered. Written to be sent to the analyst.

| # | Question | Owner | Raised | Needed by | Delivery risk if unanswered |
|---|---|---|---|---|---|

Specific enough to answer. Owner is always the BA.

Readiness only, never rightness. Phrased as questions, not verdicts — this page is read by people beyond the analyst.

### Resolved

| # | Question | Answer | Answered by | Date |
|---|---|---|---|---|

Move questions here as they are answered rather than deleting them. The record of what was asked and answered is evidence the seam is working, and it tells a reader where each ratified requirement came from.

## 6. Assumptions

Anything the delivery translation assumes that the sources did not state.

Every entry is a candidate open question. If an assumption would change the build were it wrong, promote it to section 5.

## 7. Dependencies

Other products, datasets, or platform work this depends on. Named blockers with owners.

Dependencies on products built without documented requirements deserve explicit note — their grain and semantics may be undocumented, which is a live readiness question for anything downstream.

## 8. Handoff

Where each part of this document goes when specification begins.

| From here | To |
|---|---|
| `Stated` and `Ratified` rows in section 3 | Specification §1–§3 |
| `Open` rows and every question in section 5 | Specification §9, owner and date preserved |
| Readiness failures in section 4 | Specification §9, or §10 where they bear on a gate |
| Assumptions not promoted, section 6 | Specification §7 limitations, where they bound approved use |
| Dependencies, section 7 | Specification §3 |

**Open questions carry across unresolved.** An unanswered question arrives in specification §9 with its owner and date intact rather than becoming a gap someone rediscovers later. That continuity is the audit trail.

| Field | Value |
|---|---|
| Specification | `<path, once authored>` |
| Handed off | `<date>` |
| Questions still open at handoff | `<count>` |
