# ODCS Quality Export

The declaration shape for §6a, §6b and §6c rules, so that projecting them into an ODCS `quality` block is a field rename rather than a translation.

**This file owns the table. `data-contract.md` owns whether and how a rule projects.** Authoring in this shape does not decide what reaches a contract — go there for projection policy, the pinned version, and reconciliation.

**Open question — §6c.** `data-contract.md` states runtime monitors do not project, on the grounds that a monitor is an operational control rather than a consumer promise. ODCS does carry `schedule` and `scheduler` on quality rules, so monitors *can* be expressed. Authoring §6c in this shape costs nothing either way and makes the choice reversible. **Until that policy changes, §6c rows are authored here and projected nowhere.**

---

## The declaration table

One row per rule. Same shape for §6a, §6b and §6c.

| ID | Name | Description | Type | Dimension | Condition | Threshold | Unit | Severity | Business impact | Schedule |
|---|---|---|---|---|---|---|---|---|---|---|
| `13F-D-003` | Filing identity | Each filing appears once. A breach means totals including it are overstated | `library` | `uniqueness` | `duplicateValues` on `accession_number` | `mustBe: 0` | `rows` | `error` | Manager-level position totals overstate | — |
| `13F-D-011` | Holding count reconciles | Loaded positions match the count the filer declared | `sql` | `completeness` | Count of `holdings_detail` rows per filing, less that filing's `table_entry_total` | `mustBe: 0` | `rows` | `warning` | Positions may be missing for affected filings | — |
| `13F-M-002` | Holdings volume | Positions loaded per week. A breach means recent filings may not have loaded | `sql` | `completeness` | Count of `holdings_detail` rows loaded in the trailing 7 days | `mustBeBetween: [<lo>, <hi>]` | `rows` | `warning` | Totals by manager understate until resolved | `0 6 * * *` |

### The Condition column

**State the condition. Never write the query.**

For a `library` rule the metric *is* the condition — `duplicateValues` on `accession_number` names what must hold without naming how to compute it. Carry it as declared.

For a `sql` rule the condition is written in words: what is counted or compared, over what, against what. The SQL itself is engineering's, written at implementation and recorded alongside the binding.

| Not a condition | Condition |
|---|---|
| `SELECT count(*) - max(table_entry_total) FROM …` | Count of rows per filing, less that filing's declared `table_entry_total` |
| `SELECT count(*) WHERE load_ts > now() - 7` | Count of rows loaded in the trailing 7 days |
| `GROUP BY cusip HAVING count(*) > 1` | Count of `cusip` values appearing more than once |

The test is the same one that governs tickets: **if the statement would change when the implementation changes, it is not a condition.** A query names tables, dialects and access paths, all of which change without the rule changing.

The condition plus the threshold is complete. Nothing is missing from it that a developer needs.

### Companion table — fields ODCS does not model

| ID | Threshold source | Calibration | Binding *(engineering)* |
|---|---|---|---|
| `13F-D-003` | Specification | — | |
| `13F-M-002` | Observed history | median ± 4×MAD, widened; baseline 8 quarters | |

**Threshold source and calibration project as `customProperties`.** ODCS has no concept of threshold provenance, and losing it makes a specification-sourced rule indistinguishable from a calibrated one on re-import — the distinction DoR criterion 22 exists to protect.

**The binding never projects at all.** Engineering's check identifier is specification content, not contract content. It stays in the companion table, written at acceptance. See `data-contract.md`.

---

## Field mapping

| Our column | ODCS field | Notes |
|---|---|---|
| ID | `id` | Append-only. Stable reference for re-import |
| Name | `name` | Short identifier. Read in an alert |
| Description | `description` | What the check does and what a breach means |
| Type | `type` | `library`, `sql`, `text`, `custom` |
| Dimension | `dimension` | See values below |
| Condition | `metric` (library) | Carried as declared |
| Condition | `query` (sql), `implementation` (custom) | **Engineering supplies these at implementation.** The specification declares the condition in words; the query is written against it and recorded with the binding |
| Threshold | the comparison operator | Written in ODCS syntax already — copied, not converted |
| Unit | `unit` | `rows`, `percent` |
| Severity | `severity` | Mapped from our Blocking / Non-blocking |
| Business impact | `businessImpact` | Consequence for use of the data |
| Schedule | `schedule` (+ `scheduler`) | Monitors only. Assertions run on build, not on a clock |
| Threshold source | `customProperties` | No ODCS equivalent |
| Calibration | `customProperties` | No ODCS equivalent |
| Binding | **does not project** | Specification-only. See `data-contract.md` |

---

## Permitted values

**Type** — `library` where a standard metric fits, `sql` otherwise. `text` only for a rule not yet executable; `custom` only where an engine is already chosen.

**Library metrics** — `nullValues`, `missingValues`, `invalidValues`, `duplicateValues`, `rowCount`. Prefer these over hand-written SQL: they are portable across engines and need no query maintained.

**Dimension** — `accuracy`, `completeness`, `conformity`, `consistency`, `coverage`, `timeliness`, `uniqueness`.

**Operators** — `mustBe`, `mustNotBe`, `mustBeGreaterThan`, `mustBeGreaterOrEqualTo`, `mustBeLessThan`, `mustBeLessOrEqualTo`, `mustBeBetween`, `mustNotBeBetween`.

**Severity mapping**

| Ours | ODCS |
|---|---|
| Blocking | `error` |
| Non-blocking | `warning` |

Runtime monitors carry a severity too, even though they cannot gate a merge (criterion 20). The severity says how a breach should be treated, not when the rule runs.

---

## Rules

**Write the threshold in ODCS syntax.** `mustBeBetween: [1000, 99900]`, not "between 1000 and 99900". Prose thresholds get interpreted at export, and interpretation is where the error enters.

**Every rule carries a dimension.** It is optional in ODCS and mandatory here — a rule set with no dimensions cannot be reported on by category, which is most of the value of exporting at all.

**Business impact is a consequence, not a restatement.** "Duplicate rows present" describes the breach. "Manager-level position totals overstate" is what it costs. The second is what a consumer needs.

**Prefer a library metric over SQL.** A `duplicateValues` rule survives an engine change; a query does not — and a library rule needs no query written at all.

**`query` and `implementation` are filled at implementation, like the binding.** A specification carrying a hand-written query has authored the implementation, and the query then has to be maintained by whoever wrote it rather than by the team that owns the pipeline.

**Schedule is for §6c only.** A §6b assertion runs when the asset builds. Populating a schedule on one makes it a monitor, which changes when it is allowed to fail.

**Never project a calibrated threshold without its companion row.** A number with no recorded provenance is indistinguishable from a declared one, and the next person to touch it cannot tell whether it may be re-derived from data.

**The binding stays blank until acceptance** and never leaves the specification.

**Resolve every field name against the pinned standard before emitting**, per `data-contract.md`. The names here are written against ODCS v3.x and the mapping is close enough to be done by eye and different enough to be done wrongly.

---

## Sources

- [ODCS Data Quality specification](https://bitol-io.github.io/open-data-contract-standard/latest/data-quality/)
- [Open Data Contract Standard overview](https://bitol-io.github.io/open-data-contract-standard/latest/)
- [Quality rule types](https://docs.datacontract.com/quality-rules)
