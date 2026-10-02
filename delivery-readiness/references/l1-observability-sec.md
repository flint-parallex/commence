# L1 Observability — SEC Connector

The standard observability slate for every L1 onboarding story sourced from the SEC connector.

**This is a reusable slate, not a per-ticket design.** Every SEC L1 asset has the same shape — the index says what exists, the connector lands it — so the same three things are worth watching every time. A ticket reuses this and supplies only its per-asset values.

Applied through `monitoring-subtask.md`. Thresholds are calibrated per `../../specification-authoring/references/monitor-calibration.md`.

`<angle brackets>` mark values the ticket supplies.

---

## The index is the oracle

The EDGAR index lists every filing that exists for a period. That is what makes L1 observability simple: **we do not have to infer whether the connector got everything, we can compare against the index.**

```
EDGAR index  →  header table  →  filings table
   (truth)        (landed)         (landed)
```

Completeness is the comparison. Everything else is a property of the landed tables.

This is why L1 needs no bespoke rules per asset. The question is never *is this data right* — L1 lands what was filed — it is *did all of it arrive, and is it still arriving*.

---

## 1 · Completeness — index against landed

The primary L1 control. Compare what the index lists against what landed, keyed on filed date.

| Condition | Threshold | Type | Dimension |
|---|---|---|---|
| Count of filings in the EDGAR index for the filed-date range, absent from the header table | `mustBe: 0` | `sql` | `completeness` |
| Count of filings in the EDGAR index for the filed-date range, absent from the filings table | `mustBe: 0` | `sql` | `completeness` |

**Both tables are checked, not just one.** A filing can reach the header table and fail on the detail parse. Checking only the header passes that case; checking only the filings table cannot distinguish a parse failure from a fetch failure.

**Keyed on filed date, not period of report.** Filed date is when the connector should have seen it. A filing for a prior period filed late is a filing the connector was expected to pick up on the day it appeared.

**A drop is a count and a list.** The monitor reports the count; the investigation needs the accession numbers. State that the check returns them.

---

## 2 · Freshness — the landed tables

Measured on filed date advancing in the landed table, not on run completion.

A run that completes having landed nothing is fresh by run time and stale in fact. Run status belongs in Datadog; freshness belongs on the table.

| Condition | Threshold | Type | Dimension |
|---|---|---|---|
| Days since the maximum filed date in the table advanced | `mustBeLessOrEqualTo: <n>` | `sql` | `timeliness` |

**`<n>` is calibrated per asset from the form's filing pattern**, then capped at the SLO less correction time — per `monitor-calibration.md`. SEC filing arrival is deadline-concentrated, so the gap between filings outside a deadline window is legitimately long and the threshold has to accommodate it.

---

## 3 · Out-of-the-box Monte Carlo monitors

**Enabled on every L1 table.** All three, each confirmed individually.

| Monitor | Note |
|---|---|
| Freshness | Out-of-the-box, in addition to the declared rule above. The declared one carries the spec-sourced threshold; this one carries the observed-history baseline |
| Schema | Fires on an EDGAR schema revision. These are announced but arrive on EDGAR's schedule |
| Volume | **Rolling window, not daily.** See below |

**On volume windows.** SEC filings arrive in bursts around statutory deadlines — 13F at 45 days after quarter end, most of a quarter in the final week. Outside those windows daily volume is sparse and irregular, and a daily threshold either triggers constantly or is widened until it detects nothing.

Size the window to the form's filing pattern and record the pattern as the justification. Same-day detection of a total load failure comes from freshness, not volume.

---

## Required tagging

**Every observability ticket carries this, not only SEC L1.** The tags are what make a monitor findable and reportable; an untagged monitor fires into a queue nobody can filter.

| Tag | Value | Why |
|---|---|---|
| Product | `<product>` | Groups monitors by data product for health reporting |
| Environment | `<dev / uat / prod>` | A UAT breach read as production is a false incident; the reverse is a missed one |
| Table | `<catalog>.<schema>.<table>` | Fully qualified. Resolves the monitor to the asset without inference |

**All three must be populated in Monte Carlo**, not only recorded in the ticket. A tag stated in a ticket and absent from the tool is a tag that does not exist.

This is an acceptance criterion on every observability ticket, and it is verified in the validation subtask.

---

## Datadog — pipeline observability

Per `monitoring-subtask.md` Layer 1, plus two specific to a filing-based source.

```
  - Run status on success and failure
  - Run duration
  - Input row count and output row count
  - Filings fetched vs filings landed, per run
  - Filings rejected at parse, per run, with the form type
  - Failure alert routed to <channel>
```

**Fetched versus landed catches in-run loss.** A run that fetches 400 filings and lands 380 completes successfully. The completeness check against the index catches it on the next evaluation; this catches it in the run.

**Rejected-at-parse is reported, never silently dropped.** A source schema change usually shows up here first.

---

## Not asserted at L1

Recorded so a reviewer does not read their absence as an omission.

| Not here | Why |
|---|---|
| Semantic validity of a reported value | L1 lands what was filed. A filer error is data, not a defect |
| Identifier resolution — CUSIP to instrument, CIK to entity | Reference resolution is a downstream layer |
| Cross-filing consistency | A manager filing differently across periods is disclosure, not an error |
| Unit normalisation | Handled where the unit is derived, not at landing |
| Grain and uniqueness beyond accession number | L1 has no derived grain to assert |

**A filer error must not fail an L1 monitor.** L1 asserts the filing landed completely and is still arriving — not that the filer was right.

---

## Acceptance criteria

On the parent story. Referencing the tables rather than restating thresholds — see `acceptance-criteria.md`.

```
- Ensure the completeness rules compare the EDGAR index against both the header and
  filings tables, keyed on filed date, and return the accession numbers of any absent
  filings
- Ensure the freshness rule is configured on each L1 table with a threshold calibrated
  from the form's filing pattern and capped at the SLO
- Ensure Monte Carlo out-of-the-box freshness, schema and volume monitors are enabled on
  every L1 table, each confirmed individually, with volume on a justified rolling window
- Ensure every monitor is tagged in Monte Carlo with product, environment and fully
  qualified table
- Ensure Datadog receives the pipeline events, including filings fetched vs landed and
  filings rejected at parse
- Ensure every binding is recorded in the specification
```

---

## Validation

The validation subtask (`monitoring-subtask.md`) adds a tagging check to the standard evidence table:

```
  - Every monitor resolves in Monte Carlo with product, environment and table populated
  - Report any monitor with a missing or incorrect tag as UNTAGGED
```

**An untagged monitor fails validation even if it runs correctly.** It will not appear in product health reporting, which means it is invisible exactly when someone is looking for it.

---

## Rules

**Reuse the slate; do not re-derive it.** A per-ticket observability design for an L1 SEC asset produces drift with no benefit.

**Completeness compares against the index.** Never against a prior run, a row count, or an expectation — the index is the only oracle that says what should exist.

**Check both landed tables.** Header-only passes a detail parse failure.

**Freshness is on filed date in the table, never on run completion.**

**Tagging is an acceptance criterion, not housekeeping.** Product, environment, fully qualified table, populated in the tool.

**The ticket supplies the freshness threshold input and the volume window.** Both are per-asset; report them as open questions rather than guessing.

**Monitors asserting connector properties belong in `shared/`, not a dataset file.** They apply to every SEC-derived asset unchanged. See `../../delivery-assessment/references/check-authoring.md`.
