# Asset Change Proposal

A proposal to change one existing asset. Sub-page to a solution proposal, or standalone where a change needs no larger case around it.

## Why this is separate

**Different approver.** A solution proposal goes to whoever funds the work. An asset change goes to whoever owns the asset and to its consumers — a different audience, and one that cares about different things.

**Different lifespan.** The funding decision is made once. A schema proposal may be revised two or three times before it settles. Fusing them means each schema revision reopens a decision that was already made.

**One asset each.** Where a flow touches several assets, each gets its own. A second asset needing change later produces a second sub-proposal, not a mutated first one.

The parent proposal states *which* assets change, why, and who is affected. It **links** here rather than summarising — a summary and a detail page disagree eventually, and the summary is the one people read.

## It is a proposal, not a specification

The distinguishing content is **why each change is being made, what it enables, and what it costs.** Not what must be true of the result.

| Belongs here | Belongs in the specification |
|---|---|
| Why a field is being added, and what it enables | Its meaning, logical type, nullability |
| Why a key is being extended | The declared key and its uniqueness constraint |
| What versioning an attribute would cost | The historization behaviour |
| Which consumers are affected and how | The compatibility promise |
| Options the business must choose between | The decision, once made |

Field-level detail is expected here. Field-level *declarations* are not — once this is approved, `specification-authoring` declares the model against real data.

## Structure

```
# <Asset> — Proposed Change

| Field | Value |
|---|---|
| Asset | <name> |
| Owner | <team> |
| Parent proposal | <link> |
| Author | <name> |
| Date | <date> |
| Status | Draft / For decision / Decided |
| Approver | <named person or forum> |
| Decision needed by | <date, and what is blocked> |

## 1 · What is changing

Two or three sentences. What the asset is, what is being changed, and the extent —
minimal amendment, structural change, or replacement.

## 2 · Why

Which functional requirements in the parent proposal force this change, by ID.

| FR | What it requires | Why this asset must change |
|---|---|---|

Where a change is not driven by an FR, say what drives it. A change with no driver is
scope nobody asked for.

## 3 · Field-level changes

| Field | Change | Why | What it enables | Cost |
|---|---|---|---|---|
| <name> | Add / Alter / Remove | <what makes it necessary> | <what becomes possible> | <storage, write, query> |

**Every row states what it enables**, not only what it is. A field justified only by
"needed for tracking" cannot be evaluated by an approver.

**Removals and type changes are stated first** and marked breaking. They are the rows
consumers care about, and burying them below additions reads as concealment.

## 4 · Key and historization

| Field | Content |
|---|---|
| Current key | |
| Proposed key | |
| Why it changes | |
| Historization | <what versioning is proposed, on what> |
| What "current" means after this | |

**State what current means after the change.** A versioned key alters how every
current-state query must be written, and a reader who does not work that out will
plan against the old behaviour.

## 5 · Open design choices

Decisions the business must make within this change. Per the parent proposal's
section 8 discipline: consequence stated per option, and every choice carries a default.

### Choice <n> — <what is being decided>

| Option | Functional consequence | Cost |
|---|---|---|

| Field | Content |
|---|---|
| Default if not chosen | <required> |
| Who decides | |
| Needed by | |

## 6 · Consumers affected

| Consumer | How they are affected | Breaking | Notice required |
|---|---|---|---|

Where the asset is read by consumers outside the team, this section is what the change
is judged on. An empty row is not "none" — establish it and say so.

## 7 · Extent of change

Why this much and not more or less. Where the change deliberately works within an
existing structure rather than redesigning, record that as a decision — unstated, it
reads as an oversight.

## 8 · Open questions

| # | Question | Owner | Needed by | Blocks |
|---|---|---|---|---|

## 9 · Decision

Completed after, never in advance. Recorded in the decision log — see `decision-log.md`.
```

## Rules

- **One asset per proposal.** A proposal covering two assets has two approvers and no clear owner.
- **Link from the parent; never summarise into it.**
- **State what it enables, not only what it is.** An approver cannot evaluate a field described only by its name.
- **Breaking changes first.** Removals and type changes above additions.
- **Never write a change to an asset another team owns.** Raise it with them and record it as a dependency.
- **Do not declare the model.** Grain, constraints and assertions come with the specification.
