# MR Review Subtask

The code review step. Carried on **every** story, without exception.

## Why it is a subtask and not a status

A review that lives in a Jira status is a gate nobody owns. As a subtask it has an assignee, a description of what was checked, and a comment record — which means the review is a thing that happened to a named person rather than a transition somebody clicked.

**It is the second pair of eyes on work that is about to reach production.** Everything in it is phrased as something the reviewer attests to, not something they glance at.

## Assignment

**Created unassigned.** Whoever performs the review assigns it to themselves when they pick it up.

Not pre-assigned, because the available reviewer is not known when the subtask is created, and a pre-assigned review either waits for one person or gets silently done by someone else with the record still naming the first.

**The reviewer is not the author.** A story whose only available reviewer wrote the code does not get reviewed by them — it waits, or the tech lead reviews it. Say so rather than closing the subtask.

## What the reviewer attests to

Five things. Each is a claim about the work, made by the reviewer, recorded in the subtask.

| | What is attested |
|---|---|
| **1 · Acceptance criteria** | Every criterion on the parent story is met. Checked individually against the parent, not assumed from the build subtask being done |
| **2 · Functional** | The code does what the story says. Run it, or review the author's evidence that it ran — do not infer from the diff |
| **3 · No side effects** | Nothing else in the system changed behaviour. Shared components, upstream assets, downstream consumers, other pipelines reading the same tables |
| **4 · Engineering standards** | The implementation follows the standards defined by the tech lead |
| **5 · Runbooks** | Runbooks for this work unit are created or updated, and reflect what was actually built |

**Side effects are the one that gets skipped**, because a diff shows what changed and not what else is affected. Where a shared component was touched, the reviewer names what else consumes it and whether it was considered.

**Runbooks are part of the review, not a follow-up.** A runbook written after the reviewer has moved on is written from memory by whoever remembers. Unwritten, the next incident is handled by whoever happens to know.

## Template

```
Summary: MR review — <story summary>

Component: <parent's component>
Assignee: UNASSIGNED — the reviewer assigns themselves on pickup

Description:

Reviewer: ___________    Date: ___________    MR / PR: ___________

Confirm each. Where any is not met, do not approve — comment what is outstanding and
return the story.

  1 ACCEPTANCE CRITERIA
    Every criterion on the parent story is met. Checked individually against the parent.

  2 FUNCTIONAL
    The code does what the story describes. State how this was established — run
    locally, pipeline executed, author's evidence reviewed.

  3 NO SIDE EFFECTS
    Nothing else in the system changed behaviour. Name any shared component touched and
    what else consumes it.

  4 ENGINEERING STANDARDS
    The implementation follows the standards defined by the tech lead.

  5 RUNBOOKS
    Runbooks for this work unit are created or updated and match what was built.

APPROVAL

Approving this subtask is a statement that all five hold. Where something is uncertain,
comment rather than approving — an approval with a caveat in the comments reads as an
approval to everyone downstream.
```

## Acceptance criteria

```
- Ensure the reviewer is named and is not the author
- Ensure all five review conditions are confirmed in the subtask
- Ensure every acceptance criterion on the parent story is individually confirmed
- Ensure runbooks for this work unit are created or updated
```

Four criteria. They reference the five conditions rather than restating them — see `acceptance-criteria.md`.

## Rules

**Every story carries this subtask.** Not pruned for small changes. A one-line change that reaches production is reviewed.

**Created unassigned; the reviewer self-assigns.**

**The reviewer is never the author.**

**Approval means all five hold.** A conditional approval is not an approval. Comment and return the story.

**Do not restate the parent's acceptance criteria here.** The subtask requires that each is confirmed; the criteria live on the parent.

**A failed review returns the story**, with a comment naming what is outstanding. The subtask stays open — reopening it later loses the record of what the first review found.
