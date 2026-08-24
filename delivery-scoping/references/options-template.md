# Options & Decision Template

Approaches assessed against the scope document's requirement IDs, with a recommendation and
the decision record.

**This document is authored in partnership with engineering.** Engineering generates the
approaches; the Product Owner owns the assessment against requirements and the framing of
the trade-offs.

---

## What this document is for

The scope document says what must be true. This one says how it could be made true, what
each way costs, and what was chosen. It is the artifact that gets accepted or rejected —
the scope document persists regardless.

**It never restates requirements.** It cites IDs. If an option cannot meet a requirement as
written, that is a finding about the option, not a licence to reword the requirement. Where
a requirement genuinely turns out to be wrong, the scope document changes through its
decision log first.

---

## Structure

### 1. Problem recap

Three or four sentences, citing the scope document. Enough that a reader who has not opened
it can follow. **Not a summary of the requirements** — a pointer to them.

### 2. Requirements in play

The requirement IDs this decision turns on. Usually not all of them; some are met
identically by every option and are not decision-relevant. Say which those are and set them
aside explicitly, so nobody assumes they were forgotten.

### 3. Options

At least two genuine options. A single option with two strawmen is not an options document
and reviewers recognise it immediately.

For each:

**Option N — short name**

- **What it is.** Two or three sentences.
- **How it meets each requirement in play.** Per ID: meets / partially meets / does not meet.
- **What it costs.** Effort, dependencies on other teams, operational burden, what it
  forecloses later.
- **What it gives up.**

### 4. Comparison

| | FR-1 | FR-2 | FR-3 | Cost | Foreclosed later |
|---|---|---|---|---|---|

A requirement that every option meets identically should not be a column.

### 5. Recommendation

Which option, and the reasoning in terms of the requirements — not in terms of preference,
elegance, or familiarity. State plainly what the recommendation gives up; a recommendation
with no stated cost reads as advocacy rather than assessment.

### 6. Decision

| Date | Decision | Decided by | Rationale if it differs from the recommendation |

Filled after the decision is made. **Rejected options are not deleted** — they stay above
with what they gave up, so that six months later the question *why didn't we just do X* has
an answer in the document rather than in someone's memory.

### 7. Consequences of the decision

What the chosen approach forces that was not obvious when it was chosen. Design decisions
that fall out of it and belong in the specification. New constraints it introduces.

This section is where the reasoning that produced the specification lives. Without it, the
specification arrives as a set of assertions with no visible derivation.

### Open questions

Anything the decision did not settle, with owners.
