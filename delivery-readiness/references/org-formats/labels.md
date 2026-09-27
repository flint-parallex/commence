# Labels

The label vocabulary is strict. Labels are not descriptive tags — `uat` and `dev test`
route who tests the work, so a wrong label sends a ticket to the wrong party.

---

## Testing labels

Every story carries exactly one.

| Label | Meaning |
|---|---|
| `uat` | The **business** will test it |
| `dev test` | **Technical testing only.** No business involvement |

Choosing between them is a real decision, not a formality. Ask: does anyone outside the
engineering team need to confirm this behaves correctly before release? Backend work,
database analysis and API changes are `dev test`. Anything a business consumer perceives
is `uat`.

Where the answer is unclear, ask. Defaulting to `dev test` quietly removes a business
check; defaulting to `uat` quietly commits someone else's time.

## Other labels

| Label | When |
|---|---|
| `data quality` | The work's outcome is a data quality control |
| `spike` | The ticket is a spike. Always a task, never a story — see `task.md` |

## Quarter label

Required on every epic, per `epic.md`.

---

## Notes

**A story is never a spike.** If the work is a spike, it is a task with the `spike` label.
The label does not convert a story into one.

**Labels do not replace content.** A `data quality` label does not excuse a story from
stating its acceptance criteria, and a `spike` label does not excuse a task from stating
the question it answers.
