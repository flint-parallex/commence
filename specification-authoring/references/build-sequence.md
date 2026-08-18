# Build Sequence

Section 5. The intent layer — the part no standard covers and no tool produces.

Sections 1–4 and 7–8 exist in some form in open standards. This section does not. It is the reason a maintainer two years out can understand why the product is shaped the way it is, and it is the only defence against a rule being reversed by someone who could not see why it was there.

## The four fields

Per stage. None of them can hold an implementation.

| Field | What it holds | What it must not hold |
|---|---|---|
| **Required end state** | What must be true of the data after this stage | The operation that produces it |
| **Reason** | The business justification for the rule existing | The mechanism, or a restatement of the effect |
| **Expected effect** | What changes: records removed, values derived, rows added | A row count from a specific run |
| **Dependency** | Which stage or asset must complete first | Scheduling or orchestration |

## The end-state test

**If the required end state reads like SQL, rewrite it.**

| Crossed the line | Rewritten |
|---|---|
| Deduplicate on accession number, keeping the latest | Each submission appears exactly once |
| Left join holdings to the manager table | Every position carries the manager responsible for it, or is marked as unattributable |
| Filter where status = 'ACTIVE' | Only positions the manager currently reports are present |
| Split the field on comma and cast to integer | Every integer reference the field carries is available as a separate value |
| Coalesce value to zero | No position has an absent value; a position reported without one is marked rather than defaulted |

The test in each case: **would this statement change if engineering chose a different implementation?** If yes, it is a mechanism.

## The reason field

This is the field most often filled badly, and the one that carries the most.

**A reason is not a restatement of the effect.** "Deduplication is applied so that duplicates are removed" says nothing. The reason answers why the business needs this to be true.

| Effect restated | Actual reason |
|---|---|
| Duplicates are removed so there are no duplicates | A manager filing twice for one period has corrected an error; counting both would double their reported position |
| Nulls are excluded so nulls are not present | A position without a quantity cannot be valued, and including it as zero would understate a manager's holdings rather than flag the gap |
| Rows are filtered to active status | Consumers analyse current exposure; a closed position in the set would read as held |

**Elicit this. Do not generate it.** A plausible business justification supplied by an agent is indistinguishable in the document from a real one, and it will be maintained as though it were real. Where the reason is not known, write that it is not known and route it — a recorded unknown is a scoped risk; an invented reason is a landmine.

## Stages that remove rows

Every stage that removes source rows must appear in the filter chain summary, in business terms (criterion 17).

This is what makes a row count that does not reconcile to source explicable rather than a support burden. A consumer asking "why does this have fewer rows than the filing" should find the answer in the summary, not in a ticket.

Where a stage removes nothing, say so. A build sequence in which nothing is removed is a meaningful statement — it means every source record survives to the product, and a later row-count discrepancy is a defect rather than a design.

## Ordering

The sequence records dependency, not schedule. Two stages with no dependency between them are unordered, and stating an order they do not require invents a constraint engineering must then honour.

Where two stages converge on a third, say so explicitly. A convergence is usually where the deliverable's difficulty lives, and it is the shape decomposition will cut against.

## Material decisions outside the sequence

Criterion 18. Not every choice — the ones a maintainer would otherwise reverse by accident.

The test: **would someone encountering this without explanation assume it was arbitrary and remove it?** If yes, it carries a reason. If no, leave it out; a document that justifies everything obscures the decisions that mattered.
