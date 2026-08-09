# Test Design

Proposes runnable tests at two scopes: **from a ticket's acceptance criteria**, and **as a standing suite for a data product**.

## Tests versus monitors

Both assert things about data. They are not the same artifact and should not be conflated.

| | Test | Monitor |
|---|---|---|
| Asserts | Intent — what was specified | Behavior — what is happening |
| Runs | At build, in CI, before acceptance | Continuously, in production |
| Failure means | The build does not meet the spec | The world changed |
| Owned by | The ticket or the product suite | `empirical-monitoring.md` |

An assertion can legitimately appear in both. Do not derive one from the other automatically — a test threshold comes from the specification, a monitor threshold comes from observed history.

## Available checks

Before proposing a test, read `unit_tests/checks_reference.md` in the delivery repository. It lists what `pipeline_library.data_quality` provides and what exists locally in `unit_tests/shared/`.

**Propose tests that map to available checks.** A proposal made of generic SQL, when a library check already covers the assertion, creates maintenance for no gain. Where nothing maps, say so — that is either a criterion needing rewriting or a new shared check, per `check-authoring.md`.

For placement of a new check, the tests-versus-monitors boundary, and how criteria connect to checks, see `check-authoring.md`.

## Required inputs

- **Implementation target** — pytest against Databricks is the established pattern in this environment; confirm rather than assume
- **Data model** — columns, types, nullability
- **Grain** and **primary key**
- **Referential relationships** — source tables and join keys, where applicable
- For Mode 1: the ticket and its acceptance criteria
- For Mode 2: the data product and its constituent tables

Data samples are used only to set thresholds where a criterion requires one. **Never derive a test from observed data alone** — that encodes current behavior as correct, which is the opposite of what a test does.

---

## Mode 1 · Tests from acceptance criteria

**Input:** a ticket. **Output:** one proposed test per acceptance criterion, in the implementation target's syntax, ready to paste into the ticket.

Because acceptance criteria are already written as runnable checks per `acceptance-criteria.md`, this is translation rather than interpretation. If a criterion cannot be translated, that is a defect in the criterion — flag it rather than inventing a test around it.

> Criterion: `cik` is not null in all rows.
> Test: `not_null` on `cik`.

> Criterion: The table contains no duplicate rows for the declared primary key.
> Test: `unique` on the primary key column set.

> Criterion: All accession numbers present in the L2 source tables are present in the target table.
> Test: relationship assertion from target to each source on `accession_number`.

**Output format.** For each criterion: the criterion quoted, the proposed test in target syntax, and one line on what a failure means. Group by table.

**Coverage check, required.** Every criterion has a test, or a stated reason it cannot be automated. Every test traces to a criterion. Report both directions — criteria without tests, and tests without criteria. A test with no criterion behind it is scope that was never agreed.

**Flag rather than infer:**
- A criterion that cannot be expressed as a check — the criterion needs rewriting
- A criterion needing a threshold not stated in the ticket — ask for the threshold
- A criterion referencing a relationship not declared in the data model — ask for the relationship

---

## Mode 2 · Product test suite

**Input:** a data product and its tables. **Output:** a standing test suite that accumulates as the product grows.

This is the more valuable artifact. A suite built once and extended per delivery becomes the product's regression protection — and the shared components carry across every product.

### Shared components — every product, every table

Proposed by default from the data model. These require no ticket and no interpretation:

- **Primary key uniqueness** — on the declared key set
- **Primary key not null** — every key column
- **Schema conformance** — column set, types, nullability match the model
- **Referential integrity** — target keys exist in each declared source
- **Accepted values** — on any column with an enumerated domain in the model

Where a data contract exists, schema conformance is asserted against the contract rather than restated.

### Product-specific tests

Derived from the product's own criteria — grain and historization behavior, bitemporal currency (exactly one current row per key), completeness against source, cross-table consistency where a product spans tables.

### Suite structure

Organize by table, with shared components first and product-specific tests after. State for each test whether it is a shared component or product-specific, so the shared set can be regenerated when it changes without touching bespoke tests.

### Growth

The suite is extended per delivery, never rebuilt. When a ticket adds a table or a column, its Mode 1 tests are promoted into the suite. When a production incident reveals a gap the suite did not catch, a test is added and traced back to the incident — that is the preventive backlog item made concrete.

---

## Rules

- Ask for the implementation target. Output in that syntax, not generic SQL.
- Never derive a test from observed data alone. Tests assert specification, not current behavior.
- A criterion that cannot be translated is a defect in the criterion. Flag it; do not invent a test.
- Report coverage in both directions — untested criteria and untraced tests.
- Do not generate monitors here. Thresholds derived from history belong in `empirical-monitoring.md`.
- Check `unit_tests/checks_reference.md` before proposing a test. Do not reimplement an available check.
- Placement of new checks follows `check-authoring.md`. Logic does not belong in a dataset file.
- Shared components are proposed from the data model, not from a ticket.
- The suite is extended, never regenerated wholesale.
- Where a test is added in response to an incident, record which incident.
- Neutral and brief.
