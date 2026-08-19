# Capability Test Plan

The plan for testing a capability delivered to the data engineering team by another team.

This is an **acceptance boundary document** as much as a test plan. It states what will be tested, what will not, what must be true before testing starts, and what constitutes success. Those boundaries are what prevent an unready capability being absorbed as though it were working.

Write it before testing begins, and get it agreed by the providing team.

---

## Source handling

Sources are commonly meeting transcripts, chat threads, and email.

**Extract commitments and conditions as statements. Never quote people.** A transcript records a conversation; this document records what was agreed. Timestamped quotes with attribution read as evidence-gathering, date the document to a meeting rather than to the agreement, and make it awkward to circulate.

> The capability team will complete internal testing before handoff.

Never: *"At 14:32 [name] said they'd finish internal testing first."*

Where the sources do not establish something the plan needs, list it as an open item rather than inferring agreement.

---

## Structure

### Purpose

What the capability is, and what is being requested of this team. Two or three sentences.

### In scope

What will be tested. The overview, not the procedure.

### Out of scope

Explicitly not tested, and why. Required.

Derive it: where the request implies an outcome broader than this testing can establish, name the residual and state where responsibility sits. Include anything the capability will not be tested against, and any condition under which results would not be valid.

This section is what stops a passing test being read as a broader endorsement than it was.

### Deliverables

What the providing team explicitly needs from this team for testing to be considered done. Enumerated. If it is not on this list, it was not agreed.

### Roles and responsibilities

| Role | Person | Responsibility |
|---|---|---|

### Assumptions

What must be true before testing starts. These are **preconditions, largely owned by the providing team** — what they have built, tested, and confirmed.

State each as a condition, with its owner. If an assumption proves false, testing does not start, and the record shows the precondition was stated in advance.

### Open items

| Item | Owner | Due | Status |
|---|---|---|---|

Everything that must be completed by anyone for testing to reach an outcome. This is the tracking section — commitments from every party, in one place.

---

## Test methodology

### Phases

| Phase | Description | Entry criteria | Exit criteria |
|---|---|---|---|

Each phase declares what must be true to begin and what must be true to complete. A phase without entry criteria will be started before it is ready.

### Scope of test subjects

What the capability will be run against. Enumerate the specific subjects — number and identity.

**Complexity spread** — the tiers used, with the definition of each. State how many subjects fall in each tier.

Record any open question affecting the test subjects, such as whether full data or sample data is used, as an open item rather than resolving it silently.

### Metrics

**Acceleration.** For each test subject, record:

| Field | |
|---|---|
| Start event | What is recorded as the beginning |
| Checkpoint | Where one exists |
| End event | What is recorded as completion |
| End-to-end elapsed time | |
| Baseline | The historical build this is compared against |

State the baseline source explicitly — which historical builds, and how their elapsed time was established. A comparison against an unstated baseline is not a measurement.

**Usability and friction.** For each subject, record:

- Friction points, described factually — what was unclear, blocked, or required workaround
- Iteration count — how many passes were needed to reach an acceptable output
- Enhancement candidates — specific, actionable, not general dissatisfaction

Friction points are recorded as observations about the capability, never as complaints about the team that built it.

---

## Success criteria

The exit criteria for UAT, agreed with the providing team. Each stated so it can be marked met or not met.

Typical shape:

- The pipeline runs to completion and produces the final output table
- Rework required is minor and cosmetic, with no significant logical errors
- Measured acceleration is material and acceptable to the Product Manager

Define "minor and cosmetic" and "material" in the plan. Undefined thresholds are decided after the fact, by whoever has the strongest opinion.

---

## Test procedure

The steps followed for each test subject, and what is captured at each step. Numbered, repeatable, identical across subjects — otherwise the results are not comparable.

## Test record template

The structure used to capture each subject's results: subject, tier, metrics, friction points, iteration count, outcome. One record per subject.

## Risks to the test

What could invalidate or block the test — unstandardized inputs, incomplete prerequisites, environment availability, subject availability. Each with an owner.

Distinguish risks to the *test* from risks in the *capability*. The first belong here; the second are findings.

## Final deliverables

What this team produces at the end: results, metrics summary, friction log, enhancement candidates, and the recommendation.

---

## Publication and layering

**One page, in the consumer space (DPNO). Not split across spaces.** Splitting the plan from its results fragments the record and forces a reader to chase links. Instead, the single page is layered so a stakeholder lands on what they need and execution detail sits below the reading path.

**Visible on landing** — everything through success criteria:
Purpose · In scope · Out of scope · Deliverables · Roles and responsibilities · Assumptions · Open items · Test methodology (phases, entry and exit criteria, scope of subjects, metrics definitions) · Success criteria

This is what a stakeholder needs before sign-off, and putting the exit criteria in front of them *before* results exist is the point of an agreed gate.

**Inside an expander** — `Testing details`:
Test procedure · Test record template · Test results and entries

Engineering writes results directly into this section. It stays honest and complete without dominating the page. Note that an expander controls the reading path, not access — anyone can open it. Do not place anything in it that should not be read.

**Visible below** — Issue log. What was found stays in front of the reader.

**Not published to the consumer space at all:**
Action items · Owner tracking · Anything following the issue log

These are delivery management, not stakeholder information. They stay in the internal record.

**IT space** — a link to this page. No mirrored content. One canonical location.

**Validation summaries** append beneath the plan as dated entries, per `validation-report.md`. A reader then lands on one page carrying the agreed criteria and the current status against them.

## Rules

- Never quote individuals from transcripts. Extract commitments as statements.
- Out of scope is required and derived.
- Assumptions are preconditions with owners, stated before testing starts.
- Every phase has entry and exit criteria.
- Baselines are named. A comparison against an unstated baseline is not a measurement.
- Define subjective thresholds in advance — "minor," "material," "acceptable."
- Friction is an observation about the capability, never a complaint about a team.
- Where the sources do not establish something, it is an open item — not an inference.
- One canonical page in the consumer space. Never mirror content into the IT space — link to it.
- Test procedure, test record, and results go inside the `Testing details` expander. Action items and anything after the issue log are never published to the consumer space.
- Published in Confluence storage format per `confluence-formatting.md`. Open items and deliverables as task lists with assignees.
- Neutral and brief.
