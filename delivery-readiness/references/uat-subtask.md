# UAT Subtask

The business validation step. Carried on **every story labelled `uat`**, and on no other story.

## When it exists

**The `uat` label creates this subtask.** Not a judgement made per ticket — the label already encodes the decision that someone outside engineering must confirm the behaviour before release (`org-formats/labels.md`). This subtask is what that decision produces.

A `dev test` story does not carry it. Adding one commits business time the label said was not needed.

**Changing the label after subtasks exist changes the subtask set.** `dev test` → `uat` adds this subtask; the reverse removes it. Say which, rather than leaving a stale subtask that reads as outstanding business validation.

## Who performs it

**The business owner who requested the outcome** — the named consumer on the story, or the person who raised the requirement.

Not the Product Owner, and not a developer. A PO validating on the business's behalf is the PO confirming their own translation of the requirement, which is the thing UAT exists to check independently.

Where the business owner is unavailable, UAT waits or a named delegate is recorded. Do not close it on a developer's confirmation.

## What makes it structured

The subtask does not say *confirm this works*. It restates **what the business asked for** as a set of checks they can run against the delivered thing.

Each check traces to something they requested. That is what makes a failure actionable: not *it doesn't look right*, but *this specific thing I asked for does not happen*.

**Derive the checks from the requirement, not from the implementation.** A check written from what was built validates that the build is self-consistent. A check written from what was asked validates that the build is what was wanted — and those diverge exactly where it matters.

## Template

```
Summary: UAT execution — <story summary> — <business owner>

Component: <parent's component>
Assignee: <business owner>
Environment: <uat>

Description:

What you asked for, as checks you can run. Confirm each against <where to access it>.

  | # | What you asked for | How to check it | Result | Notes |
  |---|---|---|---|---|
  | 1 | <requirement, in their words> | <the action they take> | PASS / FAIL | |

  Where a check fails, record what you saw rather than what you expected. The gap
  between the two is what gets investigated.

SCOPE

Out of scope for this validation: <what was deferred or excluded on the story>.

A check outside the story's scope is not a failure — raise it as a new requirement.

EXIT

  - Every check has a result
  - Any failure records what was observed
  - Confirmed in the environment the story targeted
```

**"How to check it" is written for someone who does not know the system.** A step naming a table or a query is a step the business owner cannot execute, and the subtask then gets completed by whoever can — which is not validation.

**Out of scope is stated.** Without it, a business owner finding something the story never promised records a failure, and the story cannot close on a gap that was agreed.

## Acceptance criteria

On the parent story.

```
- Ensure every requirement the business stated is represented as a check in the UAT subtask
- Ensure the business owner has recorded a result for every check
- Ensure any failure records what was observed
```

Three criteria. They reference the check table rather than restating the requirements — see `acceptance-criteria.md`.

## Rules

**The `uat` label creates this subtask. No label, no subtask.**

**The business owner performs it.** Not the PO, not a developer.

**Checks trace to what was requested**, not to what was built.

**Instructions are executable by a non-technical reader.** No table names, no queries.

**Out of scope is stated on the subtask**, drawn from the story.

**A failure records the observation, not the expectation.** What the business owner expected is already in the check; what they saw is the new information.

**UAT does not close the story alone.** It is one subtask in the ladder — MR review and the monitoring validation close independently. See `org-formats/subtask.md`.
