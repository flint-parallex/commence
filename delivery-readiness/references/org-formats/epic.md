# Epic

The organization's epic is a **charter**, not a feature container. It carries the business
case, the measure of success, and the resourcing — closer to a project brief than to the
agile convention of bundling a body of consumer work.

Authored per `../epic.md` for content where the two overlap. Title per
`../ticket-titles.md`.

---

## Required content

| Field | What it carries |
|---|---|
| **Summary** | High-level description. What this is, in a few sentences |
| **Business problem** | What is wrong today that justifies the work |
| **Business impact** | What the problem costs, or what solving it is worth |
| **Success criteria** | How success will be measured. Must be checkable after delivery, not a restatement of the goal |
| **Deadline** | If one exists. Absent is a valid answer; invented is not |
| **Stakeholders** | Who is involved and in what capacity |
| **Resource requirements** | For requirements **and** for testing. Both, named separately |

## Required fields

| Field | Value |
|---|---|
| **Quarter label** | Always. The epic is not complete without the quarter it belongs to |
| **Assignee** | The Product Owner |
| **Component** | The delivery **team**, which routes the epic to that team's board. Per `components.md` — note this is the team, not the per-product technical component that appears on tickets |

---

## Notes

**Success criteria are not acceptance criteria.** Acceptance criteria live on stories and
gate a merge. Success criteria here measure whether the initiative was worth doing, and
are assessed after delivery. A success criterion that can be checked by running a test is
probably an acceptance criterion in the wrong place.

**Resource requirements name requirements and testing separately.** These are distinct
commitments and the organization tracks them as such. One line covering both is an
incomplete epic.

**The deadline field is conditional.** Where no deadline exists, say so. Do not derive one
from a quarter label, a sprint boundary, or a stakeholder's expressed hope.

**This epic does not hold the data model.** Grain, columns, relationships and assertions
belong to the specification, which the epic links to. See `../../specification-authoring/`.
