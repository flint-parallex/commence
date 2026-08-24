# Monitor Calibration

Derives observed-history thresholds from approved historical data, and specifies the monitors that carry them.

This is the implementation companion to `runtime-monitors.md`. That file decides **which** monitors exist, what they measure, over what window, and which threshold source they take. This one derives **the numbers** for the observed-history ones and gives the SQL.

**Specification-sourced monitors are not calibrated here.** Their threshold is the declared rule (DoR criterion 22), and nothing in this file applies to them.

All monitors are **Monte Carlo manual monitors implemented as custom SQL**. Every monitor returns a boolean or a comparison result: **true means breach, alert fires.**

## Governing assumption

All provided historical data has been UAT-approved by the business. **No pattern present in the provided data should ever trigger an alert.** Thresholds are derived to be generous. When in doubt, widen.

**State the baseline window and its assumption in specification §6c.** The derivation treats the supplied period as correct. If that period contains an incident nobody caught, calibrating to zero breaches encodes the miss as normal, permanently. The assumption is usually sound and always worth writing down.

**Where a threshold was widened to cover an outlier, record what it can no longer detect.** Zero breaches proves the monitor will not be noisy; it proves nothing about whether it would catch anything.

**Widening applies to approved history with backfill already excluded.** Thresholds are grounded in a robust statistic and then widened to cover that history — never derived from the extremes themselves. Where a single period forces a large widening, say so: an unrecorded incident inside the baseline becomes permanently undetectable once encoded as normal.

## Required inputs

Ask for any missing. Never invent thresholds, keys, or table relationships.

1. **Daily count file** (CSV, one per table) — columns: date, record count
2. **Primary key file** (CSV) — columns: table name, primary key. If any key is composite, ask for the delimiter convention before generating
3. **Referential mapping** (only if generating referential monitors) — for each L3 table: the L2 source tables that feed it and the join key. Ask inline if not supplied
4. **Resolution timeline link** — leave as `[TBD]` placeholder if not supplied

## Monitor tiers

**Required, every table**
- Volume (empirical threshold)
- Freshness (empirical threshold)
- Primary key uniqueness

**Required, L3 tables**
- Referential completeness — count variance
- Referential completeness — key-level

**Optional, per request**
- Null rate on a named field

Schema conformance is covered by the data contract. Do not generate a schema monitor.

**Monitors 3, 4 and 5 below are specification-sourced, not empirical.** Primary key uniqueness and referential completeness take their rule from specification §4 and §6b. They appear here because this file carries their SQL, but they are never calibrated — the threshold is zero, always.

Bitemporal and SCD tables are covered by the primary key uniqueness monitor. Do not generate a separate partition monitor unless asked.

---

## Threshold derivation

### Step 1 — Exclude backfill rows

Working from the start of the series, exclude leading rows where the record count exceeds **5× the median of the following 30 rows** (or the following window, if fewer rows exist). Stop excluding at the first row that does not meet this condition. Exclude at most 5 leading rows.

**Report every exclusion** — date and count. A backfill the operator did not know about is itself information.

Do not exclude any other rows. Do not trim tails. Backfill exclusion is the only outlier handling.

### Step 2 — Volume thresholds

**Ground the threshold in a robust statistic, then widen. Never derive it from the observed minimum and maximum.**

The extremes are the two least stable values in any sample. A single anomalous day sets the bound permanently, and the bound is by construction determined by the most extreme observation in the window — which is the observation least likely to represent normal behaviour. A multiplier applied to an extreme inherits the instability rather than correcting it.

From the remaining rows, compute the **median** and the **median absolute deviation**:

```
median = P50(record_count)
MAD    = median( | record_count − median | )
```

MAD is used rather than standard deviation because a single spike inflates standard deviation and drags the bounds with it. MAD is unaffected by a small number of extreme values, which is what makes it usable on data that has not been cleaned.

**Base bounds:**

```
lower = median − (k × MAD)
upper = median + (k × MAD)
```

`k = 4` is the default. It is deliberately generous — over-alerting destroys a monitoring surface faster than a missed detection does.

**Where MAD is zero or near-zero** — a table loading an identical count every period — the spread is genuinely degenerate. Fall back to a percentage band around the median (±20% as a starting point) and record that MAD was unusable rather than reporting bounds equal to the median.

**Then widen to cover approved history.** No day remaining after backfill exclusion may fall outside the bounds. Where one does, widen to include it and state which date forced it and by how much.

**Widening covers approved history only.** Backfill is already excluded; do not trim tails and do not drop a period for looking wrong. Where a day forces a large widening, say so explicitly — a single day expanding the band by more than half is more likely an unrecorded incident than normal variation, and encoding it as normal makes the monitor permanently unable to detect a repeat.

**Record what the widening can no longer detect.**

**Deviating from `k = 4`** is permitted where a product's volatility genuinely does not fit it. Record the value used and the reason in the derivation. Silent substitution is what makes thresholds unreviewable.

Report: rows included, rows excluded, median, MAD, `k`, base bounds, any widening with the date and magnitude that forced it, final bounds, and the verification result.

### Step 3 — Freshness threshold

Compute the gap in days between consecutive dates, excluding the backfill period.

- **Cadence** = the modal gap, not the mean. A weekly table with one holiday gap has a mean of 8.2 and a mode of 7
- **Empirical threshold** = observed maximum gap × 1.5, rounded up, minimum of cadence + 1

**Then cap at the service level.** The empirical figure establishes what is normal; the SLO establishes what is permissible. The threshold is the lower of the two:

```
threshold = min( empirical threshold, SLO − correction time )
```

A freshness monitor set at the published promise fires at the moment the promise breaks, which is too late to act on. The correction-time allowance is what leaves room to detect and fix before a consumer is affected — see `runtime-monitors.md` and DoR criterion 7.

**Where the cap produces a threshold tight enough to trigger on normal variation, that is a finding, not a tuning problem.** It means the SLO is not achievable at the current refresh cadence. Report it and stop; do not widen past the SLO to make the monitor quiet. Widening here hides a commitment the product cannot meet.

Report: modal gap, mean gap, observed maximum gap, number of distinct gap values, the empirical threshold, the SLO, the correction-time allowance, and which of the two bound the final threshold. If gaps are highly irregular, say so — the cadence may not be reliably inferable.

---

## Monitor specifications

### 1 · Volume

```sql
SELECT COUNT(*) AS record_count
FROM <table>
WHERE <load_date_column> = CURRENT_DATE()
```

**Breach condition:** `record_count < <min>` OR `record_count > <max>`

### 2 · Freshness

```sql
SELECT DATEDIFF(CURRENT_DATE(), MAX(<load_date_column>)) AS days_since_load
FROM <table>
```

**Breach condition:** `days_since_load > <threshold>`

### 3 · Primary key uniqueness

```sql
SELECT COUNT(*) AS duplicate_groups
FROM (
  SELECT <primary_key_columns>
  FROM <table>
  GROUP BY <primary_key_columns>
  HAVING COUNT(*) > 1
)
```

**Breach condition:** `duplicate_groups > 0`

### 4 · Referential completeness — count variance

L2 → L3. No filtering occurs between layers, so all source records must land.

```sql
WITH source AS (
  SELECT COUNT(DISTINCT <join_key>) AS source_count
  FROM (
    SELECT <join_key> FROM <l2_table_a>
    UNION
    SELECT <join_key> FROM <l2_table_b>
  )
),
target AS (
  SELECT COUNT(DISTINCT <join_key>) AS target_count
  FROM <l3_table>
)
SELECT
  source_count,
  target_count,
  (source_count - target_count) / source_count AS variance_pct
FROM source, target
```

**Breach condition:** `variance_pct > 0.01`

Use `UNION` on distinct keys, not `SUM` of counts — a key present in both source tables would otherwise double-count and produce a permanent false shortfall.

### 5 · Referential completeness — key-level

Counts matching does not prove the right records landed: 100 dropped and 100 duplicated nets to zero variance. This monitor catches actual loss and returns the missing keys with their source.

```sql
WITH source AS (
  SELECT <join_key>, 'a' AS source_table FROM <l2_table_a>
  UNION ALL
  SELECT <join_key>, 'b' AS source_table FROM <l2_table_b>
),
missing AS (
  SELECT s.<join_key>, s.source_table
  FROM source s
  LEFT JOIN <l3_table> t ON s.<join_key> = t.<join_key>
  WHERE t.<join_key> IS NULL
)
SELECT COUNT(*) AS missing_count FROM missing
```

**Breach condition:** `missing_count > 0`

Include the `missing` CTE select in the ticket as a diagnostic query so engineering can list the missing keys and their source table on breach.

---

## Alert configuration

Every monitor carries this block. Generate one per monitor.

| Field | Value |
|---|---|
| Dataset | `<table name>` |
| Monitor name | `<table>_daily_<type>` — type is `volume`, `freshness`, `pk`, `referential_count`, `referential_keys` |
| Description | `Daily <type> monitor for <table>` |
| Criteria | The SQL and breach condition above |
| Rules | Any filter conditions. Omit if none |
| Priority | P2 unless specified |
| Issue detected | Per type, below |
| Customer impact | Per type, below |
| Resolution timeline | `[link]` |
| Pipeline owner | CAP Data |

**Issue detected / customer impact by type:**

- **Volume** — "The dataset does not have the expected incremental daily volume." / "The customer may not have complete data for the product."
- **Freshness** — "The dataset has not refreshed within the expected window." / "The customer may be working with stale data."
- **Primary key** — "The dataset contains duplicate records for the declared primary key." / "The customer may see duplicated records in the product."
- **Referential (both)** — "The dataset is missing records available in its source tables." / "The customer may not have complete data for the product."

Operator may override any wording.

---

## Ticket structure

Follow `user-story.md` for section order. Monitoring-specific content:

**Description** — one to two sentences: empirical monitors configured for `<table>`, covering volume, freshness, primary key uniqueness, and (for L3) referential completeness.

**Out of scope** — required. Standard exclusion for a first build: *ML-based anomaly monitors, which require production data history not yet available.* Add others as applicable.

**Scope — work included** — one line per monitor to configure. Completion is verified by the acceptance criteria, not by this list.

**Data model** — reference the spec artifact. Do not inline.

**Acceptance criteria** — per `acceptance-criteria.md`. One criterion per monitor, assertion form:

> `<table>_daily_volume` is configured in Monte Carlo with breach condition `record_count < <min> OR record_count > <max>`, priority P2, routing to the CAP Data Engineering channel.

**Derivation appendix** — include the threshold derivation report: rows included and excluded, excluded dates with counts, observed values, proposed thresholds, and the verification result. This is what makes the thresholds auditable rather than asserted.

---

## Rules

- **Show the derivation.** Never assert a threshold without the observed values it came from and the verification line.
- **Warn-only first.** Every new monitor lands in warn-only for an agreed observation window before routing to a channel. Untuned monitors produce noise, noise gets muted, and muted alerting is worse than none. State the window in the ticket.
- Ask for the primary key. Never infer it from column names.
- Ask for the referential mapping. Never infer which L2 tables feed an L3 table.
- If fewer than 30 days of history are provided, generate the thresholds and state prominently that the baseline is thin and should be revisited.
- State dependency and data status as neutral fact. Never as a blame narrative.
