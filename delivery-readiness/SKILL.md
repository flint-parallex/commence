---
name: delivery-readiness
description: Writes and assesses delivery artifacts for the data engineering team — epics, stories, spikes, tasks — and decomposes canonical delivery documents into proposed ticket sets. Use when writing or refining a ticket, writing acceptance criteria, breaking a requirement into work, assessing whether backlog items are ready, or preparing for grooming. Triggers on phrasing like "write a ticket for", "break this down", "is this ready to build", or "prep for grooming" without an explicit request.
---

# Delivery Readiness

Gets work to the point where a developer can build it without coming back with questions, and where done can be verified without argument.

## Scope

**In:** epics, stories, tasks, spikes, decomposition of canonical delivery documents, readiness assessment.

**Out:** specifications, data models, assertions, monitors — `specification-authoring`. Test plans, document assessment, check placement — `delivery-assessment`. Rendering and posting — `delivery-communication`.

The test: does this artifact **enter** the delivery pipeline, or **describe** it?

## What a ticket carries

**A functional requirement and its verification. Nothing else.**

State what must be true when the work is done. Never how to make it true.

| Belongs in a ticket | Does not |
|---|---|
| The functional outcome — what changes for whom | Transformation logic, DDL, join or incremental strategy |
| Acceptance criteria, each testable | Materialization, partitioning, orchestration, tuning |
| Expected schema, accepted values, sample payloads | Which library or function to call |
| The assertion ID the criterion verifies | The implementation of that assertion |
| A worked edge case where the rule is non-obvious | A walkthrough of how to handle it |

The test: **if the detail would change when the implementation changes, it does not belong in the ticket.**

Technical detail is permitted only where it specifies what correct output looks like. That is the whole exception.

## Voice

**Neutral.** Dependency status, gaps and blockers stated as fact, never as blame. Holds in private outputs too.

**Concise.** No preamble, no restating the requirement back, no narrating. Length is not the target — a ticket is as long as the work needs. What is cut is repetition, not detail.

**Non-redundant.** Every fact appears once. Where a table carries the specifics, criteria reference the table rather than restating it — a value written twice diverges on the first edit, and the ticket then contradicts itself with no way to tell which half is current. See `acceptance-criteria.md`.

**Precise.** Name the table, column, threshold, value. A ticket that could apply to any table is not specific enough.

## Core principles

These are the rules this file owns. Everything else is owned by a reference — go there rather than restating it here.

**Ask, never invent.** Tables, grain, keys, dates, thresholds, names. An invented specific is worse than a stated gap.

**Link, never copy.** Definitions of Done, OKR pages, specifications have canonical homes. Reference them. If a link is not supplied, ask.

**One standard, one home.** `acceptance-criteria.md` owns AC quality. `ticket-titles.md` owns titles. `org-formats/` owns Jira shape. Never restate a rule another reference owns.

**Never duplicate.** Epic to story, parent to subtask, table to criterion, ticket to reference — state it once, point at it from everywhere else. This applies *within* a ticket as strictly as between tickets.

**Cite the specification at a commit, never a branch.** Otherwise the ticket silently changes meaning and nobody can tell what was built against.

**Never invent business context.** Business problem and impact come from a business source. Technical inputs only means stop and ask — see `epic.md`.

**Never assign ticket-writing to an engineer.** Decomposing outcomes into build work is the Product Owner's.

## Two separate decisions

Conflating these is the common error.

**1 · What is the work unit?** Decided at decomposition, by where a grain assertion can be evaluated. A unit delivering a consumer-facing outcome differs from one required only by a later unit — that determines what the ticket says. See `decomposition.md`.

**2 · What Jira issue type is it?** Decided by the release test: coding plus a release is a **story**, in full story format, even with no business input and no UAT. Shipping nothing is a **task**. A spike is a task carrying the `spike` label — there is no spike issue type. See `org-formats/`.

These axes are independent. A technical unit with no consumer is still a story if it ships. Do not derive issue type from who receives the outcome.

**Readiness is a third question.** If the approach must be designed and ratified before anyone builds, the work is not ready in any form — it is a spike declaring a design document as its output, closed per `decomposition.md` Mode 4. Difficulty is not the test; whether the approach is one person's call is.

## Lifecycle

| Step | Reference |
|---|---|
| A canonical delivery document arrives; propose the ticket set | `decomposition.md` Mode 1 |
| Write the tickets against the approved proposal | `epic.md`, `user-story.md`, `task.md`, `spike.md` |
| Requirements keep arriving; report what changes | `decomposition.md` Mode 2 |
| Assess the backlog before grooming | `backlog-readiness-assessment.md` |
| An unknown surfaces in grooming; write the spike | `decomposition.md` Mode 3 |
| The spike returns; reshape the blocked ticket | `decomposition.md` Mode 4 |

Propose, do not write, until approved. Never regenerate a set that already exists.

## Procedure

1. Identify the work unit and the archetype. Ask if unclear.
2. Read the matching reference in full, plus `acceptance-criteria.md` for any ticket carrying criteria.
3. Collect required inputs. Ask for anything missing, including canonical links.
4. Write the artifact — functional requirement and verification only.
5. Assess against `definition-of-ready.md`. Report Ready / Partial / Not ready, naming each unmet condition and its owner.
6. Render into Jira shape per `org-formats/`, as a separate pass. A required field with no source is an open question, never a filled value.
7. Where the story delivers or changes a dataset, add the monitoring subtask and its validation pair per `monitoring-subtask.md`. Not optional, not pruned for small datasets.

## References

- `acceptance-criteria.md` — AC quality. Read for every ticket carrying criteria
- `definition-of-ready.md` — the intake gate
- `decomposition.md` — four modes: propose, refine, spike from grooming, close spike
- `epic.md` · `user-story.md` · `task.md` · `spike.md` — archetypes
- `monitoring-subtask.md` — the observability and monitoring subtask, and its validation pair
- `ticket-titles.md` — how every title is written
- `backlog-readiness-assessment.md` — assessment and grooming communication
- `example-work-set.md` — a worked decomposition
- `org-formats/` — the organization's Jira shapes. A render pass, never merged into the archetypes
- `../delivery-communication/references/confluence-formatting.md` — rendering, owned by that skill

## The line this skill holds

The Product Owner defines **what** is built and **how it is verified**. Engineering decides **how**.

Where a requirement carries an architectural decision, it is routed for ratification — never absorbed into a ticket.
