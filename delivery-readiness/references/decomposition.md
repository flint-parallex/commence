# Decomposition

How a canonical delivery document becomes a proposed work set, and how that set reshapes as information arrives.

Decomposition is never one-shot. The canonical document is live — requirements arrive by email, in conversation, and out of grooming, and the set changes with them. Every mode below is re-runnable.

## Modes

1. **Propose** — read a canonical delivery document, derive the work set
2. **Refine** — new information has arrived; report what changes
3. **Spike from grooming** — an unknown surfaced in grooming; write the spike
4. **Close spike** — the spike returned; reshape the blocked ticket

---

## Mode 1 · Propose

**Input:** the canonical delivery document. **Output:** a derived work set with its reasoning, for approval. Do not write tickets until the proposal is approved.

This mode is a reasoning task over the specification. It is not a lookup. The same number of tables can yield very different work sets depending on grain, dependency shape, and where correctness is uncertain — and where it does not, the specification has not said enough.

### The unit of work

**A work unit ends where a grain assertion can be evaluated.**

That is the cut rule. Below that boundary there is nothing to verify, which is why table-by-table decomposition produces work items with no consumer and no testable completion. Above it, the unit is too large to review.

A unit is therefore a set of assets that must exist together before the declared grain can be asserted — a parent and the children that resolve it, a match step and the reference it matches against. Frequently more than one table. Occasionally one.

### Derivation

Work in this order. Show the work; the reasoning is part of the output.

**1 · Build the dependency graph.**
From the build sequence (DoR criteria 16–17) and the declared references and shared dimensions (criterion 14). This gives sequence, not units.

**2 · Locate the grain boundaries.**
Every asset declares a grain (Level 2, criterion 1). The Solution declares a single entity at a stated grain (criterion 24). Each point where a grain becomes assertable is a candidate cut.

**3 · Separate reference data from the thread.**
Canonical references and shared dimensions are inputs that must exist before the thread starts. They are enablers, they land once, and they are **not** cut points. Do not spend a unit per dimension.

**4 · Cut the thread at the grain boundaries.**
Each cut is a unit. Name what grain assertion closes it.

**5 · Classify each unit.**

| Condition | Type | Reference |
|---|---|---|
| A consumer receives a functional change | User story | `user-story.md` |
| Required for a later unit, no consumer-facing outcome | Enabler task | — |
| The approach is undecided | Not ready — see below | `spike.md` |

Classify honestly. If a unit has no consumer, it is an enabler, and it is written as a task. Do not force a story frame onto it — `user-story.md` forbids the engineer as the user, and inventing a consumer to satisfy the form produces exactly the ticket that rule exists to prevent.

**6 · Mark where correctness is uncertain.**
Where a unit's output is a judgement rather than a determinate transformation — probabilistic matching, keyword identification, classification against an incomplete reference — say so, and name what makes it uncertain. This is not a spike. It is a flag that the unit carries risk the ticket set does not otherwise show, and it is what grooming needs to know before estimating.

**7 · Attach monitoring.**
Per the monitoring section below. Monitors attach to units, never to tables individually.

### Monitoring

Monitors are not a trailing phase. Criterion 23 makes the binding an acceptance condition, so an unbound assertion means an unacceptable unit.

**Data assertions** are acceptance criteria on the unit's build ticket. They gate the merge. They are never separate tickets.

**Runtime monitors** cannot gate a merge (criterion 20). They are a deliverable of the unit, verified post-deploy: the check is that the monitor is deployed and its binding is recorded — not that it has fired.

**Split runtime monitors by threshold source before proposing anything** (criterion 22):

| Threshold source | Implementation | Ticket |
|---|---|---|
| Observed history — freshness, volume, schema drift | Out-of-the-box tooling | **No ticket.** Record as covered |
| Specification — grain, fan-out ratio, referential, business rule | Manual | Carried on the unit |

A monitor whose threshold traces to the specification must be implemented manually even where tooling would watch the same column, because history is not a test oracle. "The tool already covers that" is the natural and wrong conclusion; state the threshold source rather than the column.

Where a unit's specification monitors are numerous enough to be their own increment, propose them as a separate ticket sequenced after the build. That is a judgement about size, not a fixed rule.

### Non-unit work

Propose from what the canonical document declares. Do not infer.

- **Refactor** — proposed at the level the input declares it
- **Capability dependencies** — where another team provides a capability, do not propose a ticket for their work. Record it as a dependency on the epic

### Do not propose spikes

Spikes are discovered in grooming, not at decomposition. Proposing them speculatively fabricates unknowns. Marking a unit as carrying uncertain correctness (step 6) is not proposing a spike — it surfaces the risk without inventing the question.

### When it does not decompose

Say so. Do not force a cut.

- **A fan-out with no assertable grain** is a defect in the specification, not a decomposition problem. Report it as a question back to the specification, naming the relationship and what would have to be declared.
- **A unit that cannot be closed by any assertion** is under-specified. Name what is missing.
- **An undecided approach** means the unit is not ready as a story (`definition-of-ready.md`). Report it; do not write a speculative ticket.

An honest refusal is a better output than a plausible ticket set built over a gap.

### Output format

```
PROPOSED WORK SET — <epic>

DERIVATION
  Dependency order: <what must land before what, and why>
  Grain boundaries: <each boundary and the assertion that closes it>
  Reference data: <enablers landing before the thread>

WORK SET
| # | Type | Title | Closing assertion | Depends on | Risk |
|---|------|-------|-------------------|------------|------|

MONITORING
  Covered by tooling: <freshness/volume/schema, per unit>
  Manual, spec-sourced: <monitor, unit, threshold source>

Not proposed: <deliberately excluded, and why>
Does not decompose: <any unit that could not be cut, and what is missing>
Missing inputs: <what the document does not specify that a ticket would need>
```

Derivation, monitoring, and missing-inputs sections are required. Ask rather than inventing table names, grain, sources, keys, or dates.

---

## Mode 2 · Refine

New information has arrived — an updated canonical document, an email, a decision in conversation. Do not regenerate the work set.

**Report only what changed:**

```
NEW — units that should now exist
AFFECTED — existing units whose scope, AC, or dependencies change, and specifically what
UNCHANGED — count only, not a list
CONFLICTS — where new information contradicts what a unit currently states
```

Where new information changes a grain or a declared assertion, say whether the cut points still hold. A changed grain can dissolve a unit boundary, and that is not visible from the ticket list alone.

Conflicts are stated, never silently resolved. The operator decides.

---

## Mode 3 · Spike from grooming

A build ticket was walked through in grooming and the team surfaced something it does not know.

**Input:** the grooming transcript and the build ticket. **The spike's question comes from what the developers actually said they needed to discover** — not from a general reading of the ticket.

Find the passage in the transcript where the ticket was discussed. Derive:
- What specifically the team said it could not proceed without knowing
- What options were raised, if any
- Whether the question is capability ownership ("should the capability team do this?") or approach ("how do we do this?")

The spike states that question. It does not restate the build ticket.

Per `spike.md`, the spike declares what its output must contain — design document, assessment, or recommendation. Link the spike to the build ticket it blocks (`blocks` / `is blocked by`).

If the transcript does not contain a clear statement of the unknown, say so rather than inferring one.

---

## Mode 4 · Close spike

The spike returned an output. Before the blocked ticket can be reshaped, the output must clear two checks.

**Check 1 — scope.** If the output carries an architectural decision above ticket scope, that decision routes for confirmation before the build ticket is touched (criterion 6). An architectural decision hiding inside a design document must not be absorbed into a build ticket.

**Check 2 — acceptance.** Every acceptance criterion on the spike is met by the output. If not, the spike is not closed.

Only when both clear does the blocked ticket reshape.

### Refine in place, or re-derive

| Condition | Action |
|---|---|
| Scope unchanged, approach now known | **Refine in place.** Update the unit's AC, constraints, and data model reference |
| The spike revealed the work is materially larger than assumed | **Re-derive.** Run Mode 1 over the affected unit only |

Where the spike changed a grain or revealed an unassertable relationship, the cut points must be re-derived, not adjusted. State what the original assumed and what the spike revealed, so the change is auditable.

---

## Rules

- **The cut rule is the grain assertion.** Not the table, not the pipeline stage.
- **Propose, never write, until approved.** Modes 1 and 2 output a derivation and a table. Ticket bodies come after.
- **Show the derivation.** A work set without its reasoning cannot be reviewed, only accepted.
- **Classify enablers honestly.** No consumer means a task, never a story with an invented user.
- **Split monitors by threshold source.** Tooling covers observed history; the specification is implemented manually.
- **Ask for missing inputs.** Never invent table names, grain, sources, keys, or dates.
- **No speculative spikes.** Unknowns are discovered. Uncertain correctness is flagged, not converted into a question.
- **Refuse to cut what does not cut.** Report the gap.
- **Do not restate the epic in a ticket, or the parent in a subtask.** Reference, do not duplicate.
- **Neutral and brief.** No padding, no restating the requirement back, no narrating the process.
