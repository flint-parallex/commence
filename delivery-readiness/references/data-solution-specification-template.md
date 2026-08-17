<!--
Data Solution Specification — Template v0.1
Copy this file to specs/<solution-name>/specification.md and fill it in.
Delete these HTML comments as you go; they are guidance, not content.
-->

# Data Solution Specification — `<solution-name>`

<!--
HOW THIS TEMPLATE WORKS

Every field exists because a Definition of Ready criterion requires it. A completed
specification is a ready specification. Completeness IS readiness — there is no
separate review pass.

There is no field for implementation, by design. Nowhere in this template is there a
place to write a join type, the order operations are applied in, a deduplication
strategy, materialisation, partitioning, clustering, incremental strategy,
orchestration, scheduling or tuning. If you find yourself wanting to write one, it
belongs to engineering. This document states what must be true of the result;
engineering decides how the result is produced.

Reference the standards; never restate them. Link to the Definition of Ready and
Definition of Done. Copying them in creates two sources that drift apart.

The binding column stays blank. You write the assertion. Engineering writes where the
check lives. You verify the binding exists at acceptance.
-->

| Field | Value |
|---|---|
| Data Solution | `<name>` |
| Version | `v0.1` |
| Author | `<name>` |
| Date | `<YYYY-MM-DD>` |
| Written against | Definition of Ready `<version>` — `<link>` |
| Status | Draft / Ready / Delivered |

---

## 1 · Solution Definition

**Purpose**

<!-- One paragraph, business terms. What this Solution is for. Do not name a table,
     pipeline or system. If a reader outside the delivery team can't follow it, rewrite. -->

`<purpose>`

**Business entity and grain**

<!-- The single entity this Solution represents, and what one row means at Solution level.
     One Solution, one entity. If you need two, you have two Solutions. -->

`<entity and grain>`

**Owner**

<!-- Named accountable person. Not a team alias alone. -->

`<name>`

**Support channel**

`<where a consumer raises a problem>`

**Consumers**

<!-- The most-omitted and most-regretted section. Without it, the Solution breaks
     silently the moment a second consumer arrives, and fitness cannot be assessed. -->

| Consumer | Their stated use |
|---|---|
| `<name / team>` | `<what they do with it>` |

---

## 2 · Service Commitments

**Required refresh cadence**

<!-- What consumers need. NOT the schedule that achieves it. -->

`<cadence>`

**Service levels**

<!-- The SLO is what we hold ourselves to. The SLA is what we publish. The SLO sits
     tighter — the gap is the room to detect and correct before a promise breaks. -->

| Measure | Target (SLO) | Published promise (SLA) |
|---|---|---|
| Freshness | | |
| Availability | | |
| Timeliness | | |

**Scale and cost**

<!-- Stated as requirements. Never as tuning guidance. -->

| Dimension | Expectation |
|---|---|
| Expected volume | |
| Growth trajectory | |
| Query patterns | |
| Cost envelope | |

---

## 3 · Sources and Dependencies

**Sources**

| Source | Contract reference | Stability | Notes |
|---|---|---|---|
| | | | |

**Canonical references**

<!-- For every lookup or reference dataset this Solution depends on, name the
     authoritative source. Engineering is never left to choose which is canonical. -->

| Reference | Authoritative source | Governed by |
|---|---|---|
| | | |

**Upstream change behaviour**

<!-- What this Solution must do when a source changes shape, semantics or availability.
     Quarantine, fail closed, last known good — stated as a required OUTCOME. -->

`<required behaviour>`

**Shared and conformed dimensions**

<!-- Which shared entities this Solution uses. Referenced, never redefined. -->

`<shared entities>`

---

## 4 · Data Assets

<!-- Repeat this whole section per Data Asset. -->

### Asset: `<asset-name>`

**Grain**

<!-- One row is exactly what. Every aggregation downstream rests on this sentence. -->

`<one row = ...>`

**Columns**

<!-- "Meaning" is the field only this seat can supply, and it is the content the catalog
     entry is written from later. Do not leave it to be inferred from the column name. -->

| Column | Logical type | Meaning | Units | Nullable | Classification |
|---|---|---|---|---|---|
| | | | | | |

**Keys and uniqueness**

`<keys>`

**Relationships**

<!-- Cardinality and its implication are in scope. The join that implements it is not. -->

| From | To | Cardinality | Implication for the result |
|---|---|---|---|
| | | | |

**History behaviour**

<!-- Which changes overwrite and which accumulate. -->

`<history behaviour>`

**Breaking change classification**

<!-- Which changes to this asset break consumers. Column removed, renamed, retyped, or a
     constraint removed or tightened are breaking. Additive change is not. -->

`<classification>`

---

## 5 · Build Sequence

<!--
THE INTENT LAYER. This is the part no standard covers and no tool produces.

Three fields per stage, none of which can hold an implementation. If "required end
state" reads like SQL, rewrite it as a statement about the data.

Write this section as though the person maintaining it in two years has never spoken
to you.
-->

### Stage 1 — `<name>`

| Field | Content |
|---|---|
| Required end state | `<what must be true of the data after this stage>` |
| Reason | `<why this rule exists — the business justification, not the mechanism>` |
| Expected effect | `<what this stage changes: records removed, values derived, rows added>` |
| Dependency | `<which stage or asset must complete first>` |

### Stage 2 — `<name>`

| Field | Content |
|---|---|
| Required end state | |
| Reason | |
| Expected effect | |
| Dependency | |

**Filter chain summary**

<!-- The full sequence in order, readable at a glance. -->

| Stage | Removes / derives | Reason |
|---|---|---|
| | | |

---

## 6 · Assertions

<!--
Three separate sections. Never one combined table — they run at different times,
against different inputs, and are authored and owned differently.

EVERY foreseeable hazard appears here as an assertion, not as a note elsewhere.
"Fan-out expected from the one-to-many relationship" is a warning someone can miss.
"Output unique on filing identifier" is a gate.

Expected values come from the specification. Never from production data, and never from
what the pipeline currently outputs.
-->

### 6a · Unit assertions

<!-- Logic tested against static inputs. Runs in CI before anything is materialised. -->

| Assertion | Severity | Binding *(engineering)* |
|---|---|---|
| | Blocking / Non-blocking | |

### 6b · Data assertions

<!-- Evaluated against real data after the asset builds. -->

| Assertion | Severity | Binding *(engineering)* |
|---|---|---|
| | Blocking / Non-blocking | |

### 6c · Runtime monitors

<!-- Production observation. Cannot block a merge. Where a correctness rule appears in
     both 6b and 6c, both instances trace to the specification. Observed history is
     never used as a test oracle. -->

| Assertion | Threshold source | Binding *(engineering)* |
|---|---|---|
| | Specification / Observed history | |

---

## 7 · Governance and Lifecycle

**Classification and sensitivity**

`<Solution level; per column in section 4>`

**Compatibility promise**

<!-- Which changes consumers may rely on us NOT making without a version.
     backward   — additive only; existing consumer code keeps working
     forward    — old data works with new consumer code
     full       — both
     transitive — the promise holds against every prior version, not just the last -->

`<backward | forward | full | transitive>`

**Versioning and deprecation**

<!-- How a version is introduced, how one is retired, and the notice consumers are owed. -->

`<policy and notice window>`

**Limitations and approved use**

<!-- What a Solution is not for is part of what it is. -->

| Approved for | Not approved for | Known gaps |
|---|---|---|
| | | |

**Change notification**

`<how consumers learn of change before it reaches them>`

**Retirement outline**

`<how this would be deprecated, who is notified, what depends on it>`

**Catalog trust status**

`<target status on delivery: certified | deprecated>`

---

## 8 · Catalog Metadata

<!-- What the catalog entry must say on delivery. Descriptions come from section 4. -->

| Field | Value |
|---|---|
| Asset descriptions | *see section 4* |
| Column descriptions | *see section 4* |
| Classification tags | |
| Ownership | |
| Certification target | |
| Domain | |

---

## 9 · Open Items — Routed to Engineering

<!--
Questions above ticket scope. Recorded and dated. NOT answered here.

Severity per assertion is declared in section 6. The mechanism that implements it
establishes precedent across pipelines and is engineering's to decide. Record the
question; never propose the answer.
-->

| Item | Raised | Owner | Status |
|---|---|---|---|
| Handling mechanism for records failing a blocking assertion — quarantine, halt, or warn and continue | `<date>` | Engineering | Open |

---

## 10 · Readiness

<!-- Twenty-five completed fields can still describe something nobody can build from.
     These three are what the fields serve. -->

| Gate | Yes / No |
|---|---|
| No clarifying question is required to begin building | |
| Someone who did not write this could maintain the product from it | |
| This agrees with its neighbours on the platform | |

---

## Provenance

Authored by `<name>`. Written against Definition of Ready `<version>`. Committed `<date>`.

<!--
NOTES ON USE

Freeze before you improve. This template is v0.1. Extend it when a real Solution needs
something it does not carry — not before. A form that is canonical because it is fixed
beats a better form still under revision.

Two products, two specifications. One Data Solution per document. A specification
covering two Solutions organises around a source system instead of around a product,
which is the wrong shape.

Section 5 is where the value is. Sections 1–4 and 7–8 exist in some form in open
standards. The build sequence and its reasoning exist nowhere and are produced by
nobody else.
-->
