# Definition of Ready

The intake gate. Assesses whether a ticket is **engineering-ready** — buildable, unambiguous, testable, dependency-mapped.

## What this does and does not assess

**Assesses:** can someone build this, and will we know when it is right.

**Does not assess:** whether the work is worth doing. Business need is validated upstream by the BA and the requirement owner. Valid pushback is *ambiguous*, *unbuildable*, *untestable*, *no upstream contract*. Not *not worth building*.

**Run as a guideline through refinement, not as a waterfall stage-gate.** Conditions are worded to be objectively checkable so the assessment does not become a debate — but the assessment is continuous, and the ready call is the Product Owner's. An automated readiness check is an input, never the gate.

**Conventions are not readiness conditions.** OKR tagging, target gate declaration, and outcome-framed titles are core principles of ticket writing. A ticket missing them is non-conforming, not unbuildable. They are not assessed here.

---

## Universal conditions

Apply to every ticket type.

**Functional clarity**
- The functional change is stated: what the system does after this that it does not do now
- Scope of the change is bounded — what is included, and what is explicitly out
- Out of scope is present and derived, not merely recalled. Where the stated outcome is broader than the provided means can deliver, the residual is named and attributed

**Acceptance criteria**
- Present, and written per `acceptance-criteria.md`
- Every criterion passes the task/criterion test — states what must be true, not what someone must do
- No unverifiable verbs without a concrete condition attached
- Criteria cover the stated scope, with nothing implied but unstated

**Dependencies**
- Cross-team and upstream dependencies captured as native Jira issue links with the correct link type
- Each dependency carries a status: confirmed, pending, or blocked

**Scope and feasibility**
- No open architectural question embedded in the ticket. If implementation approach is undecided, this is a spike, not a story
- Buildable on the current platform. Capability gaps surface as routed dependencies, not as ticket content
- Estimable — the team can size it. The Product Owner does not size it

---

## Type-specific conditions

Additional to the universal set. Assess only those applicable to the ticket type.

### User story — build

- Source system, tables, and target tables named
- Grain, semantics, and historization (SCD or bitemporal) unambiguous
- Data model specification linked, not inlined
- Stable, versioned upstream data contract exists. **No contract, not ready** — say so and name who owns it
- Source data present, accessible (Unity Catalog grants in place), and profiled
- Filing types, volumes, and expected cardinality stated where applicable

### Data onboarding

- Raw source and source location named
- History start date for backfill stated
- Go-forward cadence stated
- Databricks volume path provided
- Target L1 table name provided
- Parser ownership confirmed — whether the capability team provides it, or the data engineering team builds it
- The scope split is stated: what is within the data engineering team's control to correct, and what is a capability-team defect to be raised

### Spike

- The investigation question is specific and stated
- Success criteria defined — what a done spike produces. **Defined before the spike starts**
- Downstream work named: which build tickets this unblocks
- Timebox stated

### Empirical monitoring

- Daily count history provided for the table
- Primary key provided
- Referential mapping provided where the table is L3: source tables and join key
- Real data exists in the target environment. A monitoring ticket against a table with no data is not ready

### Initiative summary

- Business context sufficient to write the business-facing sections: named consumer, what capability is unavailable to them today, the consequence, and what becomes possible. Stated without reference to implementation. Technical scope alone is not sufficient
- Business capability being delivered is stated, with its consumer
- OKRs named (IP-OKR-###)
- Segment named
- Epics identified, or their shape known
- Scope boundary stated and derived
- Upstream source owners named. An initiative with unnamed source owners has unowned data contracts beneath every epic
- Capability dependencies named with their providing teams
- Delivery horizon stated

### Catalog comments

- Dataset comments file attached — columns `dataset`, `description`
- Column comments file attached — columns `dataset`, `column`, `column description`
- Target catalog and schema named
- Target environment stated
- Every dataset and column named in the files exists in the target environment. Descriptions for objects that do not yet exist are not ready

### Epic

- Business context sufficient to write the business problem and business impact: named consumer, what they cannot do today, the consequence, and what they will do with the deliverable. Technical scope alone is not sufficient
- Rolls up to a named initiative
- Tables or datasets delivered are named, with grain
- Stakeholder set complete, including the upstream source owner
- Scope boundary stated
- Dependency sequence stated, with external dependencies marked

---

## Outcomes

**Ready** — every applicable condition met.

**Partial** — one or more unmet. Each unmet condition is named specifically, with its owning party.

**Not ready** — the functional change or the acceptance criteria are undefined.

State unmet conditions as neutral fact. Never as a blame narrative. Name the specific condition, not the category: "upstream contract not versioned," not "upstream issues."

---

## Two load-bearing conditions

Do not cut these in later revisions without deliberate reasoning.

**Stable, versioned upstream data contract.** This is the accountability firewall expressed as a checkable condition. Where an item enters a sprint against an undefined or unstable contract, the record shows the condition was assessed and named before work began.

**No open architectural question in the ticket.** Routes spikes correctly and prevents architectural decisions being made silently at ticket level. It is the intake-side counterpart to the design scope assessment applied at spike closure and at ticket close — the same failure caught at both ends of the lifecycle.

---

## How this improves

Do not wait to notice patterns across sprints. **The partial and not-ready outcomes are the instrument.** Every failed assessment names an unmet condition and an owning party. The frequency summary in `backlog-readiness-assessment.md` aggregates them.

At v0.3:
- Conditions that never fire — cut them
- Conditions that fire constantly — either a systemic upstream gap worth raising, or a condition worded too tightly
- Failure modes the gate missed — add conditions

Revision is evidence-driven, not recalled.

---

*v0.2 — supersedes v0.1. Changes: reframed from general readiness to engineering-readiness per the delivery-PO boundary; conventions (OKR, target gate, outcome framing) moved out to core principles; acceptance criteria condition now references `acceptance-criteria.md` rather than restating it; type-specific conditions added for all five ticket types; access and profiling added; estimable added.*
