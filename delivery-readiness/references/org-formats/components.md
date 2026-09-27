# Components

`Component` is a required field, and **it means two different things depending on the
level.** Conflating them puts work on the wrong board.

| Level | What the component names | Purpose |
|---|---|---|
| **Epic** | The **team** | Routes the epic to that team's board |
| **Story, task, subtask, bug** | The **technical unit** | Categorizes what was built, within the board |

An epic's component answers *who delivers this*. A ticket's component answers *what part
of the system this touches*. They are not drawn from the same list and one never
substitutes for the other.

---

## Epic level — teams

The component is the delivery team, which determines the board the epic lands on.

Team component values are not yet recorded here. **Ask for the team's component rather
than inferring it from the team's name.**

---

## Ticket level — technical units

Two kinds of value, and the cross-cutting ones win where both could apply.

### Cross-cutting

| Component | Covers |
|---|---|
| `documentation` | Spikes, research, designs, ADRs — anything whose output is a written artifact rather than shipped behaviour |
| `production support` | Production support work, including the recurring weekly load |
| `capability testing` | Testing carried out for capability teams |

### Per data product

Build work on a data product carries that product's own technical component.

| Component | Data product |
|---|---|
| `SEC_13F` | 13F |
| `RIA_L3` | IAR |

This table is the registry. **A data product not listed here has no component the skill
can supply.** Do not derive one from the two examples above — two values are not a naming
convention, and a fabricated component routes work into a category that does not exist.
Ask, then add the confirmed value to this table.

### Precedence

Where both could apply, the cross-cutting component wins.

A spike about the 13F product is `documentation`, not `SEC_13F` — all spikes, research,
designs and ADRs go to `documentation` regardless of which product prompted them. The same
holds for production support on a data product, and for capability testing of one.

`SEC_13F` and its siblings are for build work: the stories that ship the product.

---

## Derivations that follow

**Every spike carries three markers.** A task, with the `spike` label, under the
`documentation` component. All three, always — the label and the component are not
alternatives to each other.

**Design and impact assessments carry `documentation`.** The organization lists these as
task examples, and their output is a written artifact. The same applies to an ADR, however
technical its subject.

**Subtasks inherit.** A subtask defaults to its parent's component, per `subtask.md`.
Where a subtask genuinely belongs elsewhere — capability-team testing hanging under a
build story — set it explicitly and say why, rather than letting the default carry a
ticket into the wrong category.

**An epic spanning several data products still has one team component.** The per-product
components appear on its tickets, not on the epic.

---

## When the component is not determined

Component is not cosmetic — at epic level it decides which board sees the work, and at
ticket level it decides how delivery is categorized and reported. Report it as an open
question in the same form as any other unfilled required field:

> Component not determined. The work is *[what it is]* on *[which product or team]*, which
> is not in the registry in `components.md`. Confirm the component before this is created.

---

## Notes

**Component is not a label.** Labels route testing (`uat`, `dev test`) and mark kind
(`data quality`, `spike`). Components route ownership at epic level and categorize
technical work at ticket level. A ticket needs both, and one never substitutes for the
other.

**Production support has its own story template.** Production support tickets are created
from the organization's copyable story, not authored from scratch — see
`production-support.md`.
