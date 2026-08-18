# Data Solution Specification — `13f-institutional-holdings`

| Field | Value |
|---|---|
| Data Solution | `13f-institutional-holdings` |
| Version | `v0.1` |
| Author | J. Okafor — Senior Analyst, Data Product & Operations *(invented)* |
| Date | `2026-08-18` |
| Written against | Definition of Ready v0.3 — `<confluence link>` |
| Status | Draft — contains invented placeholder content, see Provenance |

---

## 1 · Solution Definition

**Purpose**

Institutional investment managers exercising discretion over $100M or more in listed US equity securities are required to disclose their positions quarterly. Those disclosures are the only broad public record of who holds what across the institutional market, and they are the basis for ownership analysis, position-change tracking, crowding and concentration measurement, and peer comparison. This Solution makes that record queryable as a coherent set of positions attributable to named managers over time, rather than as a collection of individually-filed documents. It exists because the disclosure as filed cannot be analysed directly: a single filing may report positions on behalf of several managers, and the identifiers linking a position to the manager responsible for it are local to the filing that carries them.

**Business entity and grain**

The reported institutional holding. One row at Solution level is one security position, as reported, within one filing, for one reporting period.

**Owner**

J. Okafor — Senior Analyst, Data Product & Operations. *(invented)*

**Support channel**

`#dp-institutional-holdings` *(invented)*

**Consumers** *(invented — criterion 3 requires these be confirmed with each named team)*

| Consumer | Their stated use |
|---|---|
| Equity Research Analytics | Crowding and concentration measurement across institutional holders for a given security |
| Portfolio Risk | Peer ownership overlap between managed portfolios and disclosed institutional positions |
| Client Reporting | Quarterly ownership snapshots included in client-facing security profiles |

---

## 2 · Service Commitments

**Required refresh cadence**

Daily during filing season. 13F filings are due 45 days after quarter end, and arrive in a heavy concentration in the final week of that window. Consumers require positions to be available for analysis within one business day of the filing appearing on EDGAR.

**Delivery windows**

| Field | Value |
|---|---|
| Required delivery time | 06:00 ET |
| Calendar basis | Business days, US market holiday calendar. EDGAR does not accept filings on federal holidays |
| Upstream availability | EDGAR publishes continuously during the acceptance day, 06:00–22:00 ET |
| Processing window | 08:00 ET to 06:00 ET next day — approximately 8 hours after the acceptance window closes |
| Downstream dependency | Ownership Analytics daily refresh, starts 06:30 ET *(invented)* |
| Missed-window behaviour | The prior day's positions remain available and current. Consumers analysing the incomplete period will understate manager coverage; the period is marked open until delivery completes |
| Late-arriving data | Yes. Filings are accepted up to and past the 45-day deadline, and amendments arrive indefinitely. A period is never closed in the sense of receiving no further records |
| Recovery expectation | Within one business day. The missed period is filled, never skipped — a skipped day permanently omits every filing accepted that day |

**Seasonality**

Filing volume is extremely concentrated. 13F filings are due 45 days after quarter end, and a large majority of a quarter's filings arrive in the final week of that window. Outside that week, daily volume is sparse and irregular. This concentration is why volume and growth monitors are set on a rolling window rather than daily — see section 6c.

**Reprocessing and corrections**

Corrections arrive as new records, never as replacements. An amendment is a separate submission with its own accession number, and the original remains present and unchanged. The Solution **records** restatements and does not apply them: `amendment_type` distinguishes a restatement that replaces prior holdings from one that adds to them, and the consumer selects. History is immutable; a consumer reading a prior period today gets the same answer they got yesterday, plus any amendments that have since arrived.

**Service levels**

| Measure | Target (SLO) | Published promise (SLA) |
|---|---|---|
| Freshness | Filings available within 18h of EDGAR publication | Within 1 business day |
| Availability | 99.5% | 99% |
| Timeliness | Full quarter complete within 3 business days of the 45-day deadline | Within 5 business days |

**Scale and cost**

| Dimension | Expectation |
|---|---|
| Expected volume | ~6,000 filings per quarter; ~5–8M holding rows per quarter |
| Growth trajectory | Filing count broadly flat; holding rows growing with position counts |
| Query patterns | Manager-over-time, security-over-time, point-in-time market ownership snapshot |
| Cost envelope | Under $400/month serverless SQL and storage *(invented)* |

---

## 3 · Sources and Dependencies

**Sources**

| Source | Contract reference | Stability | Notes |
|---|---|---|---|
| EDGAR primary document (`primary_doc.xml`) | `eis_13F_Filer.xsd` | Versioned; changes announced via EDGAR release notes | Cover page, signature block, summary page, other-manager declarations |
| EDGAR information table (`*.xml`) | `eis_13FDocument.xsd` | Versioned; changes announced via EDGAR release notes | Position-level disclosure |
| EDGAR full index | EDGAR daily/quarterly index | Stable | Accession numbers, filed dates, form types |

**Canonical references**

| Reference | Authoritative source | Governed by |
|---|---|---|
| SEC state and country codes | EDGAR `stateOrCountry` code list | `<data governance owner>` |
| CUSIP → instrument | `<instrument reference solution>` | `<owner>` |
| CIK → filing entity | EDGAR company database | `<owner>` |

**Upstream change behaviour**

Where a source document fails schema validation, the filing is quarantined in full and not partially loaded — a filing whose envelope parsed but whose information table did not would otherwise present as a manager reporting zero positions, which is materially misleading. Where the schema version changes and introduces an unrecognised element, the filing loads and the unrecognised element is retained unparsed; the change is raised rather than silently dropped.

**Shared and conformed dimensions**

SEC state and country codes; the CIK entity reference; the CUSIP instrument reference. All referenced, none redefined here.

---

## 4 · Data Assets

### Asset: `filing_master`

**Grain**

One row is one 13F filing submission, identified by its EDGAR accession number.

**Columns**

| Column | Logical type | Meaning | Units | Nullable | Classification |
|---|---|---|---|---|---|
| `accession_number` | string | EDGAR's unique identifier for the submission. The only identifier stable across all assets in this Solution | — | No | Public |
| `submission_type` | string | Filing variant as submitted: `13F-HR` holdings report, `13F-NT` notice that holdings are reported by another manager, `13F-CTR` confidential treatment request, and their `/A` amendment forms | — | No | Public |
| `cik` | string | Central Index Key of the entity that submitted the filing. Not necessarily the manager holding the securities | — | No | Public |
| `period_of_report` | date | The quarter-end the filing reports on. The temporal anchor for all analysis; the filing date is not | — | No | Public |
| `report_calendar_or_quarter` | date | Quarter end as stated on the cover page. Duplicates `period_of_report` in the overwhelming majority of filings; retained because divergence indicates a filer error worth surfacing | — | No | Public |
| `is_amendment` | boolean | Whether this submission amends a previously accepted filing | — | No | Public |
| `amendment_number` | integer | Sequence of this amendment for the filer and period | — | Yes | Public |
| `amendment_type` | string | `RESTATEMENT` replaces the prior filing's holdings entirely; `NEW HOLDINGS` adds to them. The distinction determines whether prior positions remain in force and cannot be inferred from the document alone | — | Yes | Public |
| `filing_manager_name` | string | Name of the manager as stated on the cover page. Free text; not a controlled identifier and not consistent across periods for the same entity | — | No | Public |
| `filing_manager_street1` | string | Cover page address line 1 | — | Yes | Public |
| `filing_manager_street2` | string | Cover page address line 2 | — | Yes | Public |
| `filing_manager_city` | string | Cover page city | — | Yes | Public |
| `filing_manager_state_or_country` | string | Cover page state or country, as an SEC code. Resolves against the SEC state and country reference | — | Yes | Public |
| `filing_manager_zip_code` | string | Cover page postal code | — | Yes | Public |
| `report_type` | string | What the filing reports: a holdings report, a notice, or a combination report where the manager reports some positions itself and others are reported by an included manager | — | No | Public |
| `form_13f_file_number` | string | The manager's 13F file number, assigned by the SEC in the `028-#####` series | — | Yes | Public |
| `crd_number` | string | Central Registration Depository number where the manager is a registered adviser. Added to the schema in the January 2023 release; absent on earlier filings by construction, not by omission | — | Yes | Public |
| `sec_file_number` | string | The manager's SEC file number. Added in the same January 2023 release | — | Yes | Public |
| `provide_info_for_instruction_5` | string | Whether the manager is providing information required under Instruction 5, which concerns positions omitted from the public table under confidential treatment | — | Yes | Public |
| `signature_name` | string | Name of the person signing | — | Yes | Public |
| `signature_title` | string | Title of the signatory | — | Yes | Public |
| `signature_phone` | string | Contact phone as filed | — | Yes | Public |
| `signature_city` | string | Place of signature | — | Yes | Public |
| `signature_state_or_country` | string | Place of signature, SEC code | — | Yes | Public |
| `signature_date` | date | Date of signature. Distinct from the filing date and from the period of report | — | Yes | Public |
| `other_included_managers_count` | integer | The manager's own count of other managers included in this filing, as stated on the summary page. The filer's assertion about the filing's composition, and therefore a reconciliation target rather than a derived value | — | Yes | Public |
| `table_entry_total` | integer | The manager's own count of rows in the information table. A reconciliation target | — | Yes | Public |
| `table_value_total` | integer | The manager's own total of the value column. A reconciliation target, subject to the same unit change described under `holdings_detail.value` | see `value` | Yes | Public |
| `filed_date` | date | Date EDGAR accepted the submission | — | No | Public |

**Keys and uniqueness**

`accession_number` is the primary key. Unique across all periods and all filers.

**Relationships**

| From | To | Cardinality | Implication for the result |
|---|---|---|---|
| `filing_master` | `associated_filers` | 1 : 0..n | A filing may include no other managers, or many. Joining without aggregating multiplies filing-level facts by manager count |
| `filing_master` | `holdings_detail` | 1 : 0..n | A `13F-NT` filing has no holdings by design. Zero rows is valid and must not be treated as a load failure |

**History behaviour**

Immutable once accepted. A correction arrives as a new filing with its own accession number, never as a change to an existing row. Amendments are additional rows, not updates.

**Breaking change classification**

Removal, rename or retype of any column is breaking. Removal of the `accession_number` uniqueness constraint is breaking. Addition of columns as EDGAR extends the schema is not.

---

### Asset: `associated_filers`

**Grain**

One row is one manager other than the filer, declared on the cover page of one filing.

**Columns**

| Column | Logical type | Meaning | Units | Nullable | Classification |
|---|---|---|---|---|---|
| `accession_number` | string | The filing this declaration belongs to | — | No | Public |
| `manager_sequence_number` | integer | The ordinal the filer assigns to this manager on the cover page. **Meaningful only within its own filing** — sequence 2 in one filing and sequence 2 in another refer to unrelated entities. This is the identifier the information table uses to attribute a position, and the reason a bridge is required | — | Yes | Public |
| `filer_role` | string | `OTHER_REPORTING_MANAGER` where this manager reports positions on the filer's behalf; `OTHER_INCLUDED_MANAGER` where the filer reports positions on theirs. The direction of the reporting relationship, and it determines who is understood to hold the position | — | No | Public |
| `cik` | string | Central Index Key of the declared manager, where given | — | Yes | Public |
| `name` | string | Name of the declared manager as stated. Free text | — | Yes | Public |
| `form_13f_file_number` | string | The declared manager's 13F file number | — | Yes | Public |
| `crd_number` | string | The declared manager's CRD number, where given. Available on filings from the January 2023 schema release onward | — | Yes | Public |
| `sec_file_number` | string | The declared manager's SEC file number, where given | — | Yes | Public |

**Keys and uniqueness**

Unique on (`accession_number`, `manager_sequence_number`) for rows where `filer_role` is `OTHER_INCLUDED_MANAGER`. Other reporting managers are declared without a sequence number and are not addressable from the information table.

**Relationships**

| From | To | Cardinality | Implication for the result |
|---|---|---|---|
| `associated_filers` | `filing_master` | n : 1 | Every row belongs to exactly one filing |
| `associated_filers` | `associated_filers_bridge` | 1 : 0..n | A declared manager may be referenced by many holdings, or by none. A manager declared but never referenced is valid and is not an error |

**History behaviour**

Immutable. Follows `filing_master`.

**Breaking change classification**

Removal or retype of `manager_sequence_number` or `accession_number` is breaking. Removal of the composite uniqueness constraint is breaking.

---

### Asset: `holdings_detail`

**Grain**

One row is one security position line as reported in the information table of one filing.

**Columns**

| Column | Logical type | Meaning | Units | Nullable | Classification |
|---|---|---|---|---|---|
| `accession_number` | string | The filing this position was reported in | — | No | Public |
| `holding_sequence` | integer | The position of this row within the filing's information table as filed. Required because the information table has no natural key: a manager may legitimately report the same security more than once, split by discretion or by the manager responsible | — | No | Public |
| `name_of_issuer` | string | Issuer name as reported. Free text, filer-supplied, and not consistent across filers for the same issuer | — | No | Public |
| `title_of_class` | string | Class of security as reported — common stock, a note series, a warrant. Filer-supplied text | — | No | Public |
| `cusip` | string | Nine-character CUSIP as reported. The primary means of resolving a position to an instrument, and filer-supplied rather than validated by EDGAR | — | No | Public |
| `figi` | string | Financial Instrument Global Identifier, twelve characters. Optional in the schema from the January 2023 release; absent on all earlier filings and sparsely populated since | — | Yes | Public |
| `value` | integer | Market value of the position. **Reported in thousands of dollars on filings before the January 2023 schema change, and in whole dollars from that release onward.** The column is numerically continuous across the boundary and semantically discontinuous, which makes any unconverted time series wrong by three orders of magnitude at a single point | USD or USD thousands — see `value_unit` | No | Public |
| `value_unit` | string | Which unit `value` is expressed in for this row: `THOUSANDS` or `DOLLARS`. Derived, and recorded so that the unit is a property of the data rather than knowledge a consumer must hold | — | No | Public |
| `ssh_prnamt` | integer | Quantity held, in the unit given by `ssh_prnamt_type` | shares or principal | No | Public |
| `ssh_prnamt_type` | string | Whether `ssh_prnamt` is a share count (`SH`) or a principal amount (`PRN`). Summing quantity across both without separating them produces a meaningless total | — | No | Public |
| `put_call` | string | `PUT` or `CALL` where the position is an option; null where the position is the security itself. A null is not a missing value | — | Yes | Public |
| `investment_discretion` | string | Whether discretion is sole, defined by agreement, or held by another. Determines whether the reporting manager is understood to control the position | — | No | Public |
| `other_manager` | string | **Free text as filed.** Nominally the sequence number or numbers of the cover-page managers responsible for this position, comma-separated. In practice the field carries blanks, single integers, comma-separated lists, ranges, names, and text. It is not a validated field and cannot be relied on as structured | — | Yes | Public |
| `voting_authority_sole` | integer | Shares over which the manager has sole voting authority | shares | Yes | Public |
| `voting_authority_shared` | integer | Shares over which voting authority is shared | shares | Yes | Public |
| `voting_authority_none` | integer | Shares over which the manager has no voting authority | shares | Yes | Public |

**Keys and uniqueness**

Unique on (`accession_number`, `holding_sequence`). No uniqueness is asserted on (`accession_number`, `cusip`) — repeated CUSIPs within a filing are a legitimate reporting pattern, not a duplicate.

**Relationships**

| From | To | Cardinality | Implication for the result |
|---|---|---|---|
| `holdings_detail` | `filing_master` | n : 1 | Filing-level attributes joined to holdings repeat per row. Any filing-level aggregate computed after this join double-counts unless the holding grain is collapsed first |
| `holdings_detail` | `associated_filers_bridge` | 1 : 0..n | A position naming several managers produces several bridge rows. **Joining holdings to the bridge fans out.** Any position-value aggregate computed after that join overstates by the number of managers named |

**History behaviour**

Immutable. Follows `filing_master`.

**Breaking change classification**

Removal, rename or retype of any column is breaking. Removal of `value_unit`, or any change that makes `value` unit-ambiguous again, is breaking. Removal of the composite uniqueness constraint is breaking.

---

### Asset: `associated_filers_bridge`

**Grain**

One row is one resolvable attribution of one holding row to one cover-page manager.

**Columns**

| Column | Logical type | Meaning | Units | Nullable | Classification |
|---|---|---|---|---|---|
| `accession_number` | string | The filing both sides of the attribution belong to. The attribution is only meaningful within a filing | — | No | Public |
| `holding_sequence` | integer | The holding row being attributed | — | No | Public |
| `manager_sequence_number` | integer | The cover-page manager the holding is attributed to | — | No | Public |
| `resolution_status` | string | Whether the parsed sequence number matched a manager declared on the cover page of the same filing, or did not. An unmatched reference is a filer inconsistency, and recording it is what distinguishes a data defect from an absence | — | No | Public |

**Keys and uniqueness**

Unique on (`accession_number`, `holding_sequence`, `manager_sequence_number`).

**Relationships**

| From | To | Cardinality | Implication for the result |
|---|---|---|---|
| `associated_filers_bridge` | `holdings_detail` | n : 1 | Many attributions per holding |
| `associated_filers_bridge` | `associated_filers` | n : 1 | Many holdings per declared manager |

**History behaviour**

Immutable. Follows `filing_master`.

**Breaking change classification**

Removal or retype of any column is breaking. Removal of `resolution_status`, which would render unmatched references indistinguishable from absent ones, is breaking.

---

## 5 · Build Sequence

### Stage 1 — Envelope established

| Field | Content |
|---|---|
| Required end state | Every accepted 13F submission in the period range is present exactly once, identified by its accession number, with its cover page, signature block and summary page attributes recorded as filed |
| Reason | The accession number is the only identifier stable across every asset in this Solution. Nothing else can be attributed until the filing it belongs to exists |
| Expected effect | Adds one row per accepted submission. Removes nothing |
| Dependency | None |

### Stage 2 — Declared managers recorded

| Field | Content |
|---|---|
| Required end state | Every manager declared on a filing's cover page is recorded against that filing, with the sequence number the filer assigned and the direction of the reporting relationship |
| Reason | The information table attributes positions by sequence number alone. Without the cover-page declaration, a sequence number on a holding is an integer with no referent |
| Expected effect | Adds rows for filings declaring other managers. Filings declaring none contribute no rows, which is valid |
| Dependency | Stage 1 |

### Stage 3 — Positions loaded

| Field | Content |
|---|---|
| Required end state | Every row of every information table is present, in filed order, attributed to its filing |
| Reason | The information table is the disclosure. Filed order is retained because the table has no natural key and the ordinal is the only stable way to address a row |
| Expected effect | Adds one row per information table line. Notice filings contribute no rows, which is valid |
| Dependency | Stage 1 |

### Stage 4 — Value unit resolved

| Field | Content |
|---|---|
| Required end state | Every position row states the unit its value is expressed in, and that unit is correct for the schema version the filing was submitted under |
| Reason | The reported value changed from thousands of dollars to whole dollars at the January 2023 schema release. The column is numerically continuous across that boundary, so an unconverted series is wrong by three orders of magnitude at a single point without any visible discontinuity in the data itself |
| Expected effect | Derives a unit for every position row. Removes nothing |
| Dependency | Stage 3 |

### Stage 5 — Manager references parsed

| Field | Content |
|---|---|
| Required end state | Every integer manager reference expressed in a position's other-manager field is available as a separate value, associated with the position that carried it |
| Reason | The field is filer-supplied free text carrying what is nominally a list. Positions naming several managers cannot be attributed while the references remain in one field |
| Expected effect | Derives zero or more references per position. Non-integer content yields no reference and is not treated as an error, because the field is not validated at source |
| Dependency | Stage 3 |

### Stage 6 — Attributions resolved

| Field | Content |
|---|---|
| Required end state | Every parsed manager reference is recorded against the cover-page manager it identifies within the same filing, or recorded as unresolved where no such manager was declared |
| Reason | An attribution is only meaningful within its filing. Recording unresolved references rather than discarding them is what makes a filer inconsistency visible instead of silently reducing a manager's apparent holdings |
| Expected effect | Adds one row per parsed reference. Removes nothing; unresolved references are retained and marked |
| Dependency | Stages 2 and 5 |

**Filter chain summary**

| Stage | Removes / derives | Reason |
|---|---|---|
| 1 | Derives the filing envelope | Establishes the identifier everything else attributes to |
| 2 | Derives declared managers | Gives sequence numbers a referent |
| 3 | Derives positions | Loads the disclosure itself |
| 4 | Derives value unit | Makes a semantic discontinuity explicit |
| 5 | Derives manager references | Separates a list held in one text field |
| 6 | Derives attributions; marks unresolved | Makes filer inconsistency visible rather than silent |

Nothing is removed at any stage. Rows failing to parse are retained and marked, never dropped — a manager whose positions were partially discarded would present as holding less than they disclosed.

---

## 6 · Assertions

### 6a · Unit assertions

**Manager reference parsing** — `holdings_detail.other_manager` → parsed references. All blocking.

| Input | Expected output |
|---|---|
| `""` (empty) | No references |
| `" "` (whitespace only) | No references |
| `"3"` | One reference: 3 |
| `"2,5"` | Two references: 2, 5 |
| `"2, 5, 11"` | Three references: 2, 5, 11 |
| `"02"` | One reference: 2 |
| `"2,,5"` | Two references: 2, 5 |
| `"2, 2"` | One reference: 2. A position naming the same manager twice attributes once |
| `"2-5"` | No references. A range is not expanded, and no failure is raised |
| `"4, Barclays"` | One reference: 4 |
| `"Barclays"` | No references. No failure raised |
| `"see attached"` | No references. No failure raised |
| `"N/A"` | No references. No failure raised |
| `"0"` | One reference: 0. Sequence numbering starts at 1, so this will not resolve and is retained as unresolved rather than discarded at parse |
| `"-3"` | No references. A negative cannot be a sequence number |
| `"3.0"` | No references. A sequence number is an integer, and accepting a decimal would silently coerce |

<!-- Pairs marked from the declared schema and stated rules only. Discovery against real
     EDGAR content has not been run — see Open Items. Any shape found in production that
     no pair above covers is an unwritten rule, not an edge case to absorb at build. -->

**Value unit resolution** — filing → `value_unit`. All blocking.

| Input | Expected output |
|---|---|
| Filing under a pre-January-2023 schema version | `THOUSANDS` |
| Filing under a January-2023-or-later schema version | `DOLLARS` |
| Filing whose schema version cannot be determined | No unit assigned; the filing is marked rather than defaulted |

### 6b · Data assertions

| Assertion | Severity | Binding *(engineering)* |
|---|---|---|
| `filing_master` is unique on `accession_number` | Blocking | |
| `accession_number` is not null in all rows of every asset | Blocking | |
| `associated_filers` is unique on (`accession_number`, `manager_sequence_number`) where `filer_role` is `OTHER_INCLUDED_MANAGER` | Blocking | |
| Every `associated_filers.accession_number` exists in `filing_master` | Blocking | |
| Every filing declaring a non-zero `other_included_managers_count` has at least one `associated_filers` row | Blocking | |
| The count of `OTHER_INCLUDED_MANAGER` rows per filing equals that filing's `other_included_managers_count` | Non-blocking | |
| `holdings_detail` is unique on (`accession_number`, `holding_sequence`) | Blocking | |
| Every `holdings_detail.accession_number` exists in `filing_master` | Blocking | |
| Every filing whose `report_type` is a holdings or combination report has at least one `holdings_detail` row. Notice filings have none, and zero is correct for them | Blocking | |
| The count of holding rows per filing equals that filing's `table_entry_total` | Non-blocking | |
| Every holding row has a non-null `value_unit` | Blocking | |
| `value_unit` is one of `THOUSANDS`, `DOLLARS` | Blocking | |
| `cusip` is nine characters in all rows | Non-blocking | |
| `figi`, where present, is twelve characters | Non-blocking | |
| `ssh_prnamt_type` is one of `SH`, `PRN` | Blocking | |
| `put_call`, where present, is one of `PUT`, `CALL` | Blocking | |
| `associated_filers_bridge` is unique on (`accession_number`, `holding_sequence`, `manager_sequence_number`) | Blocking | |
| Every bridge row's (`accession_number`, `holding_sequence`) exists in `holdings_detail` | Blocking | |
| Every bridge row marked resolved has a matching (`accession_number`, `manager_sequence_number`) in `associated_filers` | Blocking | |
| No holding row is lost to bridge construction: every holding naming at least one integer reference has at least one bridge row | Blocking | |
| Holdings-to-bridge fan-out is bounded: no holding row produces more bridge rows than its filing declared included managers | Non-blocking | |
| **Stage conservation — stage 1:** every accession number in the EDGAR index for the period range is present in `filing_master`, and no others | Blocking | |
| **Stage conservation — stage 2:** the count of `associated_filers` rows equals the count of manager declarations in the source cover pages | Blocking | |
| **Stage conservation — stage 3:** the count of `holdings_detail` rows equals the count of information table lines in the source documents | Blocking | |
| **Stage conservation — stage 4:** row count is unchanged from stage 3, and `value_unit` is non-null on every row | Blocking | |
| **Stage conservation — stage 5:** every position whose other-manager field contains at least one integer yields at least one parsed reference; positions containing none yield none | Blocking | |
| **Stage conservation — stage 6:** the count of bridge rows equals the count of parsed references from stage 5. No reference is discarded; unresolved references are retained and marked | Blocking | |

### 6c · Runtime monitors

| Monitor name | Description | Measure and window | Threshold source | Calibration | Binding *(engineering)* |
|---|---|---|---|---|---|
| Filings volume — 7-day rolling | Detects a drop in filings loaded from EDGAR. A trigger means recent submissions may be absent; manager coverage for the period will be incomplete until resolved | Count of new `filing_master` rows, 7-day rolling total | Observed history | Statistical over baseline; backtest to zero triggers | |
| Holdings volume — 7-day rolling | Detects a drop in positions loaded. A trigger means positions may be missing for recent filings; totals by manager will understate | Count of new `holdings_detail` rows, 7-day rolling total | Observed history | Statistical over baseline; backtest to zero triggers | |
| Filings freshness | Detects that no new filing has loaded within the expected interval. A trigger means the data is stale and analysis will reflect an earlier point in time than it appears to | Max `filed_date` has advanced within n days, n from observed pattern | Observed history | Set inside the SLO of 1 business day; backtest to zero triggers | |
| Holdings growth per refresh | Detects an abnormal change in positions added between successive loads. Anchored on the freshness timestamp rather than the calendar, because filing arrival is heavily concentrated near the 45-day deadline | Change in `holdings_detail` row count between successive refresh events | Observed history | Statistical over baseline; backtest to zero triggers | |
| Source schema unchanged | Detects a change to either EDGAR document schema. A trigger means new or altered elements may be unparsed | Element set of `primary_doc.xml` and the information table | Observed history | — | |
| Filing identity holds | Detects duplicate accession numbers. A trigger means a filing has been loaded twice and every manager-level total including it is overstated | `filing_master` unique on `accession_number` | Specification (§6b) | — | |
| Position identity holds | Detects duplicate position rows within a filing | `holdings_detail` unique on (`accession_number`, `holding_sequence`) | Specification (§6b) | — | |
| Attribution identity holds | Detects duplicate attributions of a position to a manager | Bridge unique on its declared key | Specification (§6b) | — | |
| Value unit present | Detects positions loaded without a stated value unit. A trigger means any value series spanning those rows is unit-ambiguous and may be wrong by three orders of magnitude | Every `holdings_detail` row has non-null `value_unit` | Specification (§4, stage 4) | — | |
| Filing reference resolves | Detects orphaned rows in any asset. A trigger means positions or manager declarations exist with no filing to attribute them to | Every asset's `accession_number` resolves to `filing_master` | Specification (§6b) | — | |
| Filings without positions | Detects holdings or combination filings that loaded with no positions. A trigger means a manager appears to report nothing when they reported holdings — the loss direction, which the orphan check above cannot see | Count of holdings and combination filings with zero `holdings_detail` rows | Specification (§6b) | — | |
| Positions per filing within range | Detects filings loading with abnormally few positions. Catches partial load of an information table, which resolves referentially and is therefore invisible to the orphan check | Median positions per filing, 7-day rolling | Observed history | Statistical over baseline; backtest to zero triggers | |
| Unresolved attribution rate | Detects a rise in manager references that do not match a declared manager. A trigger means filer inconsistency has increased and manager-level attribution is less complete than usual | Unresolved share of `associated_filers_bridge` rows per quarter, threshold 2.0% *(invented)* | Specification | — | |

**Baseline window**

Eight quarters, excluding the quarter in which the January 2023 schema change took effect. That quarter carries a step change in reported value units and a schema element addition, and including it would widen every volume and growth threshold enough to mask a genuine drop. The remaining seven quarters are assumed incident-free; this assumption has not been independently verified and is recorded here so that a later reviewer can challenge it.

**Detection cost of widened thresholds**

Volume and growth thresholds are set on a 7-day rolling window rather than daily. Filing arrival is heavily concentrated in the final week before the 45-day deadline, and daily counts outside that window are too sparse to threshold without constant triggering. The cost is that a one-day total loss of loading is not detected on the day it occurs; it becomes visible as the rolling total falls over the following days. Same-day detection of a complete load failure is covered by the freshness monitor, not by volume.

---

---

## 7 · Governance and Lifecycle

**Classification and sensitivity**

Public. Every asset in this Solution derives from a public regulatory disclosure. No column carries a restricted classification.

**Compatibility promise**

Backward, transitive. Consumers may rely on no column being removed, renamed or retyped, and on no declared uniqueness constraint being removed or loosened, without a version. Additive change is made without a version.

**Versioning and deprecation** *(invented)*

A breaking change introduces a new version alongside the existing one. Two versions run concurrently for no less than 90 days, after which the older is retired. Consumers are owed 90 days notice of a breaking change and 30 days notice of retirement. Additive change ships without a version and without notice.

**Limitations and approved use**

| Approved for | Not approved for | Known gaps |
|---|---|---|
| Ownership and position-change analysis at the reported grain | Any use requiring complete institutional ownership | Positions under confidential treatment are omitted from the public table by permission and are not present here |
| Manager-level attribution within a filing | Cross-filing manager identity resolution | `filing_manager_name` is free text and is not consistent across periods for one entity; no entity-resolution layer is in scope |
| Value analysis where `value_unit` is respected | Value analysis ignoring `value_unit` | A series spanning the January 2023 boundary is wrong by three orders of magnitude if the unit is not applied |
| Positions as reported | Positions as held | 13F reports discretion, not beneficial ownership. Short positions are not reported at all |
| Point-in-time analysis at period of report | Analysis assuming amendments supersede | Amendment supersession is recorded, not applied. Consumers must select |

**Change notification** *(invented)*

Posted to `#dp-institutional-holdings` and included in the quarterly consumer note. Breaking changes are additionally raised directly with each named consumer in section 1.

**Retirement outline** *(invented)*

Two quarters notice to all named consumers and to the owner of the Ownership Analytics downstream process. Catalog trust status moves to deprecated at notice, not at removal. Assets remain readable for one quarter after the announced retirement date.

**Catalog trust status**

Certified on delivery.

---

## 8 · Catalog Metadata

| Field | Value |
|---|---|
| Asset descriptions | *see section 4* |
| Column descriptions | *see section 4* |
| Classification tags | Public; Regulatory disclosure; US equities |
| Ownership | Data Product & Operations *(invented)* |
| Certification target | Certified |
| Domain | Capital Markets Reference *(invented)* |

---

## 9 · Open Items — Routed to Engineering

| Item | Raised | Owner | Status |
|---|---|---|---|
| Handling mechanism for records failing a blocking assertion — quarantine, halt, or warn and continue | 2026-08-18 | Engineering | Open |
| Whether the value-unit boundary is determined from the schema version declared on the submission or from the filing date, where the two disagree | 2026-08-18 | Engineering | Open |
| Discovery survey of `other_manager` content across the full filing history has not been run. Any value shape it surfaces that §6a does not cover is an unwritten parse rule and must be decided before build | 2026-08-18 | Author | Open |
| Retention treatment for filings quarantined on schema validation failure | 2026-08-18 | Engineering | Open |

---

## 10 · Readiness

| Gate | Yes / No |
|---|---|
| No clarifying question is required to begin building | Partial — consumers are recorded but **invented, not confirmed** (criterion 3). Two open items in §9 remain unanswered |
| Someone who did not write this could maintain the product from it | Yes |
| This agrees with its neighbours on the platform | Unassessed — depends on the CUSIP instrument reference and CIK entity reference Solutions |

---

## Provenance

Authored by J. Okafor. Written against Definition of Ready v0.3. **Sections marked *(invented)* are placeholder content authored without a source and must be confirmed before this specification is treated as Ready.** Structure and element names verified against the EDGAR Form 13F submission taxonomy (`eis_13F_Filer.xsd`, `eis_13FDocument.xsd`) and a live `primary_doc.xml` on EDGAR. Committed `<date>`.
