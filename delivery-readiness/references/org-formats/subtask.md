# Subtask

The execution ladder under a story. Each responsible party's work is its own subtask.

Subtasks are created when the ticket moves **Open → Backlog**.

---

## The standard ladder

Most build stories decompose to some subset of these. Propose the set, prune what does not
apply, and do not invent stages that no party executes.

| Subtask | Party |
|---|---|
| Implement the build | Developer |
| Add observability and monitoring | Developer |
| Validate the build implementation | Developer |
| Validate the observability and monitoring implementation | Developer |
| MR review | Unassigned — the reviewer self-assigns |
| Prepare for release | Developer |
| UAT execution | Business |

**A subtask never exists without its parent body.** Subtasks carry the execution steps;
the parent carries the acceptance criteria that close the ticket. A subtask set with no
parent has no stated completion condition and nothing to review against. Where the parent
does not exist yet, write it first — the full body, not a placeholder summary.

### Always carried, never pruned

| Subtask | Template | Applies to |
|---|---|---|
| Add observability and monitoring | `../monitoring-subtask.md` | Every story delivering or changing a dataset |
| Validate the observability and monitoring implementation | `../monitoring-subtask.md` | The same |
| MR review | `../mr-review-subtask.md` | **Every story**, without exception |
| UAT execution | `../uat-subtask.md` | Every story labelled `uat`. Never on `dev test` |
| Produce and socialise the design | `../spike-subtasks.md` | **Every spike** |
| Approve the design and the recommended build | `../spike-subtasks.md` | **Every spike** |

The monitoring pair covers three layers: Datadog pipeline observability, the three
out-of-the-box monitors, and the data quality rules declared in specification §6.

MR review is created **unassigned** — whoever performs the review assigns it to themselves.
The reviewer is never the author.

A spike carries two subtasks rather than one: producing the design and **approving** it are
different events performed by different people. The approver is the tech lead or lead
engineer, never the spike author.

**The `uat` label creates the UAT subtask.** It is not a per-ticket judgement — the label
already encodes the decision that the business must confirm the behaviour
(`labels.md`). Changing the label changes the subtask set: `dev test` → `uat` adds it, the
reverse removes it. Say which, rather than leaving a stale subtask that reads as
outstanding business validation.

Two separate validation subtasks, because the build and its monitoring are validated
independently. A monitor that was never verified is the failure this split exists to
catch — and it is where the specification's §6c monitors and their Monte Carlo bindings
get confirmed.

---

## Required content

| Field | What it carries |
|---|---|
| **Summary** | What the subtask is, plus the assigned party. `UAT execution` for the assigned developer |
| **Description** | The test steps the responsible party executes |
| **Attachments** | Excel and PowerPoint templates laying out test cases, with images and tests |
| **Component** | Defaults to the parent's component |
| **Assignee** | The responsible party |
| **Comments** | Testing results, one entry per round |

---

## The description is frozen

**The description should not be edited once development has begun,** except where
absolutely needed.

It is decomposed by the PM and PDLs working together, before development starts. That is
what makes it a stable target to test against. A description edited mid-development means
the thing being tested moved while it was being tested, and no result from before the edit
means anything.

Where a change is genuinely required after development begins, say what changed and why,
rather than editing silently.

---

## Comments carry the test record

For each round or instance of testing, add a comment containing:

- Execution date
- Failed steps
- Actual results
- Accepted screenshots

Each round is its own comment. Overwriting the previous round's result destroys the
history of what failed and when, which is the only evidence that a retest was a retest.
