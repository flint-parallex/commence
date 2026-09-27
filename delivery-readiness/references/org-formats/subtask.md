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
| Prepare for release | Developer |
| UAT execution | Business |

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
