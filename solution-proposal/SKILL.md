---
name: solution-proposal
description: Authors the proposal that decides how a data delivery problem will be approached — the business need in non-technical terms, evidence that the current state cannot meet it, the functional requirements any solution must satisfy, the options considered, the recommendation, and what will be committed to. Use when the approach is still open and a decision is needed: rebuilding or replacing an inherited pipeline, a data model redesign, choosing between build and buy or fix and replace, or writing functional requirements that bridge a business need to a specification. Triggers on phrasing like "write a proposal for", "how should we approach", "what are our options for", "we need to justify rebuilding", "write the functional requirements", or "make the case to leadership" without an explicit request.
---

# Solution Proposal

The document that decides how a problem will be approached, written for the people who must agree to it.

This fires when the business need is understood and the approach is not. It sits between requirements and specification: one proposal may produce several specifications, and none of them can be authored until the approach is settled.

**A proposal argues. A specification declares.** Everywhere else in this system, alternatives are forbidden — a specification states what must be true and carries no options. Here, the options are the substance. Keep the two postures separate; a proposal that reads like a specification has hidden the decision it was written to surface.

## Scope

**In:** the business case, evidence of current-state failure, functional requirements, options with a recommendation, scope boundaries, correctness commitments, traceability from business need to delivery.

**Out:** grain, columns, types, keys, build sequence, assertions, monitors — `specification-authoring`, and only after the approach is agreed. Epics, stories, tickets — `delivery-readiness`. Rendering and posting — `delivery-communication`. Consolidating what the business asked for — `ba-requirements-conversion`.

The test: is the approach **still open**? If it is settled, this is not a proposal, and the artifact is a specification.

## Audience

**Two readers, one document.**

A non-technical approver needs to follow the business need, what is broken, what it costs, and what is being committed to — without reading a schema.

A delivery reader needs functional requirements precise enough to specify from.

Serve both by **layering, not by splitting**. Business framing first, technical detail below it, and a traceability spine connecting them so either reader can move between the two. Two separate documents diverge, and the technical one becomes the real one while the business one goes stale.

## Voice

**Neutral about inherited work.** State what the system does and does not do, evidenced. Never characterise the people who built it or the decision to acquire it. A proposal that reads as blame invites a defence of the past instead of a decision about the future — and the people who must approve it may be the people who approved the original.

**Evidenced, not asserted.** "The load timestamp is null in 100% of rows" is a fact anyone can check. "The pipeline is broken" is a conclusion, and it will be argued with.

**Precise about commitments.** What will be delivered, by when, and what will be true when it is done. A proposal that commits vaguely gets accepted and then relitigated.

**Plain about consequences.** What happens if this is not done, stated without escalation. The facts usually carry it.

## Core principles

**Every failure claim traces to an observation.** Name the table, the column, the measurement, the date. A claim that cannot be traced is removed — not softened. See `evidence-and-options.md`.

**Distinguish what is broken from what is missing.** A capability that was never built is not a defect. Both may need solving; conflating them makes the proposal sound like an accusation and makes the scope harder to bound.

**Distinguish local from systemic.** Where the same failure affects other consumers of the same asset, say so — and say explicitly whether this proposal fixes it for them. A proposal that quietly fixes one dataset will be assumed to have fixed the shared problem.

**Record every option, including the rejected ones and doing nothing.** A recommendation without its alternatives invites "did you consider X" and, later, gets reversed by someone who cannot see why it was chosen.

**Functional requirements state capability, never implementation.** *The pipeline must detect records changed on the client side since the previous run* — not how. See `functional-requirements.md`.

**State what correct means before committing to build it.** Not assertions — those come with the specification. Acceptance-level statements: what will be observably true when this works. A proposal that commits to build without committing to a definition of working has committed to nothing checkable.

**Bound the release explicitly.** What is in the first release, what is deferred, and what remains unsolved after it. Deferred scope that is not written down is assumed to be included.

**Name the owner of every asset in the flow.** A flow that crosses teams has decisions nobody has been assigned. An asset we touch but do not own will be assumed to have become ours unless the proposal says otherwise — and silence defaults badly in both directions.

**Separate options from open design choices.** Options are mutually exclusive approaches; one is chosen and the rest are rejected. Open design choices sit inside the chosen approach and are the business's to pick from a menu. Each carries its functional consequence and a stated default, because a chooser will not infer what versioning an attribute does to row counts or to the meaning of *current*.

**Say why the change is the extent it is.** Where the approach deliberately builds on what exists rather than redesigning, record that as a decision. Unstated, it reads as an oversight and a reviewer will ask why a proper redesign was not proposed.

**Ask rather than invent.** Volumes, costs, dates, effort, what another team committed to. An invented number in a proposal is a number someone will plan against.

## Procedure

1. **State the business need** in the language of the people who have it. No schema.
2. **Establish current state** against that need, per `evidence-and-options.md`. Every claim carries its observation.
3. **Write the functional requirements** — what any solution must do — per `functional-requirements.md`. These are the durable artifact; they survive whichever option is chosen.
4. **Set out the options**, including doing nothing, each assessed against the functional requirements.
5. **Recommend**, with the reasoning and what the recommendation gives up.
6. **State what changes** — data model, schema, what gets built. At the level of what, not how. Name every asset in the flow and its owner, and say why the change is the extent it is.
7. **Record the open design choices** the business must still make, each with its functional consequence and a default.
8. **Bound the scope.** This release, deferred, and still unsolved after.
9. **Commit to correctness.** What will be observably true when this works.
10. **Build the traceability spine.** Business need → functional requirement → change → specification. Both directions.
11. **State consequences** of not proceeding.
12. **Open the decision log.** Every decision the proposal asks for, with a named decider, what it blocks, and a default.

Where an asset needs field-level change, write it as a sub-page per `asset-change-proposal.md` and link to it. Do not summarise a sub-page into the parent.

Use `proposal-template.md`.

## What this skill does not do

**It does not specify.** Once the proposal is agreed, the functional requirements become inputs to `specification-authoring`. Do not author grain, keys or assertions here — a proposal that has specified has decided the modelling before anyone approved the approach.

**It does not estimate.** Effort and sizing come from the team. Record what they supplied and attribute it.

**It does not commit another team.** Where a dependency requires someone else's work, state it as a dependency with its owner and its status. A proposal that commits a team that has not agreed will fail at the point it matters.

**It does not specify an asset another team owns.** Cite it, state whether we depend on it and what happens if it changes, and raise its future as an open question with a named owner. Never write requirements for someone else's asset.

**It does not decide.** The proposal is written to be accepted or rejected. Present the recommendation; the decision and its record belong to the approver.

**It never marks something decided from an absence of objection.** Silence is not agreement. An unanswered decision stays open with its default visible.

## References

- `proposal-template.md` — the structure
- `functional-requirements.md` — writing capability requirements that bridge business need and specification
- `evidence-and-options.md` — the evidence discipline, and how options are recorded
- `asset-change-proposal.md` — proposing a change to one existing asset, as a sub-page
- `decision-log.md` — the open set: every decision the proposal asks for, and who must make it
- `../specification-authoring/references/specification-template.md` — where agreed functional requirements go next
- `../delivery-communication/references/confluence-formatting.md` — rendering, owned by that skill

## The line this skill holds

A proposal establishes **what we should do and why**. A specification establishes **what must be true of what we built**.

The test: **if this sentence would still be written the same way after the approach was chosen, it belongs in the specification, not here.**
