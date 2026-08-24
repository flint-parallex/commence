---
name: delivery-scoping
description: Author the pre-delivery artifacts a Product Owner owns — scope documents, options/decision documents, and release plans — for data products and platform capabilities. Use this skill whenever the user needs to define what a delivery must accomplish before an approach is chosen, write up the business problem and functional requirements for a capability being built or rebuilt, document options and the decision made between them, or allocate agreed requirements to a release. Also use when the user says a capability was declined or is being replaced and they need to state why, when they ask what a solution "must do" independent of how, when they need a scope or requirements document from meeting notes, profiling, or a BA document, or when they need to record what is deferred and why. Trigger on casual phrasing too ("write up the scope for this", "what are we actually committing to", "put this into a scope doc", "which requirements land in release one"). Do not use for engineering specifications, schemas, or tickets — those are downstream.
---

# Delivery Scoping

Authors the artifacts that sit between a business need and an engineering specification.

## Where this sits

```
raw material  →  SCOPE  →  OPTIONS + DECISION  →  spec  →  work set → tickets
(BA doc,         (mode 1)   (mode 2)               (downstream)
 meetings,           │
 profiling)          └──▶  RELEASE PLAN (mode 3)
```

Downstream skills assume a canonical input already exists. This skill makes it.

## The governing principle

**Scope states what must be true. It never states how.**

The single test, applied to every sentence written into a scope document:

> Would this be written differently depending on which option was chosen?

If yes, it is not scope. Bitemporality fails the test — it is an approach. *A CIK may map
to multiple target identifiers* passes — it is a fact about the data that constrains every
option.

This matters because a scope document that has already specified has decided the design
before anyone approved it, and the options document then has nothing left to decide. The
value of the artifact is in what it refuses to answer.

## Raw material never travels

BA documents, meeting notes, chat threads, vendor demos, profiling output — all of it is
**evidence, read once to author the scope document, never fed forward.** The scope document
is the boundary at which raw becomes canonical. Everything downstream trusts it instead of
re-reading history.

A BA document is itself raw material. It carries phases, many requirements, and context
spanning multiple deliverables. Authoring is **extraction into a fixed shape**, not
translation.

## The three modes

**Scope mode** — the business problem, what must be delivered, why the current state cannot
deliver it, functional requirements, constraints, and boundaries. Release-agnostic and
approach-neutral. Template: `references/scope-template.md`.

**Options mode** — approaches assessed against the scope document's requirement IDs, with a
recommendation and the decision record. Template: `references/options-template.md`.

**Release mode** — allocation of agreed requirements to a delivery, with known limitations.
Template: `references/release-template.md`.

The modes chain. Scope first; options assessed against it; release allocates what was
agreed. Run scope alone when the problem is still being defined. Run release alone when
scope exists and an approach has been decided.

If the mode is ambiguous, ask. The outputs are different enough that guessing wastes time.

## The one-directional citation rule

The release plan cites requirement IDs. **The scope document never names a release.**

This is the mechanism that prevents erosion. A scope document that cannot name a release
cannot be quietly trimmed to fit one. When engineering says a requirement cannot be met in
time, it is **allocated to a later release** — never deleted, never reworded smaller. The
requirement stays true; only its timing moves.

Trimming is how a scope document silently becomes a description of what is being built
rather than what the business needs.

## Nothing is deleted, things are resolved

An open question becomes a decision with its date and decider. A deferral stays open while
it remains true. An alternative becomes a rejected option with what it gave up. Only
speculation that turned out to be irrelevant is removed.

This preserves the thread from business need through to built thing, which is the entire
reason these artifacts exist.

## Three categories, never flattened

- **In scope, allocated** — committed to a specific release.
- **In scope, not yet allocated** — committed, blocked on something nameable, with a
  trigger condition and an owner.
- **Out of scope** — a permanent boundary, usually because another team owns the outcome.

Collapsing the middle category into a "future enhancements" list understates what is being
committed to and invites it to be re-litigated in every subsequent meeting. A deferral with
a stated blocker and trigger reads as a managed boundary; the same item in a wish list
reads as a nice-to-have.

## Dependencies are not constraints

A requirement placed on another team is a **dependency**. The consequence of their failure
lands on them, not on the author, which makes it unenforceable and unfalsifiable as written.

For every dependency, find the **reciprocal requirement** on your own system that makes
their failure visible and attributable. *They must populate identifiers* is a warning.
*We must be able to state which answers are outstanding and for how long* is a requirement,
and it is what turns a silent recurring failure into an escalation with evidence.

## Design decisions get parked, not discarded

Scope discussions surface real design decisions — grains, keys, attributes, provenance
semantics. They do not belong in scope, and discarding them loses reasoning that was
expensive to produce.

Park them in the release plan's approach section as notes for the proposal and
specification. See `references/release-template.md`.

## Running any mode

1. Read all supplied material completely before writing.
2. Fill the template section by section.
3. Run the checks in `references/authoring-checks.md` against the draft.
4. Mark anything the source does not answer as a gap, in an HTML comment, with what would
   close it. Do not infer, and do not write the plausible version — an authored requirement
   is one nobody validated.
5. Convert remaining gaps into numbered open questions with owners.

A document with visible gaps is correct output when the source was thin. It is an accurate
picture of what is actually known.

## What a finished scope document never contains

Tables, grains, keys, uniqueness rules, pipeline steps, schema attributes, technology
names, release names, dates, effort estimates, or a preferred approach.
