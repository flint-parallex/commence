# Functional Requirements

The bridge artifact. What any solution must be able to do, stated so that a business reader can check it against their need and a delivery reader can specify from it.

Nothing else in this system holds these. Business requirements say what the business needs to happen; specifications say what must be true of a built product. Between them sits a gap: **what capability is required**, independent of who builds it or how.

## The form

**Subject, capability, condition.** One requirement, one capability.

> The pipeline must detect records changed on the client side since the previous run.

Not two capabilities in one sentence, not a capability with its mechanism attached, not an outcome with no actor.

## Three tests

**1 · Would this survive a change of implementation?**

If the requirement names a mechanism, it is not a requirement — it is a design decision that has skipped the decision.

| Not a requirement | Requirement |
|---|---|
| Add a `last_modified` column and filter on it | The pipeline must identify which records changed since the previous run |
| Use a merge statement keyed on CIK | Re-processing a record must update the existing match rather than create a second one |
| Run the client extract nightly at 02:00 | Changes made on the client side must be reflected within one processing cycle |
| Build a staging table for unmatched records | Records with no match must be available to the review process as a distinct set |

**2 · Can a non-technical reader tell whether it is met?**

The approver has to check these against the business need. A requirement they cannot evaluate is one they will approve without understanding.

**3 · Can it be specified from?**

A requirement too vague to become a grain, a key or an assertion will produce a specification that invents the missing precision. *"The pipeline must handle changes properly"* fails this. *"A record that changed on the client side must be updated on the next run without producing a second match"* does not.

## What they must cover

Work these deliberately — the ones most often missing are the last three.

| Area | The question |
|---|---|
| **Direction** | Which way does data move, and does it move both ways? State each direction as its own requirement |
| **Change detection** | How is *changed since last time* established? Both directions, separately |
| **Identity** | What makes two records the same record? What makes a match the same match? |
| **Idempotence** | What must be true if the same input is processed twice? |
| **Unmatched handling** | What happens to records that do not resolve? Where do they go, and how do they come back? |
| **Return path** | When something is resolved elsewhere, how does it get back, and what does it update? |
| **History** | What must be recoverable about prior state? |
| **Volume and cadence** | How much, how often, and what must hold at peak |
| **Boundaries** | What must this *not* do — what it must not modify, overwrite or delete |

**Idempotence and the return path are the two most commonly omitted**, and they are the two that produce duplicates and silent staleness respectively. Write them even when they seem obvious.

## Numbering and traceability

Number them. `FR-001`, and never renumber — a proposal is cited by approvers and by downstream specifications, and a renumbered requirement silently re-points every citation.

Each functional requirement carries:

| Field | Content |
|---|---|
| ID | `FR-###`, append-only |
| Requirement | Subject, capability, condition |
| Traces from | The business requirement it serves |
| Rationale | Why it is required — usually a current-state failure, cited |
| Release | Which release it is committed to, or Deferred |

**Every functional requirement traces upward to a business need.** One that does not is either scope nobody asked for, or a business need that was never written down. Both are worth surfacing.

**Not every business need produces a functional requirement in this release.** Where one is deferred, say so and say what the business does in the meantime.

## Correctness

Alongside the requirement, state what would demonstrate it is met. Not an assertion — assertions are declared in the specification, against a built model. This is the acceptance-level statement the approver is agreeing to.

> **FR-004** — Records changed on the client side must be reflected in our data on the next run.
> **Demonstrated when:** a match created on the client side after our previous run is present, with its identifier, after the next run, and no duplicate match exists for that record.

That sentence becomes an assertion later, in specification §6. Written here, it is what the approver is committing to accept.

**A functional requirement with no demonstration is not committable.** If nobody can say what would show it working, it cannot be accepted, and it will be argued about at delivery instead of now.

## Do not

- Name tables, columns, or types. Those come with the specification.
- State a grain. Grain is a modelling decision made against real data.
- Write a threshold from observed data. A number here is a requirement, and requirements are decided.
- Bundle two capabilities into one requirement so the list looks shorter.
- Write a requirement that only restates the business need. If it does not add capability precision, delete it.
