# Spike

A timeboxed investigation that produces a decision or a recommendation. A spike does not ship code.

Spikes are **discovered in grooming, not planned at decomposition** — the team walks through a build ticket and surfaces something it cannot proceed without knowing. Write the spike after that discovery, anchored to what the developers actually said they needed to determine. See `decomposition.md`, Mode 3.

**Spikes are exempt from the standard build gates.** The Definition of Done for a data product deliverable does not apply. The spike's own acceptance criteria are the gate.

## Required inputs

Ask for any missing. Never invent the question the spike is answering.

- **Investigation question** — the specific thing that must be determined. From the grooming transcript where possible
- **Success criteria** — what a done spike produces. **Define this before the spike starts.** It is the primary guard against scope crawl
- **Downstream work unblocked** — which build tickets depend on this outcome, and when it is needed
- **Timebox** — hard cap

## Structure

**Issue type:** Task
**Title:** `Spike - <the question being answered>`
**Labels:** OKR tag (IP-OKR-###) and `spike`

### Description

Two parts.

**What is being investigated and why.** State the decision the spike must produce, not the background.

**What depends on this.** Name the build tickets this unblocks and when the outcome is needed. Link them (`blocks` / `is blocked by`).

The framing is simple: *this unblocks implementation of `<ticket>`. The output must enable that ticket's acceptance criteria to be met.* Reference the build ticket's AC rather than restating them — the spike exists to make a design possible that satisfies them.

### In scope / Out of scope

What the investigation covers, and what it deliberately does not. Out of scope is where a spike's boundaries hold — an investigation with no stated limit expands to fill the timebox and beyond.

### Output

A design document in the Confluence space, reviewed and ratified by the team, reaching a conclusion.

**Producing and socializing the design document is the developer's responsibility.** The spike does not close on a document that has been written but not ratified.

## Acceptance criteria

Spike criteria are **process conditions, not runnable data checks.** The categories in `acceptance-criteria.md` do not apply — those govern data product deliverables. The task-versus-criterion test still does: every criterion states what must be true, verifiable without observing who did what.

Standard set:

- The investigation question is answered.
- Findings are documented in a design document in the Confluence space.
- A recommendation is made with its rationale stated.
- The design document has been reviewed and approved by the lead engineer or technical lead.
- The design output has been assessed for architectural decisions above ticket scope. Where the design extends or establishes an architectural pattern, that pattern is ratified as one of: conforms to an existing pattern, extends an existing pattern, or establishes a new pattern — each requiring technical lead review and approval.

That last criterion is the scope gate. A design document produced under a single ticket must not silently set architectural precedent. See `decomposition.md`, Mode 4.

**Do not add a criterion requiring the developer to create follow-up implementation tickets.** Decomposing a spike outcome into build work is the Product Owner's responsibility, not the engineer's.

## Timebox

Two checkpoints, both stated in the ticket:

- **Within 3 working days of pickup** — something reviewable is posted to Confluence for the team to react to
- **By end of sprint** — fully ratified and approved per the acceptance criteria, or the spike does not close

The first checkpoint is what keeps a spike from consuming a sprint silently. The second is what closes it.

## Rules

- The question comes from the grooming transcript where one exists. If the transcript does not contain a clear statement of the unknown, say so rather than inferring one.
- Success criteria are defined before the spike starts, never after.
- Do not restate the build ticket. Reference it.
- Do not assign ticket-writing to the engineer.
- No OKR tag is strictly required, but apply it if available — it is automatic.
- Neutral and brief. A spike ticket should be shorter than the story it unblocks.
