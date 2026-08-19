# Published Document Discipline

This document is published where the analyst, their management, and delivery stakeholders can read it. It is the standing record of what was requested and what is being built.

That changes how it must be written and how it must change over time.

## Tone

The facts do not soften. The framing does.

A requirements analysis that marks most items as not supplied is accurate — and on a page other people read, an accurate scorecard reads as an indictment of the analyst. It provokes defensiveness, and a defensive counterpart stops supplying information, which defeats the document's purpose.

Write gaps as questions, not verdicts:

| Instead of | Write |
|---|---|
| Grain not supplied | What is the intended grain — one row per asset, or per asset per valuation date? |
| No quality thresholds defined | What should reconcile, and against which source? |
| Semantics missing for status field | How should each status value be interpreted for reporting? |
| SLA not specified | How current does this need to be for the reporting cycle? |

Same information. One is a person asking; the other is a scorecard.

Additional guidance:

- Attribute to documents, not people. "The requirements document does not specify X" rather than "the analyst did not provide X."
- State delivery risk in terms of what delivery needs, not what someone failed to do.
- Do not editorialize about the completeness of the material. The reader can see the table.
- Never characterize the counterpart's work quality anywhere in the document.

## Ratification

The document has a lifecycle. Open questions get answered, and answers become stated requirements.

**Preserve the history rather than overwriting it.** When a question is answered, the requirement row updates and the question moves to resolved — with the answer, who gave it, and the date. Do not silently absorb an answer as though the requirement had always said that.

This matters for two reasons. A reader needs to know whether a requirement came from the original material or from a later conversation. And a resolved question is evidence the seam is working — questions asked, questions answered, delivery proceeding on an agreed basis.

**Mark requirement provenance:**

- `As supplied` — from the original material
- `Ratified [date]` — resolved through conversation with the analyst
- `Open` — still outstanding

**Keep resolved questions visible.** Move them to a resolved section rather than deleting them. The record of what was asked and answered is the point.

**Version the document.** A dated change log at the top: what changed, why, and which conversation drove it. A reader arriving mid-delivery must be able to tell what state they are looking at.

## What the document does not do

It does not declare its own authority. A page that asserts it is the canonical or authoritative record invites someone to contest that claim. A page that is simply the complete, current, well-maintained record becomes canonical without ever saying so.

Describe what the document contains. Let its completeness make the argument.
