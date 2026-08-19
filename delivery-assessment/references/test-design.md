# Test Design

Proposes runnable tests at two scopes: **from a ticket's acceptance criteria**, and **as a standing suite for a data product**.

## Tests versus monitors

Owned by `../../specification-authoring/references/assertion-authoring.md`, which defines three classes — unit, data, and runtime monitor — with severity and threshold source per class.

The rule that matters here: **a test threshold comes from the specification; a monitor threshold may come from observed history.** Never derive one from the other. Do not generate monitors in this file — those are declared in specification §6c per `runtime-monitors.md`.

## Available checks

Before proposing a test, read `unit_tests/checks_reference.md` in the delivery repository. It lists what `pipeline_library.data_quality` provides and what exists locally in `unit_tests/shared/`.

**Propose tests that map to available checks.** A proposal made of generic SQL, when a library check already covers the assertion, creates maintenance for no gain. Where nothing maps, say so — that is either a criterion needing rewriting or a new shared check, per `check-authoring.md`.

For placement of a new check, the tests-versus-monitors boundary, and how criteria connect to checks, see `check-authoring.md`.

## Required inputs

- **Implementation target** — pytest against Databricks is the established pattern in this environment; confirm rather than assume
- **The specification**, cited at a commit. §4 supplies the data model, grain, keys and relationships; §6 supplies the assertions
- For Mode 1: the ticket and its acceptance criteria
- For Mode 2: the data product and its constituent tables

Where the specification does not declare something a test needs, ask. Do not source it from the data model file or from the tables.

Data samples are used only to set thresholds where a criterion requires one. **Never derive a test from observed data alone** — that encodes current behavior as correct, which is the opposite of what a test does.

---

## Mode 1 · Tests from acceptance criteria

**Input:** a ticket. **Output:** one proposed test per acceptance criterion, in the implementation target's syntax, ready to paste into the ticket.

Because acceptance criteria are already written as runnable checks per `acceptance-criteria.md`, this is translation rather than interpretation. If a criterion cannot be translated, that is a defect in the criterion — flag it rather than inventing a test around it.

**Where a criterion cites an assertion ID**, carry the ID onto the test. That is what lets a Mode 1 test be promoted into the Mode 2 suite without losing its trace.

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

### The suite is built from the specification, not re-derived

**Every test traces to a declared assertion.** Specification §6b already declares uniqueness, key nullability, schema conformance, referential integrity and accepted values, each carrying an assertion ID. Do not re-derive these from the data model — a suite derived independently can disagree with the specification, and nothing reconciles the two.

Read §6a and §6b and organize them into a runnable suite. Each test carries the ID of the assertion it implements.

**This makes coverage a diff.** Assertions in §6 with no test in the suite are uncovered. Tests in the suite with no assertion behind them are scope that was never declared. Both are reportable without judgment.

**Where an assertion has a check file** at `products/<product>/checks/<ASSERTION-ID>.sql`, the suite references it rather than restating the logic.

**Where the suite needs a test that §6 does not declare**, that is a gap in the specification, not a test to add quietly. Report it back — the specification is where assertions are decided.

### Product-specific tests

The product's own §6a and §6b assertions beyond the common set — grain and historization behaviour, bitemporal currency (exactly one current row per key), stage conservation, completeness against source, cross-table consistency where a product spans tables.

§6a assertions are written as input/output pairs and translate to tests directly, including the negative cases. A pair set with only well-formed inputs is a specification defect, not a suite to fill in.

### Suite structure

Organize by table. State each test's assertion ID and its class, so the suite can be reconciled against §6 without reading the test bodies.

### Growth

The suite is extended per delivery, never rebuilt. When a ticket adds a table or a column, its Mode 1 tests are promoted into the suite. When a production incident reveals a gap the suite did not catch, the question is which assertion would have caught it. Where one exists and was untested, add the test. Where none exists, the gap is in specification §6 and the incident record says so — see `REPO-STRUCTURE.md` on incidents.

---

## Rules

- Ask for the implementation target. Output in that syntax, not generic SQL.
- Never derive a test from observed data alone. Tests assert specification, not current behavior.
- A criterion that cannot be translated is a defect in the criterion. Flag it; do not invent a test.
- Report coverage in both directions — untested criteria and untraced tests.
- Do not generate monitors here. Runtime monitors are declared in specification §6c per `runtime-monitors.md`.
- Check `unit_tests/checks_reference.md` before proposing a test. Do not reimplement an available check.
- Placement of new checks follows `check-authoring.md`. Logic does not belong in a dataset file.
- Every test carries the ID of the assertion it implements. A test with no assertion behind it is undeclared scope.
- The suite is built from specification §6, never re-derived from the data model.
- The suite is extended, never regenerated wholesale.
- Where a test is added in response to an incident, record the incident and the assertion ID.
- Neutral and brief.
