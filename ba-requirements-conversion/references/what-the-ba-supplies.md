# What the BA Supplies

The business analyst owns the business need — the what and the why. This is the inventory of what that ownership covers.

Use it as the analysis lens: read the supplied material against this list and record, for each item, what was stated and what was not.

**Silence is signal, not failure.** Where the source does not address something, flag it. Flagging a gap is the analysis working correctly — it is how a requirement's thin spots become visible before they become delivery problems. Filling the gap yourself destroys exactly the information the flag was carrying.

## The inventory

**Business need**
What the product is for. Who consumes it. What decision it supports. The use cases it must serve.

**Semantics**
What each field means in business terms. How derived metrics are defined. What enumeration values signify operationally. Edge cases — what a classification means at its boundary, what happens to a record whose status changes.

This is the most frequently under-specified item and the most expensive to get wrong, because a semantic misunderstanding produces data that is technically correct and business-wrong.

**Grain**
One row per what, in business terms. A business question before it is a modelling one: per entity, or per entity per date, depends on what needs answering.

Record the business answer. Declared keys, composites and uniqueness constraints are modelling decisions made later against real data — do not ask for them here, and do not record them if offered.

**Historization**
How change over time should be tracked. Whether a past state must be recoverable or only the current state matters. Business question, modeling consequence.

**Source and lineage**
Which system the data comes from. Which source is authoritative when two disagree.

**Quality expectations**
What correct looks like. What null signifies where it appears. What must reconcile and against what.

Ask what would make the data wrong. Do not ask for assertions — converting a business expectation into a check is specification work.

**Freshness and SLA**
How current the data must be, and the business consequence of staleness.

**Consumers and downstream use**
Who reads it and through what — dashboards, semantic layers, models, reports. What breaks if the shape changes.

**Dependencies**
Other datasets this relies on. Known constraints.

## Using the inventory

For each item, record one of:

- **Stated** — the source addresses it, specifically enough to build from
- **Partial** — the source gestures at it without pinning it down ("recent data", "the main clients"). Partial answers are the most dangerous, because they read as answers and invite false precision. Record what was actually said, then raise the question.
- **Silent** — the source does not address it

Every `Partial` and every `Silent` becomes an open question owned by the BA.

## What this is not

This is not a form the BA must complete before work begins. Presented as a gate, it reads as obstruction and stops being used.

It is the standing expectation of what is needed to convert a requirement into delivery — held consistently, applied the same way to every requirement, from every analyst. Consistency is what makes it a standard rather than a preference.
