# Data Onboarding

Landing a new source and making it queryable. **L0** is raw files in Databricks volumes. **L1** is those files parsed into variant type and queryable.

This is not building a dataset. There is no business data model to conform to and no consumer-facing transformation. The deliverable is **coverage and fidelity**: everything that landed in L0 is represented in L1, parsed without error, backfilled to the historization date, and safe to re-run.

Use `user-story.md` for anything that transforms or models data. Use this for getting a source in.

## The scope boundary — read first

**The data engineering team does not build the parser.** Parsing capability commonly comes from a capability team, though this varies by source — some sources are fully covered by a capability team, others are not. Confirm per source; do not assume either way.

This makes the scope split load-bearing on every onboarding ticket:

**In scope** — everything within the data engineering team's custom builds or within its control to modify, adjust, or correct at the pipeline level.

**Out of scope** — correcting capability-team capabilities that are producing dataset-level issues. These are raised to the Product Owner so communication with the external team can begin. The engineer does not work around a capability defect, and the ticket is not judged against one.

State this explicitly on every onboarding ticket. It is the clearest available case of the derivation in `user-story.md`: the outcome (clean parsed data) is broader than the means provided (a parser owned by another team), and the residual is named and attributed rather than absorbed.

## Required inputs

Ask for any missing. Never invent a source, a date, or a volume path.

**Always**
- Raw source — where the filings are pulled from
- OKR tag (IP-OKR-###)
- Target environment — typically UAT for a first landing
- Capabilities leveraged, and which team provides each. List as capability dependencies

**For L0**
- Source location for the filings
- History start date the backfill must reach
- Go-forward cadence — how frequently the pipeline runs
- Databricks volume path

**For L1**
- Target L1 table name

## Structure

Follow `user-story.md` section order: Overview, Scope of Work (out of scope, in scope, known constraints), Acceptance criteria.

The story statement is still consumer-outcome framed. For onboarding, the outcome is availability:

> As a *[consumer]*, I need *[source]* data available and queryable from *[date]* forward so that *[business value]*.

## Acceptance criteria

Per `acceptance-criteria.md` — assertion form, task/criterion test applies. The categories below replace the standard user story set.

### L0 — landing

- History is loaded from *[start date]* for all filing types provided by the source.
- The ingestion pipeline is scheduled at *[cadence]*.
- The ingestion job is idempotent — re-running against already-loaded data produces no duplicate records.

### L1 — parsing

- All records present in the L0 dataset are represented in the L1 table.
- The L1 table contains no parse errors.
- The L1 table contains no duplicate records.
- Data in the L1 table is queryable in *[target environment]*.
- The L1 job is idempotent — re-running produces no duplicate data.

Coverage and idempotency are the two that matter most and the two most often assumed rather than asserted. Coverage is what proves nothing was silently dropped between layers; idempotency is what makes a re-run safe after a partial failure, which is when it will actually be needed.

Where parse errors originate in a capability-team parser, the criterion is not met — and that is a capability defect to raise, not a pipeline defect to work around. The out-of-scope statement is what makes that distinction hold at acceptance.

## Rules

- Confirm parser ownership per source. Do not assume the capability team owns it, and do not assume they do not.
- The scope split is stated on every onboarding ticket, not just where a capability dependency is known.
- Coverage is asserted, never assumed.
- Both jobs assert idempotency.
- Reference the data model only where one exists — L1 is variant type, so schema conformance criteria generally do not apply.
- Empirical monitors are out of scope on a first landing; they require real data. They belong on a separate ticket per `empirical-monitoring.md`.
- Neutral and brief.
