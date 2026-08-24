# Authoring Checks

Run against any draft before it is shared. Ordered by how often each catches something
real.

---

## 1. Mechanism masquerading as problem

Describing what a system does is not stating a problem. If the business problem section
mentions pipelines, matching, schemas, or timestamps, it is a mechanism description.

Strip the mechanism and ask what the business loses. That sentence is the problem.

**Related failure:** the stated problem and the stated delivery do not match. If coverage
is named as the problem but the delivery only resolves identity, the gap is real and the
first reviewer to compare them will find it. Either widen the delivery or narrow the
problem — do not leave them mismatched.

## 2. Contamination by the thing being replaced

A requirement written after seeing a failed capability tends to describe its inverse rather
than the business need.

> *Must have populated timestamps* — a complaint.
> *Must be able to state what was believed as of any past date* — the requirement, which
> happens to also rule out the failed capability.

Ask of every requirement: would this be written this way if the failed thing had never
existed?

## 3. Results where states belong

A result collapses distinctions that matter operationally. *No match* can hide *not yet
looked at*, *looked at, answer pending*, and *looked at, answer is no*.

Wherever a document reports an outcome, ask how many distinguishable situations produce
that same outcome, and whether the consumer needs to tell them apart.

## 4. Timing substituted for state

A run window, a schedule, or a batch cadence proposed as the answer to a correctness
problem. It will fail on partial completion, because timing cannot distinguish an
unprocessed remainder from a genuine unknown.

Ask what any scheduling decision is compensating for. If the answer is a missing state
source, the state source is the requirement and the schedule is a mitigation.

## 5. Independent clocks modelled as one

Two systems on different cadences — a source that publishes annually and a human process
that resolves in days — modelled as though they move together. A recurring source of silent
failure.

Ask: what are the clocks here, who controls each, and what happens when they diverge?

## 6. Dependency stated as constraint

*Team X must do Y* places the consequence on Team X and is unenforceable. Find the
reciprocal requirement on this system that makes their failure visible, attributable, and
measurable in elapsed time.

Without it, the failure recurs silently and the argument six months later has no evidence.

## 7. Illegible deferral reasons

*We had thirty days* reads as a corner cut and invites re-litigation. *Their output schema
is not stable and integrating now buys rework* is engineering-legible and survives review.

Every deferral needs a blocker, a trigger condition, an owner, and what is accepted in the
meantime.

## 8. Grain and uniqueness assumptions against real source data

Ask what happens when the authoritative system does something that looks wrong — maps one
identifier to several targets, or holds duplicates it considers valid.

If the authoritative system is authoritative, its data is taken as true. A uniqueness rule
that rejects valid source data is a defect, and properties that were relying on that
uniqueness need another mechanism.

## 9. Approach leakage into scope

Scan the scope document for tables, grains, keys, uniqueness rules, pipeline steps, schema
attributes, technology names, releases, dates, or estimates.

For each, find the requirement underneath it. *Bitemporality* becomes *prior belief must be
reconstructable for any date*. Move the concrete decision to the release plan's parked
design notes.

## 10. Flattened deferral categories

Check that *in scope but blocked* items are not sitting in the same list as *permanently out
of scope* items. Flattening understates commitment and turns a managed boundary into a wish
list.

## 11. Unfilled and unmarked

Every gap should be visible, in an HTML comment, stating what would close it. A gap that
was smoothed over with a plausible sentence is worse than an empty section, because nobody
knows to ask about it.
