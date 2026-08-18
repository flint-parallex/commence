# Verification

What may be checked against real tables, and what may never be sourced from them.

This is the discipline that makes platform-connected authoring an advantage rather than a liability. An agent with query access will, by default, profile a table and propose values from what it finds. That behaviour is correct for verification and corrupting for specification.

## The three questions

Before any query, state which one is being asked.

| Question | Name | Permitted | Output |
|---|---|---|---|
| Does the declared fact hold in the data? | **Verification** | Yes | A finding |
| What content exists that no declared rule covers? | **Discovery** | Yes | A question |
| What should the declared fact be? | **Sourcing** | No — for anything in sections 4, 5 or 6 | A statement |

Verification takes a statement that already exists and tests it. Discovery looks for content the statements do not reach and reports the gap. Sourcing takes the data and produces a statement.

The same query can serve all three. The difference is what comes out: verification and discovery hand the operator something to decide, sourcing decides for them.

**Observed history is never a test oracle** (DoR criterion 22). A threshold derived from what the pipeline currently produces can never fail, because it was defined by the thing it is meant to check. It will pass on the day the product silently breaks.

## What may be verified

Each of these takes a declared statement and reports whether it holds.

| Declared in the specification | Verification |
|---|---|
| Grain — one row is exactly what | Count rows against distinct declared key. Report whether uniqueness holds |
| Primary and composite keys | Test uniqueness and null-freedom |
| Relationship cardinality | Test the declared cardinality. Report actual fan-out where it exceeds what was declared |
| Nullability per column | Report columns declared non-null that contain nulls, and columns declared nullable that never do |
| Logical type | Report where the physical type cannot carry the declared logical type |
| Column existence | Report declared columns absent from the table, and table columns absent from the specification |
| Accepted values | Report values present that the specification does not accept |
| Referential integrity | Test that declared foreign keys resolve |

## What may never be sourced

Nothing that becomes a statement in sections 4, 5 or 6 may originate from a query.

| Never sourced from data | Why |
|---|---|
| Grain | The grain is a decision about what the product represents. A grain read off the data records what the pipeline currently emits, which is the thing the grain is meant to constrain |
| Keys and uniqueness | Same. A key inferred from observed uniqueness will hold until the day it matters |
| Accepted values | An enumeration built from observed distinct values accepts whatever error is already present |
| Assertion thresholds | Criterion 22. A threshold from observed history cannot fail |
| Nullability | Observed nulls describe a state; declared nullability describes a permission |
| Cardinality | A relationship observed as one-to-one in current data may be one-to-many by design |
| Business reason | Not present in the data at all |
| Required end state | Describes intent. The data describes outcome |

## The three exceptions

Three fields may be **informed** by observation, and all three must be confirmed by the operator and recorded as requirements rather than observations.

- **Expected volume and growth** (criterion 28) — observed volume is legitimate evidence for a stated expectation. State the requirement, not the measurement.
- **Query patterns** — observed usage is evidence for what consumers need. Confirm with the consumer; a pattern in the logs is not a stated use (criterion 3).
- **Cost envelope** — observed cost informs the envelope. The envelope is a decision.

In each case: propose, attribute to observation, and ask. Never record as fact.

## Discovery

Finding an edge case is reconnaissance. Deciding what the specification says about it is not.

This is the mode that makes platform-connected authoring worth having. A specification written from intent alone covers the cases its author thought of. Real data contains the cases nobody thought of, and those are where products break — a field that is nominally a number and occasionally a range, a code that changed meaning mid-history, a relationship that is one-to-one everywhere except for one filer.

**Discovery is permitted because its output is a gap, not a rule.** The agent surveys, reports what is not covered, and stops.

### What to survey

For a free-text or weakly-typed field, where declared parse rules exist:

| Survey | Finds |
|---|---|
| Distribution of value shapes — empty, all digits, digits with separators, contains letters, contains only punctuation | Content the declared cases do not name |
| Frequency per shape | Which uncovered cases matter and which are one filer's mistake |
| Values appearing once or twice | The long tail, where the genuinely strange content lives |
| Minimum and maximum length, minimum and maximum numeric value | Boundary content the rules may not survive |
| Shape by time period | Content that appears only after a date — usually an upstream schema or convention change |
| Shape by source, filer, or form type | Whether an uncovered case is systemic or attributable to one producer |

For a declared model generally:

| Survey | Finds |
|---|---|
| Values outside a declared accepted set | Enumerations that are incomplete |
| Nulls in columns declared non-null | Either a bad load or a wrong declaration |
| Observed cardinality against declared | Relationships that fan out where the model says they do not |
| Keys resolving in neither direction | Orphans, and parents whose children are gone |
| Columns present that the specification does not describe | Scope the specification has not caught up with |
| Rows at the type boundary — maximum length, maximum magnitude, zero, negative | Content a declared type cannot carry |

### Reporting

```
DISCOVERY — <solution>, run <date>, against <catalog.schema>

UNHANDLED CASES
| Observed shape | Frequency | Example | Covered by | Decision required |
|---|---|---|---|---|
| <shape> | <count and share> | <redacted or public example> | <declared rule, or "none"> | <the question for the operator> |

SYSTEMIC OR ISOLATED
  <whether each uncovered case concentrates in one producer, one period, or is spread>

NOT SURVEYED
  <what the survey could not reach and why>
```

### Rules

**Report the case; never write the rule.** An uncovered shape produces a question — *what should the product do with a value of this form?* — not a proposed assertion. The rule is the operator's, and a proposed rule in the same output as the discovery will be accepted rather than decided.

**Report frequency; never derive a threshold from it.** Observing that 1.8% of references fail to resolve is a finding. Setting the monitor threshold at 2% because of it is sourcing, and criterion 22 forbids it. State the observation and ask.

**Say whether a case is systemic or isolated.** One filer producing malformed content every quarter and two hundred filers producing it once are different problems with different answers, and the frequency alone does not distinguish them.

**Run discovery before authoring the parse rules, not after.** Discovery against data the rules were already written from tells you only that the rules match the data they came from.

**Discovery on a product that does not yet exist runs against the source, not the target.** Survey the landing data or the source documents. The absence of a built table is not a reason to author parse rules blind.

## Reporting findings

A verification failure never edits the specification.

The specification states what must be true. The data may be wrong — a declared grain that does not hold is frequently a defect in the product, not an error in the document. Silently reconciling the specification to the data destroys the only thing that would have caught it.

Report in this form:

```
VERIFICATION — <solution>, run <date>, against <catalog.schema>

HOLDS
  <declared fact> — confirmed

DOES NOT HOLD
  <declared fact>
    Declared: <what the specification says>
    Observed: <what the data shows>
    Reading: <the specification is wrong | the data is wrong | cannot be determined here>

NOT VERIFIABLE
  <declared fact> — <why: table absent, insufficient data, not testable by query>

IN DATA, NOT IN SPECIFICATION
  <columns, values or relationships present that the specification does not describe>
```

**The reading is required and is often "cannot be determined here."** Say so rather than choosing. A declared grain that does not hold may mean the grain was wrong, the pipeline is wrong, or the specification describes a target state the current build has not reached. Only the operator knows which.

**Never propose a specification edit in the same output as a finding.** Report, then wait. An edit offered alongside a failure invites reconciliation to the data, which is the failure mode this whole file exists to prevent.

## Where verification runs

Verification applies to an existing build. A specification for a product that does not yet exist has nothing to verify against, and that is normal — most specifications are authored before the tables exist.

Run verification when:

- Revising a specification for a product already in production
- Authoring a specification for a product built before the standard existed
- Checking that a delivered product still matches what was specified
- Before acceptance, alongside the binding check (criterion 23)

Do not delay authoring for lack of tables. Author from intent; verify when there is something to verify.
