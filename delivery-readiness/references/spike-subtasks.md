# Spike Subtasks

The two subtasks every spike carries, and the body the spike itself must have.

Content per `spike.md`. This file covers the execution ladder under it.

## Why two

A spike closes on a **ratified** design, not a written one. Those are different events, performed by different people, and collapsing them into one subtask means the approval is whatever the author decided it was.

| Subtask | Party |
|---|---|
| Produce and socialise the design | The developer performing the spike |
| Approve the design and the recommended build | Tech lead or lead engineer |

**Neither is pruned.** A spike with no approval subtask has no record of who agreed to the design that the next several tickets will be built against.

## The spike body comes first

`spike.md` requires the investigation question, success criteria, timebox and the tickets unblocked. The subtasks execute against those — a subtask set with no spike body has nothing to answer and nothing to approve.

Where the body does not exist, write it first. Same rule as stories: see `org-formats/subtask.md`.

---

## Subtask 1 · Produce and socialise the design

**Assignee:** the developer performing the spike.

Four things, and the last two are where spikes stall.

```
Summary: Spike — produce and socialise design — <question> — <assignee>

Component: <parent's component>
Assignee: <developer>

Description:

  1 INVESTIGATE
    Answer the investigation question on the parent spike. Every question stated there,
    not a subset.

  2 DOCUMENT
    Write the design document and post it to the CDP Confluence space. Findings, the
    recommendation, and the rationale for it.

  3 SCHEDULE THE REVIEW
    Schedule a review session with the team. Scheduling it is part of this subtask —
    not an outcome that happens if a slot appears.

  4 HOLD THE REVIEW
    Walk the team through the document. Record attendance and what was raised.

CHECKPOINT

  Within 3 working days of pickup, something reviewable is posted to Confluence —
  per the parent spike's timebox.

EXIT

  - Every investigation question on the parent spike is answered in the document
  - The document is in the CDP Confluence space
  - The review session was held, with attendance recorded
  - Points raised in review are addressed in the document or recorded as open
```

**Scheduling is a step, not a hope.** A design document posted and never walked through is the most common way a spike reaches its deadline unratified. Putting the meeting in the subtask makes it someone's task rather than everyone's assumption.

**Answer every question, not the interesting ones.** A spike that answers three of four and recommends on that basis has recommended from partial ground.

---

## Subtask 2 · Approve the design and the recommended build

**Assignee:** tech lead or lead engineer. **Never the spike author.**

This is the one that carries weight. The approver is agreeing to what gets built next.

```
Summary: Spike — approve design and recommended build — <question> — <approver>

Component: <parent's component>
Assignee: <tech lead or lead engineer>

Description:

Approver: ___________    Date: ___________    Document: ___________

Confirm each. Where any is not met, do not approve — comment what is outstanding and
return to subtask 1.

  1 QUESTIONS ANSWERED
    Every investigation question on the parent spike is answered. Checked individually.

  2 ACCEPTANCE CRITERIA
    Every acceptance criterion on the parent spike is met.

  3 RECOMMENDATION
    The recommendation is sound, and its rationale is stated rather than implied.

  4 WHAT GETS BUILT
    Explicit approval of the recommended design as the basis for implementation.
    State what is being approved — not "approved" alone.

  5 ARCHITECTURAL SCOPE
    The design has been assessed for decisions above ticket scope, and classified as
    one of: conforms to an existing pattern / extends an existing pattern /
    establishes a new pattern. Where it extends or establishes, that is ratified here.

  6 UNBLOCKS
    The output enables the acceptance criteria of the tickets this spike blocks.

APPROVAL

Approving this subtask is a statement that the recommended design is what the team will
build. Where something is uncertain, comment rather than approving — the next several
tickets will be written against this.
```

**Condition 4 is the point of the subtask.** A review that confirms the document is thorough but never says *build this* leaves the decision unmade, and the first build ticket then carries an architectural choice nobody ratified.

**Condition 5 is the scope gate**, per `spike.md` and `decomposition.md` Mode 4. A design produced under one ticket must not silently set precedent for the rest of the initiative.

**Do not require the approver to create implementation tickets.** Decomposing a spike outcome into build work is the Product Owner's. See `spike.md`.

---

## Acceptance criteria

On the parent spike, referencing the subtasks rather than restating their conditions.

```
- Ensure every investigation question stated on this spike is answered in the design document
- Ensure the design document is posted to the CDP Confluence space
- Ensure the review session was held with the team
- Ensure the tech lead or lead engineer has explicitly approved the design and the
  recommended build
- Ensure the design's architectural scope is classified and, where it extends or
  establishes a pattern, ratified
```

## Rules

**Two subtasks, always.** Neither is pruned.

**The approver is never the author.**

**Approval names what is approved.** "Approved" alone is not approval of a design.

**A failed approval returns to subtask 1** with a comment naming what is outstanding. Subtask 2 stays open — reopening it later loses the record of what the first review found.

**The spike does not close on a written document.** It closes on a ratified one, per `spike.md`.

**Do not restate the parent spike's questions in the subtasks.** The subtask requires each is answered; the questions live on the parent.
