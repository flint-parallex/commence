# Task

Non-development work that does not require a release.

Tasks are tracked because they consume capacity. An untracked task does not make the work
free — it makes the Scrum team's allocation wrong.

Title per `../ticket-titles.md`.

---

## The test

**Does it require coding and a release?**

| Answer | Type |
|---|---|
| Yes | Story — see `story.md`, even if purely technical |
| No | Task |

This is the only test. Whether a consumer receives the outcome does not decide it.

---

## Required content

| Section | What it carries |
|---|---|
| **Purpose** | The what and the why. Why this is needed, not only what it is |
| **Acceptance criteria** | Per `../acceptance-criteria.md`. What must be true for this to be done |
| **Dev notes** | As on a story — inherited or unrefined detail, held until someone owns it |
| **Component** | Per `components.md`. Required |

A task with a clear objective and no stated purpose is the failure mode this format exists
to prevent. It passes grooming, consumes a sprint, and nobody can later say why it was
run.

---

## Examples the organization gives

- Design or impact assessments for an epic
- Upgrades to a technical component
- Analysis of a complex issue that will require follow-on work

The third is worth noting: a task whose output is *the discovery that more work is needed*
is legitimate here, and is the shape a spike takes.

---

## Spikes

There is no spike issue type. A spike ships nothing, so it is a **task carrying the
`spike` label and the `documentation` component**, authored per `../spike.md` — which governs the question being asked and
the declared output, and is stricter than this format.

`../spike.md` still applies in full: the question comes from what the team said they
needed to discover, the output type is declared, and the task is linked to the ticket it
blocks.
