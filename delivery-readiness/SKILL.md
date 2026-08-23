---
name: delivery-readiness
description: Writes and assesses delivery artifacts for the data engineering team — epics, user stories, spikes — and decomposes canonical delivery documents into proposed ticket sets. Use when writing or refining a ticket, writing acceptance criteria, breaking a requirement into work, assessing whether backlog items are ready, or preparing for grooming. Triggers on phrasing like "write a ticket for", "break this down", "is this ready to build", or "prep for grooming" without an explicit request.
---

# Delivery Readiness

Everything required to get work to the point where engineering can build it.

The job is not producing documents. It is producing work that a developer can pick up and build without coming back with questions, and that can be verified as done without argument. Every rule here serves that.

## Scope

**In:** epics, user stories, spikes, enabler tasks, catalog comments, decomposition of canonical delivery documents, readiness assessment, test design.

**Out:** sprint plans, quarterly plans, burndown, velocity, status reporting, stakeholder-facing communications — `delivery-communication`. Authoring or revising a specification, data model, grain, column semantics, build sequence or assertions — `specification-authoring`.

The test: does this artifact **enter** the delivery pipeline, or **describe** it?

## Voice

Every artifact this skill produces is written in the same register.

**Neutral.** State dependency status, requirement gaps, and blockers as fact. Never as a blame narrative. "Upstream contract pending from the platform team" — not "blocked because platform hasn't delivered." This holds even in private outputs.

**Brief.** No padding, no preamble, no restating the requirement back, no narrating the process. A long ticket slows delivery as surely as a vague one.

**Precise.** Name tables, columns, thresholds, times, channels, values. A ticket that could apply to any table is not specific enough.

**Professional throughout.** These artifacts are read by engineers, stakeholders, and leadership. They are the operator's work product.

## Core principles

**Ask, never invent.** Table names, grain, sources, keys, dates, thresholds, stakeholder names — if it is not provided, ask. An invented specific is worse than a stated gap.

**Link, never copy.** The Definition of Done, OKR pages, sprint pages, and data model specifications have canonical homes. Reference them; never hard-code a URL and never restate a standard that lives elsewhere. If a canonical link is not supplied, ask for it rather than inventing or omitting it.

**One standard, one home.** This applies to the references themselves. `acceptance-criteria.md` owns AC quality; every other reference points at it. Do not restate a rule that another reference owns.

**Tie work to an OKR.** Every ticket carries its OKR label (IP-OKR-###). Ask for it rather than omitting or inventing it.

**Declare the target gate.** UAT, prod, or spike. This determines which DoD gate applies. Default to UAT if unspecified.

**Publish in storage format.** Anything posted to Confluence is converted to Confluence storage format per `../delivery-communication/references/confluence-formatting.md`, using the appropriate macros. Never post raw Markdown.

**Never invent business context.** Business problem and business impact sections on initiatives and epics come from a business source. If the only inputs are technical, stop and ask — see the business context gate in `initiative-summary.md` and `epic.md`.

**Outcome-framed titles.** What the work achieves, not the activity.

**Dependencies are Jira links.** Native issue links with the correct type — `is blocked by` upstream, `blocks` for what this enables, `relates to` for reference. Listed separately from the body, never written into the description as prose.

**Never duplicate.** Epic to story, parent to subtask, ticket to reference — state it once, in the place that owns it, and point at it from everywhere else.

**Never assign ticket-writing to an engineer.** Decomposing outcomes into build work is the Product Owner's responsibility.

## How the pieces work together

The lifecycle, and which reference governs each step:

1. **A canonical delivery document arrives** from `specification-authoring` → `decomposition.md` Mode 1 proposes the ticket set. Propose, do not write, until approved.

2. **Tickets are written** against the proposal → `epic.md`, `user-story.md`. All acceptance criteria per `acceptance-criteria.md`.

3. **Requirements keep arriving** — by email, in conversation, out of grooming → `decomposition.md` Mode 2 reports what changes. Never regenerate the set.

4. **Before grooming**, the backlog is assessed → `backlog-readiness-assessment.md` applies `definition-of-ready.md` and produces both the private assessment and the team communication.

5. **In grooming, an unknown surfaces** → `decomposition.md` Mode 3 writes the spike from the transcript, anchored to what the developers actually said they needed to determine.

6. **The spike returns** → `decomposition.md` Mode 4. Two gates: the design carries no unratified architectural decision, and every spike criterion is met. Only then does the blocked ticket refine in place or decompose.

The through-line: **nothing enters a sprint unassessed, and nothing sets architectural precedent silently.**

## Choosing the archetype

| The work | Archetype | Reference |
|---|---|---|
| Spans multiple epics, delivers a business capability, ties to OKRs | Initiative summary | `initiative-summary.md` |
| Spans multiple tickets, delivers a named dataset, has its own stakeholders | Epic | `epic.md` |
| Delivers a functional change a consumer receives | User story | `user-story.md` |
| Applies supplied dataset and column descriptions to catalog objects | Catalog comments | `catalog-comments.md` |
| Answers a question the team cannot proceed without | Spike | `spike.md` |

If unclear, ask. Three boundary cases worth knowing:

- **Purely internal engineering work with no consumer-facing outcome** is a task, not a story. Do not force a story frame onto it.
- **Undecided implementation approach** means the work is not ready as a story. It is a spike.
- **Runtime monitors do not gate a merge.** They are a deliverable of the unit, verified post-deploy. Split them by threshold source per `decomposition.md` — monitors covered by out-of-the-box tooling get no ticket.

## Procedure

1. Identify the archetype. Ask if unclear.
2. Read the matching reference in full before writing anything.
3. Read `acceptance-criteria.md` for any ticket carrying AC.
4. Collect the reference's required inputs. Ask for anything missing.
5. Ask for any canonical Confluence link the artifact must reference.
6. Write the artifact.
7. Assess against `definition-of-ready.md` before finalizing. Report Ready / Partial / Not ready, naming any unmet condition and its owning party.

## References

- `acceptance-criteria.md` — the AC quality standard. Read for every ticket carrying criteria
- `definition-of-ready.md` — the intake gate, universal and type-specific conditions
- `decomposition.md` — four modes: propose, refine, spike from grooming, close spike
- `initiative-summary.md` · `epic.md` · `user-story.md` · `spike.md` · `catalog-comments.md` — archetypes
- `backlog-readiness-assessment.md` — backlog assessment and grooming communication
- `test-design.md` · `check-authoring.md` · `capability-test-plan.md` — assertion and test authoring
- `../delivery-communication/references/confluence-formatting.md` — storage format and macro usage for anything published to Confluence

## The line this skill holds

The Product Owner defines **what** is being built and **how it will be verified**. Engineering decides **how** it is built.

Never write implementation approach into a ticket. Technical detail belongs in a ticket only where it specifies what correct output looks like — expected schema, sample payloads, the assertion that is the acceptance check, accepted values, a worked edge case. Never how to build it: transformation logic, DDL, materialization structure, join or incremental strategy, orchestration, tooling, tuning.

The test: **if the detail would change when the implementation changes, it does not belong in the ticket.**

Where a design or requirement carries an architectural decision, it is routed for ratification — never absorbed into a ticket.
