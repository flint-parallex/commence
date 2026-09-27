# Enabler Task

Work required for a later unit that delivers no consumer-facing outcome.

This file governs **what a task must say**. It does not govern field layout, ordering, or
naming — those come from the organization's required format, applied last. Content
discipline here; shape there.

> **This file does not decide the Jira issue type.** The organization splits story from
> task on whether the work requires coding and a release, not on whether a consumer
> receives the outcome. An enabler that ships code is a **story** in Jira. See
> `org-formats/README.md`. The enabler framing below still decides whether a unit is real,
> justified work — it just does not name the issue type.

---

## The governing rule

**A task states what must exist when it is done. Not how to build it.**

The absence of a consumer is not a lower bar. It is a different client: the unit that
cannot start until this one finishes. That unit is named, and it is what makes the task
justified.

`user-story.md` forbids the engineer as the user. A task does not evade that rule by
dropping the user frame — it satisfies it by naming the real dependent instead.

---

## The justification test

**A task with no dependent unit is not an enabler. It is speculative work.**

If nothing is blocked by it, it does not belong in this work set yet. Name the unit or
units that cannot start without it. If that list is empty, say so and stop — an enabler
nobody is waiting on is either premature or belongs to a different initiative.

This is the task's equivalent of "no consumer means not a story," and it fails the same
way when skipped: the ticket looks legitimate, passes grooming, and consumes a sprint on
something nothing needed yet.

---

## Required content

| | |
|---|---|
| **Title** | Per `ticket-titles.md`. Names the thing, six words, fifty-five characters |
| **What must exist at completion** | The end state, stated as a property of the system. Not a list of steps |
| **Dependent units** | Every unit blocked by this one, linked. At least one, per the justification test |
| **Specification reference** | The section that sourced this. Link, never restate |
| **Acceptance criteria** | Per `acceptance-criteria.md`. A task carries AC like any other ticket |

Nothing else is required. A task that needs a long description is usually two tasks, or a
task whose approach is not yet determinate — see below.

---

## Determinate, or not yet a task

A task is determinate work: the end state is known, and a competent engineer can reach it
without a decision being ratified first.

Where the approach must be **designed and agreed** before anyone builds — not merely
chosen at the keyboard — the work is not a task yet. It is a spike whose declared output
is a design document, per `spike.md`. The design is reviewed, the spike closes per
Mode 4 in `decomposition.md`, and the build ticket is reshaped from what the design
established.

The distinction is not difficulty. It is whether the approach is one person's call.

| | |
|---|---|
| Add a column to an existing load, grain unchanged | Task |
| Backfill a table from a stated source and window | Task |
| Choose how history is carried across a set of assets | Spike → design → build ticket |
| Establish a pattern the rest of the initiative will follow | Spike → design → build ticket |

The last row is the one most often mis-shaped as a task. Work whose output is a *precedent*
needs ratification, because everything after it inherits the decision.

---

## Never

- **Never invent a user** to make a task look like a story
- **Never name the implementation approach.** The tool, the pattern, the library, the
  retry logic — the developer decides. See `../specification-authoring/SKILL.md` on the
  same line in specifications
- **Never restate the epic or the parent.** Reference it
- **Never bundle unrelated enablers** to keep the ticket count down. Two dependents with
  nothing in common are two tasks
- **Never copy acceptance criteria out of the specification.** Link to the section and the
  assertion IDs. A copied criterion is a second source and drifts on the first edit
