# Decomposition

How a canonical delivery document becomes a proposed ticket set, and how that set reshapes as information arrives.

Decomposition is never one-shot. The canonical document is live — requirements arrive by email, in conversation, and out of grooming, and the ticket set changes with them. Every mode below is re-runnable.

## Modes

1. **Propose** — read a canonical delivery document, propose the ticket set
2. **Refine** — new information has arrived; report what changes
3. **Spike from grooming** — an unknown surfaced in grooming; write the spike
4. **Close spike** — the spike returned; reshape the blocked ticket

---

## Mode 1 · Propose

**Input:** the canonical delivery document. **Output:** a proposed ticket table for approval. Do not write tickets until the proposal is approved.

### The table delivery pattern — fixed

For every table in the deliverable, propose **two tickets**:

| # | Ticket | Scope | Closes |
|---|---|---|---|
| 1 | Build | The table, the data contract, pipeline observability | At UAT |
| 2 | Empirical monitoring | Volume, freshness, primary key, referential (L3) | After real data exists in UAT |

The split exists because empirical thresholds require real data. Ticket 1 always carries the standard exclusion: *ML/empirical monitors requiring production data history are out of scope*.

Six tables means twelve tickets before any other work is considered.

### Non-table work

Propose from what the canonical document declares. Do not infer.

- **Refactor** — proposed at the level the input declares it. Scoped to tables, it attaches per table. Scoped to the product, it is its own workstream.
- **Matching or transformation pipelines** — where the input document specifies them, propose per pipeline
- **Capability dependencies** — where another team provides a capability, do not propose a ticket for their work. Record it as a dependency on the epic

### Do not propose spikes

Spikes are discovered in grooming, not at decomposition. The team learns it does not know how to parse a source, or whether an internal library conversion is viable, when the build ticket is walked through. Proposing spikes speculatively fabricates unknowns.

Write the build ticket with the data source, its origin, and the business context. Grooming surfaces what is unknown.

### Output format

```
PROPOSED TICKET SET — <epic>

| # | Type | Title | Depends on | Notes |
|---|------|-------|------------|-------|

Sequence: <what must land before what>
Not proposed: <anything in the document deliberately excluded, and why>
Missing inputs: <what the document does not specify that a ticket would need>
```

The missing-inputs section is required. Ask rather than inventing table names, grain, sources, or dates.

---

## Mode 2 · Refine

New information has arrived — an updated canonical document, an email, a decision in conversation. Do not regenerate the ticket set.

**Report only what changed:**

```
NEW — tickets that should now exist
AFFECTED — existing tickets whose scope, AC, or dependencies change, and specifically what
UNCHANGED — count only, not a list
CONFLICTS — where new information contradicts what a ticket currently states
```

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

**Check 1 — scope.** Run the design scope assessment. If the output carries an architectural decision above ticket scope, that decision routes for confirmation before the build ticket is touched. An architectural decision hiding inside a design document must not be absorbed into a build ticket.

**Check 2 — acceptance.** Every acceptance criterion on the spike is met by the output. If not, the spike is not closed.

Only when both clear does the blocked ticket reshape.

### Refine in place, or decompose

| Condition | Action |
|---|---|
| Scope unchanged, approach now known | **Refine in place.** Update the build ticket's AC, constraints, and data model reference |
| Spike revealed the work is materially larger than the ticket assumed | **Decompose.** Propose child tickets, each following the table delivery pattern where applicable |

When decomposing, state what the original ticket assumed and what the spike revealed, so the change is auditable.

---

## Rules

- **Propose, never write, until approved.** Modes 1 and 2 output a table. Ticket bodies come after.
- **Ask for missing inputs.** Never invent table names, grain, sources, keys, or dates.
- **Two tickets per table, always.** Build and empirical monitoring.
- **No speculative spikes.** Unknowns are discovered, not predicted.
- **Do not restate the epic in a ticket, or the parent in a subtask.** Reference, do not duplicate.
- **Neutral and brief.** No padding, no restating the requirement back, no narrating the process. Professional register throughout. A long ticket slows delivery as surely as a vague one.
