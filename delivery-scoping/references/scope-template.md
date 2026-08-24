# Scope Document Template

Release-agnostic, approach-neutral. Requirement IDs are stable and cited by downstream
documents.

---

## Structure

### Header

Two short paragraphs, no metadata table:

- That the document is release-agnostic and approach-neutral, and that requirement IDs are
  stable and cited by the release plan while this document never cites a release.
- That it changes only when scope changes — not when an approach is decided — and that each
  change is a decision-log entry.

### 1. Business problem

**One sentence**, containing no mechanism: no pipelines, schemas, timestamps, technologies,
or vendor names. Then the costs it produces, named and separated.

*Test: would this sentence be written the same way if the capability being replaced had
never existed?* If not, it is a complaint wearing a requirement's clothes.

> Form D funds must be created and populated manually in the client system, resulting in
> significant time to establish coverage of private funds and leaving persistent coverage
> gaps.
>
> **Two costs:**
> - **Time to coverage.** Manual research and attribute population delay when a fund
>   becomes usable in the client system.
> - **Coverage gap.** Funds present in Form D but never created in the client system are
>   invisible to it, and the size of that gap is not currently known.

### 2. What we must deliver

Two or three numbered statements in business terms. This is what an engineering leader
reads first.

It sits **before** the current-state critique, so the critique has something specific to
fail against. Note explicitly if the delivery must hold for an accumulated backbook rather
than only forward flow.

### 3. Why the current state cannot deliver it

Argue on **three levels**. A case missing any level is incomplete.

**Level one — the capability cannot do it.** A table of observation → consequence, one row
per independent failure. Tie consequences back to the numbered deliverables in §2. Check
that rows are genuinely independent failures rather than one failure described three ways.

**Level two — the available workaround also fails.** The level most often omitted and most
often asked about in review. Without it, a reader proposes the obvious workaround and the
document has no answer. State why it fails on correctness rather than on cost where that is
true — a cost argument invites a budget response.

**Level three — cost of doing nothing.** A business fact, not an approach. Downstream
documents cite it rather than re-arguing it.

### 4. Functional requirements

**Aim for three.** Sub-criteria carry the detail. Nine top-level requirements are
unmemorable in a meeting; three with sub-criteria are both memorable and concrete enough
for a specification to inherit.

Each requirement states what **any** solution must do. Give each a stable ID (`FR-1`).

Every requirement must pass:

| Check | Failure looks like |
|---|---|
| **Falsifiable** | An engineer could satisfy the words and still fail the business |
| **Not a constraint in disguise** | It describes a limitation, not a need |
| **Survives every option** | It only makes sense if the preferred approach is chosen |
| **Uncontaminated** | It describes the inverse of the thing being replaced |

### 5. Constraints and boundaries

Facts that constrain every option. Give stable IDs (`C-1`). Typical content: what system is
authoritative, what cannot be relied on as a trigger, what is produced outside this system
and asynchronously, what already exists and is already delivered.

Something already built and delivered belongs here as a constraint, not as a requirement —
requiring what exists adds nothing.

### 6. In scope, not yet allocated

| Capability | Extends | Blocked on | Owner | Accepted until delivered |

These remain requirements. The table records only what blocks them. A blocker must be
nameable and legible — *their output schema is not stable, integrating now buys rework*
survives review; *we had thirty days* invites re-litigation.

### 7. Out of scope — permanent boundary

Lead with the ownership frame when the capability spans teams:

> *Matching is a three-team capability — data engineering, the AI application, and the
> client. This document commits the pipeline and the data.*

Then what is not committed, and the dependencies. This framing turns an apparent admission
of incompleteness into a stated boundary.

Include dependencies here, each with its reciprocal requirement.

### 8. Decision log

| Date | Change | Why | Decided by |

### Open questions

Numbered, with owners. Include baselines needed to size the problem — a missing number that
sizes a problem is a much better gap to have than a missing argument that proves it.
