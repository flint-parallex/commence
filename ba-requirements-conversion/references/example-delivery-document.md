# Example — Delivery Document

A worked example of `delivery-document-template.md`, filled from realistically thin BA material.

**Read this for the shape, not the content.** What it demonstrates: how `Partial` and `Silent` are recorded, what an empty delivery-translation cell looks like next to a gap, and how open questions get phrased so an analyst can answer them. The material below is invented.

Note what is absent. No columns, no types, no keys, no assertions, no monitors — none of that is the business's to supply, and all of it is authored later in the specification. This document records what the business asked for, and what it did not say.

**The most important thing this example shows is how much is left blank.** The BA supplied a real business need and almost none of the modelling detail. That is normal. A document that filled those gaps would look more finished and be worth less.

Note that there are no epics or stories, and no section for them. This artifact ends at open questions — work is derived from the specification afterwards.

---

# 13F Institutional Holdings — Delivery Requirements

## Change log

| Date | Change | Driven by |
|---|---|---|
| 2026-08-14 | Initial consolidation from three sources | Intake |
| 2026-08-18 | Q3, Q7 answered; rows 2 and 6 marked Ratified | BA response, 2026-08-18 |

## 1. Sources

| Source | Author | Date | Type | Link |
|---|---|---|---|---|
| "Institutional Ownership — Analytics Need" | M. Reyes (BA) | 2026-07-29 | Confluence page | `<link>` |
| "Re: ownership data — timing" | M. Reyes | 2026-08-05 | Email thread | `<link>` |
| Requirements walkthrough | M. Reyes, J. Okafor | 2026-08-11 | Meeting notes | `<link>` |

**Conflicts and supersession:** the Confluence page states positions are needed "quarterly." The 2026-08-05 email states the research team needs them "as filings come in, not at quarter end." These are not obviously reconcilable — the second may describe an updated need or may describe a different consumer. Raised as Q1. Not resolved here.

## 2. Business need

From the Confluence page, in the analyst's framing: research and risk teams currently pull institutional ownership from a vendor terminal one manager at a time, which makes cross-manager questions — who else holds this, how concentrated is ownership, which managers moved together — impractical to answer. The stated need is to make disclosed institutional positions queryable across managers and over time.

The meeting notes add that ownership figures currently appear in client-facing security profiles and are assembled manually each quarter.

## 3. Requirements analysis

| Item | As supplied | Source | Delivery translation | Status | Provenance |
|---|---|---|---|---|---|
| Business need | Cross-manager ownership analysis; concentration and crowding; replace manual quarterly assembly | Confluence 2026-07-29; meeting 2026-08-11 | Positions queryable across managers and periods | Stated | As supplied |
| Semantics | "Positions held by institutional managers." Meeting clarified that options positions are in scope and short positions are "not in this data" | Confluence; meeting 2026-08-11 | Reported positions including derivatives; short positions out of scope | Partial | Ratified 2026-08-18 |
| Grain | Not addressed | — | | Silent | Open |
| Historization | Not addressed. Meeting mentioned "we'd want to see changes quarter over quarter" without specifying whether restated filings replace or accumulate | Meeting 2026-08-11 | | Partial | Open |
| Source and lineage | SEC EDGAR filings. No statement on which source is authoritative where a manager's own disclosure conflicts with a vendor figure | Confluence 2026-07-29 | EDGAR as source system | Partial | Open |
| Quality expectations | "Should match what the terminal shows." No tolerance stated, no reconciliation target named | Confluence 2026-07-29 | | Partial | Open |
| Freshness / SLA | Email: needed as filings arrive. Confluence: quarterly. Conflict — see section 1 | Email 2026-08-05; Confluence 2026-07-29 | | Partial | Open |
| Consumers and downstream use | Research, risk, and client reporting named. Client reporting confirmed as downstream of a manual process today | Confluence; meeting 2026-08-11 | Three named consumers | Stated | Ratified 2026-08-18 |
| Dependencies | Not addressed | — | | Silent | Open |

Nine rows, two Stated, one now Ratified from a conversation rather than the original material — which is why the provenance column exists.

**On the empty cells:** grain, historization, quality and freshness have no delivery translation because nothing was supplied that could be translated. Writing "one row per position per filing" in the grain cell would be inventing the single most consequential modelling decision in the product.

## 4. Readiness assessment

| Precondition | Result | Reason |
|---|---|---|
| (a) Stable versioned upstream contract | Dependency, not failure | EDGAR publishes versioned schemas and announces changes, but the organization has no contract-registration mechanism. Recorded as a dependency; no action available to the BA |
| (b) Source available, accessible, profiled | Fail | Filings are public and available. No one has profiled them. The free-text manager-attribution field in particular is unprofiled — see Q5 |
| (c) Business has stated what correct looks like | Fail | "Should match what the terminal shows" names no reconciliation target and no tolerance. Nothing states what would make the data wrong — Q4 |
| (d) Grain, semantics, historization unambiguous | Fail | Grain silent, historization partial. Semantics ratified 2026-08-18 and now adequate |
| (e) Upstream dependencies named and sequenced | Fail | Not addressed. Instrument and entity reference data are almost certainly required and are unmentioned — Q6 |
| (f) Technical feasibility on current platform | Pass | Ingestion, parsing and serving are all within existing platform capability |

Four failures, one dependency, one pass. This requirement is not yet specifiable — an ordinary state for material three weeks into intake, not a judgment on the analyst.

## 5. Open questions

| # | Question | Owner | Raised | Needed by | Risk if unanswered |
|---|---|---|---|---|---|
| 1 | The Confluence page says positions are needed quarterly; the 5 Aug email says as filings arrive. Are these two different consumers with different needs, or has the need changed? | M. Reyes | 2026-08-14 | 2026-08-22 | Refresh design and cost profile differ substantially between the two. Building for the wrong one is a rebuild, not a tuning change |
| 2 | For cross-manager analysis, is one row one position per manager per period — or one position per filing, where a single filing may report on behalf of several managers? | M. Reyes | 2026-08-14 | 2026-08-22 | Determines the data model. A grain change after build is a rebuild |
| 3 | ~~Are options positions in scope?~~ | M. Reyes | 2026-08-14 | — | **Answered 2026-08-18** — see Resolved |
| 4 | "Should match what the terminal shows" — which figure specifically, and what difference would be acceptable? Position value, share count, or both? | M. Reyes | 2026-08-14 | 2026-08-25 | Without a named target and tolerance, there is no condition under which this can be accepted |
| 5 | Where a filing reports on behalf of several managers, the filing attributes each position to a manager using a reference that is filer-supplied free text. Should positions that cannot be attributed be excluded, or included and marked? | M. Reyes | 2026-08-14 | 2026-08-25 | Changes reported totals per manager. Excluding silently understates a manager's holdings; the choice belongs to the business |
| 6 | Does ownership analysis need positions resolved to instruments and managers resolved to entities — or is the identifier as filed sufficient? | M. Reyes | 2026-08-14 | 2026-08-25 | Determines whether reference data products are dependencies of this one |
| 7 | ~~Which teams consume this?~~ | M. Reyes | 2026-08-14 | — | **Answered 2026-08-18** — see Resolved |
| 8 | When a manager amends a previously filed report, should the amendment replace the original in analysis, or should both remain visible? | M. Reyes | 2026-08-14 | 2026-08-25 | Determines historization. Affects every quarter-over-quarter comparison |

Note the phrasing on Q5. The finding is that a source field is unreliable; the question asks what the business wants done about it. "Manager attribution field is unreliable" would be the same information delivered as a verdict on a page the analyst's management reads.

### Resolved

| # | Question | Answer | Answered by | Date |
|---|---|---|---|---|
| 3 | Are options positions in scope? Are short positions expected? | Options in scope and should be distinguishable from the underlying. Short positions are not disclosed in this filing type — research already knows this and does not expect them | M. Reyes | 2026-08-18 |
| 7 | Which teams consume this, and through what? | Research (ad hoc queries), risk (overlap analysis against managed portfolios), client reporting (quarterly security profiles, currently manual) | M. Reyes | 2026-08-18 |

## 6. Assumptions

| Assumption | Basis | Promote to question? |
|---|---|---|
| Coverage is US-listed equity holdings only | Source scope; not stated by the BA | No — determined by the source, not a business choice |
| Historical filings are in scope, not only new ones from go-live | Meeting reference to quarter-over-quarter comparison | **Yes — promoted to Q8's needed-by.** Changes initial load volume materially |
| Positions omitted under confidential treatment are acceptable as absent | Not addressed in any source | **Yes.** If completeness is expected, that expectation cannot be met and the BA needs to know before build |

## 7. Dependencies

| Dependency | Owner | Status |
|---|---|---|
| Instrument reference (identifier → security) | `<owner>` | Unconfirmed as required — see Q6 |
| Entity reference (manager identity across periods) | `<owner>` | Unconfirmed as required — see Q6 |
| Contract registration mechanism | Platform | Absent organization-wide. Recorded per readiness (a) |

The two reference dependencies are conditional on Q6 and are listed as unconfirmed rather than omitted, so that a reader planning sequencing can see they may appear.

## 8. Handoff

| Field | Value |
|---|---|
| Specification | Not yet authored |
| Handed off | — |
| Questions still open at handoff | 6 of 8 |

Two ratified rows — semantics and consumers — are ready to seed specification §1–§3. The six open questions carry into §9 with owner and date intact.

**Specification should not begin while Q2 is open.** Grain determines what the assets are; authoring §4 without it means the model fixes a business decision by implication rather than by the analyst deciding it.
