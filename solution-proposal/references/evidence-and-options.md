# Evidence and Options

The two disciplines that make a proposal survive contact with the people who must approve it.

## Evidence

The load-bearing claim in most proposals is that the current state cannot meet the need. That claim will be tested, sometimes by the people who built or bought the current state.

**Every failure claim traces to an observation.**

| Field | Content |
|---|---|
| Claim | What cannot be done |
| Observation | The measurement, with table, column and date |
| Consequence | What this prevents, in business terms |

> **Claim:** change on the client side cannot be detected.
> **Observation:** `<table>.<timestamp column>` is null in 100% of rows, measured `<date>`, `<row count>` rows.
> **Consequence:** a match resolved by the client is not visible to us. Records already resolved are re-sent for review.

**A claim without an observation is removed, not softened.** Hedging language — *appears to*, *may not fully* — reads as an unverified claim rather than a careful one, and it invites the reader to discount the claims that *are* evidenced.

### Distinctions to hold

**Broken versus missing.** A capability that fails is a defect. A capability that was never built is absent. Both may need solving, and conflating them turns a scope statement into an accusation.

**Local versus systemic.** Where the same failure affects other consumers of the same asset, say so — and state explicitly whether this proposal fixes it for them. A proposal that fixes one dataset on a shared asset will be assumed to have fixed the shared problem, and the assumption will not be corrected until someone else's product breaks.

**Observed versus inferred.** *We observed X* and *therefore Y must also be true* are different statements. Mark inferences as inferences. An inference presented as an observation is the thing that discredits an otherwise sound proposal.

### Neutrality

State what the system does and does not do. Never characterise the people who built it, chose it, or approved it — they may be reading, and they may be approving this.

> The inherited pipeline loads in one direction. Timestamps are unpopulated, so change detection is not possible in either direction.

Not: *the pipeline was built without any thought to incremental loading.*

Same facts. The first is a basis for a decision; the second is a basis for an argument about the past.

## Options

**Record every option considered, including doing nothing.**

A recommendation presented alone invites *did you consider X* — and later, someone reverses the decision because they cannot see what it was weighed against. This is criterion 18 applied to an approach rather than to a build rule: a material decision carries its reason, or it gets undone by accident.

### Per option

| Field | Content |
|---|---|
| Option | What it is, in one line |
| Meets | Which functional requirements it satisfies, by ID |
| Does not meet | Which it does not, by ID |
| Cost | Effort, elapsed time, dependencies. Supplied by the team, attributed |
| Risk | What could go wrong, and what would be unrecoverable |
| Why not chosen | Required for every option except the recommendation |

**Assess options against the functional requirements, not against each other.** Comparative language — *better*, *cleaner*, *more robust* — is opinion. `FR-003` and `FR-007` unmet is a fact, and it is the same fact for any reader.

### Doing nothing

Always an option, always recorded. What the business does in the meantime, what degrades, and what the cost is of continuing.

Where doing nothing is genuinely untenable, the evidence section has already shown why, and this row is short. Where it is tenable, the proposal must be honest that it is — a proposal that presents an optional change as unavoidable is discovered eventually, and it costs credibility on the next one.

### The recommendation

State what it gives up. A recommendation with no downside has not been examined, and a reader who finds an unstated downside will assume it was concealed rather than missed.

**Do not decide.** Present the recommendation and its reasoning. The decision, its date, and who made it belong in the decision record — written after, not in advance.
