# Status Vocabulary

Permitted values for intake state and delivery status fields.

## The principle

**Status values describe where the work is, not the quality of what was supplied.**

This is the whole design. A status that assesses the incoming requirement — "unratified," "incomplete," "insufficient" — reads as a verdict on the analyst's work on a page other people see. It invites an argument about whether the requirement was good, which is the wrong argument: the requirement's quality is not the question, and debating it stalls delivery.

A status that describes process position conveys the identical operational information without the verdict. A reader still sees exactly which products are moving and which are not.

## The vocabulary

| Value | Meaning |
|---|---|
| `Requirements received` | Material in hand. Conversion not yet started. |
| `In conversion` | Being worked through; open questions outstanding. |
| `Confirmed` | Questions resolved. Ready to enter delivery. |
| `In delivery` | Building. |
| `Documented — pre-process` | Predates the intake process. No conversion was run. |

## Notes on specific values

**`In conversion`** places the activity on the delivery side of the seam, which is where it actually is. The work being done is conversion, not waiting.

**`Confirmed`** rather than "ready." Ready implies a gate that someone failed to clear. Confirmed states what happened: a conversation occurred and concluded.

**`Documented — pre-process`** rather than "retrofit." Retrofit suggests remediation of something done wrong. Work that predates a standard is ordinary, and stating it neutrally keeps it that way.

## Rules

Use only these values.

Where a situation does not fit, describe it in prose in the body of the document rather than coining a new status. A vocabulary that grows ad hoc stops being comparable across products, which is the only reason to maintain one. Flag that the vocabulary may need extending.

Apply consistently across every product and every analyst. Consistency is what makes this an instrument rather than a preference.
