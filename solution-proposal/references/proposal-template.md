# Solution Proposal — Template

Copy per proposal. Sections may be marked not-applicable; do not reorder or omit them.

The order is deliberate: business need, then evidence, then what is required, then how it could be done. A reader who stops after section 3 should still understand the problem and what it costs.

Sections 7 and 8 carry the decisions that are easiest to leave implicit: who owns what in the flow, and what the business must still choose.

---

# `<Title — the problem, not the solution>`

| Field | Value |
|---|---|
| Product | `<data product or system>` |
| OKR | `IP-OKR-###` |
| Author | `<name>` |
| Date | `<date>` |
| Status | Draft / For decision / Decided |
| Decision needed by | `<date, and what is blocked without it>` |
| Approver | `<named person or forum>` |

---

## 1 · Summary

Four to six sentences. What the business needs, what stands in the way, what is recommended, what it costs.

Written for someone who reads only this section. No schema, no table names.

## 2 · Business need

What the business needs to happen, in their language. Who needs it and what they do with it.

**Where this originates from an intake document, cite it** rather than restating.

| Business requirement | ID | Source |
|---|---|---|
| `<what the business needs>` | `BR-001` | `<intake document, section>` |

## 3 · Current state

What exists, and what it can and cannot do against section 2.

Every claim carries its observation, per `evidence-and-options.md`. Neutral about inherited work.

| Claim | Observation | Consequence |
|---|---|---|
| `<what cannot be done>` | `<measurement, table, column, date, volume>` | `<what this prevents, in business terms>` |

**Broken, missing, or both**

<!-- A capability that fails is a defect. One never built is absent. Say which. -->

**Local or systemic**

<!-- Does this failure affect other consumers of the same asset? Does this proposal fix
     it for them? State explicitly \u2014 silence will be read as yes. -->

**Inferences**

<!-- Anything concluded rather than observed, marked as such. -->

## 4 · Functional requirements

What any solution must be able to do. Per `functional-requirements.md`.

These survive whichever option is chosen, and they are what the specification is authored from.

| ID | Requirement | Traces from | Rationale | Release |
|---|---|---|---|---|
| `FR-001` | `<subject, capability, condition>` | `BR-001` | `<why \u2014 usually a cited current-state failure>` | `<R1 / Deferred>` |

**Demonstrated when**

| ID | What would show this is met |
|---|---|
| `FR-001` | `<observable condition, acceptance level>` |

## 5 · Options

Every option considered, including doing nothing. Assessed against section 4 by ID, never against each other.

### Option A — `<name>`

| Field | Content |
|---|---|
| What it is | |
| Meets | `<FR IDs>` |
| Does not meet | `<FR IDs>` |
| Cost | `<effort, elapsed, dependencies \u2014 supplied by the team, attributed>` |
| Risk | `<what could go wrong; what would be unrecoverable>` |
| Why not chosen | `<required unless this is the recommendation>` |

### Option B — `<name>`

<!-- Repeat. -->

### Option — Do nothing

| Field | Content |
|---|---|
| What the business does meanwhile | |
| What degrades | |
| Cost of continuing | |

## 6 · Recommendation

Which option, and why — in terms of the functional requirements it meets.

**What this gives up.** Required. A recommendation with no stated downside has not been examined.

## 7 · What changes

At the level of what, not how. No grain, no columns, no types.

| Change | Why | Affects |
|---|---|---|
| `<data model or schema change>` | `<which FR requires it>` | `<downstream consumers, if any>` |

**Consumers affected**

<!-- Anyone reading the current assets who must be told. Where a change is breaking for
     them, say so here rather than at delivery. -->

### Ownership boundary

Every asset and component in the flow, and who owns it. A flow that crosses ownership
boundaries has decisions nobody has been assigned, and they surface at delivery.

| Asset or component | Owner | Are we changing it | Do we depend on it | If it changes or is retired |
|---|---|---|---|---|
| `<pipeline>` | `<team>` | `<yes / no / minimally>` | `<yes / no>` | `<consequence for us>` |
| `<intermediate output>` | `<team>` | | | |
| `<downstream asset>` | `<other team>` | | | |

**Assets we do not own are dependencies, not scope.** Where an asset in the flow belongs
to another team, we cite it and we do not specify it. Where its continued existence is
in question, that is an open question with a named owner — not an assumption.

**State what we are not taking on.** An asset we touch but do not own will be assumed to
have become ours unless the proposal says otherwise. Silence defaults badly in both
directions: either someone plans on us maintaining it, or it stops being maintained by
anyone.

### Rationale for the extent of change

<!-- Where the change is deliberately minimal — building on an existing schema rather
     than redesigning — say so and say why. An unstated decision to work within what
     exists reads as an oversight, and a reviewer will ask why a proper redesign was not
     proposed. Record it as the decision it was. -->

`<why this extent of change and not more or less>`

## 8 · Open design choices

Decisions inside the recommendation that the business must make. **Not options** — options
in section 5 are mutually exclusive approaches where one is chosen and the rest are
rejected. These sit within the chosen approach, and the business picks from a menu.

They are recorded here because each has a functional consequence the chooser will not
infer. A business reader selecting an attribute to version will not, unaided, work out
what it does to row counts or to the meaning of *current*.

### Choice `<n>` — `<what is being decided>`

| Field | Content |
|---|---|
| Decision | `<what must be chosen>` |
| Affects | `<asset, and which FR it bears on>` |
| Default if not chosen | `<what happens with no decision — required>` |
| Who decides | `<named person or forum>` |
| Needed by | `<date, and what is blocked>` |

| Option | Functional consequence | Cost |
|---|---|---|
| `<option>` | `<what changes about behaviour, volume, or meaning>` | `<effort, storage, query impact>` |

**Consequence is stated per option, not per choice.** "This affects row counts" is not
usable; "versioning this attribute means a change to it creates a new version row, so
current-state queries must filter rather than select, and row counts grow with revision
frequency rather than with entity count" is.

**Every choice has a stated default.** A choice with no default blocks delivery when
nobody answers, and someone will pick one silently at build time.

## 9 · Scope

| | Content |
|---|---|
| **This release** | `<what is committed>` |
| **Deferred** | `<what is not, and what the business does meanwhile>` |
| **Still unsolved after** | `<what this does not address at all>` |

Deferred scope that is not written down is assumed to be included.

## 10 · Correctness commitment

What will be observably true when this works. Acceptance level, not assertions — assertions are declared in the specification against a built model.

| FR | Demonstrated when | Evidence at acceptance |
|---|---|---|
| `FR-001` | `<observable condition>` | `<what will be shown>` |

**How correctness will be maintained**

<!-- One or two sentences. Detail belongs in specification \u00a76. -->

## 11 · Dependencies

| Dependency | Owner | Status | Committed? |
|---|---|---|---|

**Nothing here commits another team.** A dependency is recorded with its owner and whether they have agreed — never as an assumption that they will.

## 12 · Traceability

The spine. Readable in both directions.

| Business requirement | Functional requirement | Change | Specification |
|---|---|---|---|
| `BR-001` | `FR-001`, `FR-004` | `<change>` | `<path, once authored>` |

**Unmet business requirements**

<!-- Any BR with no FR in this release, and what the business does meanwhile. -->

## 13 · Consequences of not proceeding

Stated without escalation. What continues to happen, what it costs, what becomes harder later.

## 14 · Open questions

| # | Question | Owner | Needed by | Blocks |
|---|---|---|---|---|

## 15 · Decision record

<!-- Completed after the decision, never in advance. -->

| Field | Value |
|---|---|
| Decision | |
| Decided by | |
| Date | |
| Conditions attached | |
| Specification authored | `<path>` |
