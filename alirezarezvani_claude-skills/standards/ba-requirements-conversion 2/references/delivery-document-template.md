# Delivery Document Template

The consolidated output. One document, however many inputs.

Use this structure. Sections may be marked not-applicable, but do not reorder or omit them — the order lets a reader audit the conversion rather than take it on trust: what arrived, then what was made of it.

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

## 5. Epics and stories

Derived from section 3, not from raw source prose.

**Epic:** [name] — the business value it delivers, traced to its source.

| Story | Acceptance criteria | Traces to |
|---|---|---|

Acceptance criteria expressible as automated data checks. A story whose AC cannot be made testable from the supplied material does not appear here — it appears in section 6.

If the material does not yet support epics, say so and leave this section empty. A document that stops at open questions is correct output for material not yet ready to build from.

## 6. Open questions

Numbered. Written to be sent to the analyst.

| # | Question | Owner | Raised | Needed by | Delivery risk if unanswered |
|---|---|---|---|---|---|

Specific enough to answer. Owner is always the BA.

Readiness only, never rightness. Phrased as questions, not verdicts — this page is read by people beyond the analyst.

### Resolved

| # | Question | Answer | Answered by | Date |
|---|---|---|---|---|

Move questions here as they are answered rather than deleting them. The record of what was asked and answered is evidence the seam is working, and it tells a reader where each ratified requirement came from.

## 7. Assumptions

Anything the delivery translation assumes that the sources did not state.

Every entry is a candidate open question. If an assumption would change the build were it wrong, promote it to section 6.

## 8. Dependencies

Other products, datasets, or platform work this depends on. Named blockers with owners.

Dependencies on products built without documented requirements deserve explicit note — their grain and semantics may be undocumented, which is a live readiness question for anything downstream.
