# Decision Log

One log per proposal. Every decision the proposal asks for, who must make it, and what it blocks until they do.

## Why it exists separately

A proposal states a recommendation. It rarely asks for **one** decision — it asks for a funding call, several design choices, an ownership question, and often a dependency someone else must commit to. Those go to different people and land on different dates.

Left inside the proposal, they are invisible as a set. Nobody can answer *what are we still waiting on and from whom* without re-reading the whole document, and unanswered choices get picked silently at build time.

**The log is the open set.** The proposal argues; the log tracks.

## What goes in it

Every decision the proposal requires, of any kind:

| Kind | Example |
|---|---|
| Approach | Which option is chosen |
| Design choice | Which attributes are versioned |
| Ownership | Who owns the filing-level asset going forward |
| Scope | Whether a deferred item stays deferred |
| Dependency | Whether another team commits to their part |
| Acceptance | Whether a stated correctness commitment is sufficient |

**A decision belongs here even if you expect it to be uncontroversial.** The ones that get missed are the ones everyone assumed were settled.

## Structure

```
# <Proposal> — Decision Log

| Field | Value |
|---|---|
| Proposal | <link, at a commit> |
| Maintained by | <name> |
| Last updated | <date> |

## Open

| # | Decision | Kind | Decider | Raised | Needed by | Blocks | Default if unanswered |
|---|---|---|---|---|---|---|---|
| D-001 | <what must be decided> | <kind> | <named person or forum> | <date> | <date> | <what cannot proceed> | <what happens with no answer> |

## Decided

| # | Decision | Outcome | Decided by | Date | Conditions | Recorded in |
|---|---|---|---|---|---|---|
| D-002 | <what was decided> | <what was chosen> | <who> | <date> | <any conditions attached> | <proposal section, spec section, or ADR> |

## Superseded

| # | Decision | Original outcome | Superseded by | Date | Why |
|---|---|---|---|---|---|
```

## Rules

**Numbered and append-only.** `D-###`. A decided item moves between tables; it never loses its number and is never renumbered. Citations point at numbers.

**A named decider, never a team.** "Data engineering" cannot decide anything. One person or one standing forum. A decision with no named decider does not get made.

**Every open decision states what it blocks.** A decision blocking nothing is not urgent and should be marked so rather than sitting in the open set creating noise.

**Every open decision states its default.** What happens if nobody answers by the date. This is the field that prevents silent choices at build time — where there is no acceptable default, say that the work stops, and say what stops.

**Conditions are recorded with the outcome.** "Approved, provided the deferred items return in Q4" is a different decision from "approved." A condition that is not written down is not a condition.

**Record where the decision went.** A decision that changed the proposal, seeded an ADR, or became a specification statement points at where it now lives. The log is the index; it is not the home.

**Never mark something decided from an absence of objection.** Silence is not a decision. It stays open with its default visible.

**Reversals are superseded, not edited.** The original decision stays with its outcome and date, and the new one references it. A decision log that can be edited to show only the current answer cannot explain why anything is the way it is.

## Where it lives

Alongside the proposal it belongs to, per `REPO-STRUCTURE.md`. It is not a general-purpose team decision log — a decision that outlives the proposal, or applies across products, belongs in the product's `decisions/` directory or in `docs/ways-of-working/`.

**Closing the log.** When every decision is decided or superseded, the log closes with the proposal. Anything still open at that point is not closed by the proposal being accepted — it carries into specification §9 with its decider and date intact, the same way intake questions do.
