# Acceptance Criteria

The standard for what makes a criterion acceptable. Owned here, referenced everywhere — `user-story.md`, `spike.md`, `data-onboarding.md`, `empirical-monitoring.md`, and `definition-of-ready.md` all point at this file. Do not restate these rules elsewhere.

## The test

**A criterion states what must be TRUE when the work is complete. A task states what someone must DO.**

Apply to every line before it enters the acceptance criteria:

> Can this be checked as true or false without observing who did what?

If no, it is a task. Move it.

| Task | Criterion |
|---|---|
| Configure Datadog monitors | Pipeline run status, record counts, and job duration are observable in Datadog |
| Set up the Unity Catalog grants | The consumer role has SELECT on the target table |
| Add data quality checks | `cik` is not null in all rows |
| Implement retry logic | Given an HTTP 5xx from the source, when the pipeline retries 3 times without success, then the run fails and alerts route to CAP Data Engineering |
| Register the table in the catalog | The table is registered in the data catalog with column descriptions populated |

The conversion is usually mechanical: state the end condition rather than the action, and drop any reference to how it is achieved.

**Where tasks go**
- Ticket-specific work → subtasks, or the Scope section
- Recurring standard work (catalog registration, naming conventions, monitor setup) → the Definition of Done. Reference it; never restate it in AC

## Form

Two forms only. Assigned by category, not by preference — nobody should be deciding per criterion.

**Assertion** — default. States a condition that must hold.

**Given / when / then** — only where behavior is conditional on a trigger. An event occurs, the system responds.

Do not wrap a simple assertion in given/when/then. "Given the table is loaded, when I query it, then `cik` is not null" is worse than `cik is not null`, and it moves the criterion away from the form it must end up in as a test.

## Categories

Not every category applies to every ticket. Each ticket-type reference declares which are required for that type. Generate only the applicable ones.

### Data model / schema conformance — *assertion*
The delivered structure matches the specification.

Reference the spec artifact; do not inline the column list. Where a data contract exists, schema conformance is covered by the contract — assert conformance to it rather than restating columns.

> The target table conforms to the data model specified in [link], including column names, types, and nullability.

### Data quality — *assertion, written as a runnable check*
Must be expressible as a dbt test, DLT/Lakeflow expectation, or Monte Carlo custom SQL. Write it so it can be lifted into that form without translation.

Name the field. Name the rule. No unverifiable verbs.

> `cik` is not null in all rows.
> `exchange` is one of: NYSE, NASDAQ, AMEX.
> The table contains no duplicate rows for the declared primary key.

### Complete data capture — *assertion*
All expected records are present. State the expectation and its source.

> All accession numbers present in the L2 source tables are present in the target table.
> Every filing published by the source for the run date is captured.

### History and bitemporality — *assertion*
How change is recorded. State the grain and the versioning behavior.

> The table is bitemporal on `valid_from` / `valid_to` and `system_from` / `system_to`.
> A change to a source record closes the prior version and opens a new one; prior versions are retained.
> Exactly one row per primary key is current at any point in system time.

### Scheduling — *assertion*
When the pipeline runs. Include timezone.

> The pipeline runs daily at 06:00 UTC.

### Pipeline observability — *assertion* for what is observable, *given/when/then* for notification behavior

> Run status, record counts, and job duration are observable in Datadog.
> Given a pipeline run fails, when the failure is detected, then an alert routes to the CAP Data Engineering channel.

### Monitoring — *assertion*
Monitors configured, with their breach conditions and routing. Full specification in `empirical-monitoring.md` — reference it rather than deriving thresholds here.

> `<table>_daily_volume` is configured in Monte Carlo with breach condition `record_count < <min> OR record_count > <max>`, priority P2, routing to the CAP Data Engineering channel.

### Error handling and alerting — *given / when / then*
The only category that is given/when/then by default. State the trigger, the system response, and the notification.

> Given an HTTP 5xx from the source endpoint, when the pipeline retries 3 times without success, then the run fails and an alert routes to the CAP Data Engineering channel.
> Given a record fails schema validation, when the pipeline processes the batch, then the record is written to the reject table with a failure reason and the run continues.

## Specificity

A criterion that could apply to any table is not specific enough. Name:

- **Tables and columns** — actual names, not "the target table"
- **Thresholds** — numbers, not "reasonable" or "acceptable"
- **Schedules** — times with timezone, not "daily"
- **Channels** — the actual destination, not "the appropriate channel"
- **Values** — enumerated sets, not "valid values"

## Banned verbs

Never use without a concrete condition attached: handle, support, ensure, improve, optimize, appropriately, as needed, as applicable, properly, correctly, sufficient, reasonable.

If a criterion needs one of these to make sense, it is underspecified. Ask for the missing detail rather than writing it vaguely.

## Coverage

The criteria must cover the functional change stated in the description, with nothing implied but unstated.

Before finalizing, check both directions:
- Does every element of the stated scope have a criterion?
- Does every criterion trace to something in the stated scope?

Criteria with no corresponding scope are scope creep. Scope with no criteria is an untestable deliverable.

## Parent tickets and subtasks

When work splits into subtasks, **acceptance criteria live on the subtask that delivers them. Never duplicate them on the parent.**

- **Parent holds:** description, scope boundary, out of scope, the data model reference, and criteria that span the whole deliverable
- **Subtask holds:** the criteria for its own scope, and nothing already stated on the parent

The parent's own criteria should be few — usually the integration-level conditions that no single subtask can assert alone. If the parent has no such conditions, it has no acceptance criteria of its own, and that is correct.

Duplication across parent and subtask makes tickets long, makes the source of truth ambiguous, and slows delivery. Do not do it.

## Rules

- Every line in the AC section passes the true/false test. Anything that does not is a task and moves.
- Assertion by default; given/when/then only for triggered behavior.
- Generate only the categories declared applicable by the ticket-type reference.
- Name the specifics. Ask for missing detail rather than writing around it.
- Never restate a standard that has a canonical home — link it.
- No duplication between parent and subtask.
