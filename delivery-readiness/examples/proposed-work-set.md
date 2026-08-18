# PROPOSED WORK SET — 13F Institutional Holdings

Derived from `specification.md` v0.1 per `decomposition.md` Mode 1.

---

## DERIVATION

**Dependency order**

From build sequence stages 1–6 and the declared references (section 3).

```
SEC state/country codes ─┐
CUSIP instrument ref ────┼─ (enablers, land once, before the thread)
CIK entity ref ──────────┘

Stage 1 filing_master
   ├── Stage 2 associated_filers
   └── Stage 3 holdings_detail
            └── Stage 4 value unit
            └── Stage 5 manager references parsed
                     │
       Stage 2 ──────┴── Stage 6 attributions resolved
```

Stage 6 is the only stage with two upstream dependencies. That convergence is the shape of the deliverable.

**Grain boundaries**

Four assets, three assertable boundaries.

| Boundary | Assets that must exist together | Closing assertion |
|---|---|---|
| Filing envelope | `filing_master` + `associated_filers` | `associated_filers` unique on (`accession_number`, `manager_sequence_number`); included-manager count reconciles to the summary page |
| Position disclosure | `holdings_detail` (with `value_unit`) | `holdings_detail` unique on (`accession_number`, `holding_sequence`); row count reconciles to `table_entry_total` |
| Attribution | `associated_filers_bridge` | Bridge unique on its declared key; every resolved row matches a declared manager; no holding naming an integer reference lacks a bridge row |

`associated_filers` is not a separate cut. Its uniqueness is assertable alone, but its reconciliation assertion — declared count against the summary page — reads `other_included_managers_count` from `filing_master`. The two close together or neither closes.

`value_unit` (stage 4) is not a separate cut. It derives no new grain and its assertion is evaluated on `holdings_detail` rows.

Stage 5 is not a separate cut. Parsing produces no materialised asset; its assertions are unit assertions against static inputs, which run in CI on the unit that carries the parse.

**Reference data**

SEC state and country codes, CUSIP instrument reference, CIK entity reference. All three are canonical references governed elsewhere (section 3), land before the thread starts, and are referenced rather than redefined. **Not cut points.** One enabler where the reference does not yet exist; otherwise a dependency on the epic.

---

## WORK SET

| # | Type | Title | Closing assertion | Depends on | Risk |
|---|---|---|---|---|---|
| 1 | Enabler | SEC state and country code reference available | Reference resolves for every code appearing in cover-page addresses | — | Low |
| 2 | Story | Institutional filings and their declared managers are queryable for a reporting period | `associated_filers` unique on (`accession_number`, `manager_sequence_number`); included-manager count reconciles to summary page | 1 | Low |
| 3 | Story | Reported positions are queryable with their value unit stated | `holdings_detail` unique on (`accession_number`, `holding_sequence`); row count reconciles to `table_entry_total`; `value_unit` non-null on all rows | 2 | **Medium** — value-unit boundary rule is an open item (§9) |
| 4 | Story | Positions are attributable to the manager responsible for them | Bridge unique on declared key; every resolved row matches a declared manager; no holding naming an integer reference lacks a bridge row | 2, 3 | **High** — see below |

Four units. Not eight.

**Unit 4 carries uncertain correctness.** The other-manager field is filer-supplied free text that is not validated at source. Blanks, single integers, comma-separated lists, ranges, names and prose all occur in it. The specification declares which content yields a reference (§6a) and requires unresolved references to be retained and marked rather than discarded (stage 6). What cannot be determined in advance is the proportion of references that will fail to resolve, and therefore what unresolved rate is acceptable. This is not a spike — the approach is decided. It is a flag that estimation on this unit is less reliable than on units 2 and 3, and that grooming should walk the parse rules before sizing it.

**Unit 3 carries a dependency on an open item.** Whether the value-unit boundary is determined from the declared schema version or the filing date is routed to engineering (§9). The unit is buildable — the assertion states the required end state either way — but the two sources disagree for filings submitted near the January 2023 release, and the unit cannot be accepted until the question is closed.

---

## MONITORING

**Covered by tooling — no ticket**

Threshold source is observed history. Out-of-the-box freshness, volume and schema-change detection covers these.

| Monitor | Unit |
|---|---|
| `filing_master` receives new rows during filing season | 2 |
| Holding row volume per quarter within observed range for the equivalent quarter | 3 |
| Source schema unchanged for either document | 2, 3 |

**Manual, specification-sourced — carried on the unit**

Threshold traces to the specification, so these are implemented manually even where tooling would watch the same column. History is not a test oracle (DoR criterion 22).

| Monitor | Unit | Threshold source |
|---|---|---|
| `filing_master` remains unique on `accession_number` | 2 | §6b |
| Every asset's `accession_number` resolves to `filing_master` | 2 | §6b |
| `holdings_detail` remains unique on (`accession_number`, `holding_sequence`) | 3 | §6b |
| Every holding row retains a non-null `value_unit` | 3 | §4, stage 4 |
| Bridge remains unique on its declared key | 4 | §6b |
| Unresolved attribution rate within the rate declared acceptable at acceptance | 4 | §6c |

Unit 4's monitors are not their own increment — three monitors on one unit does not warrant a separate ticket. All carried on their build units, verified post-deploy: the criterion is that the monitor is deployed and its binding recorded, not that it has fired.

**Data assertions are acceptance criteria, not tickets.** All nineteen in §6b attach to the unit whose grain they close.

---

## NOT PROPOSED

- **A ticket per table.** Four assets, four units would coincide only by accident. `associated_filers` and `filing_master` close together; `value_unit` and the parse close no grain of their own.
- **A ticket for the CUSIP instrument reference or CIK entity reference.** Both are governed elsewhere (§3). Recorded as dependencies on the epic, not as work in this set.
- **Spikes.** No unknown has surfaced in grooming. Unit 4's uncertainty is flagged, not converted into a question.
- **Amendment supersession logic.** §7 states amendments are recorded, not applied, and consumers select. Nothing to build.

---

## DOES NOT DECOMPOSE

Nothing. All four units close on a declared assertion.

Noted for the record: `holdings_detail` → `associated_filers_bridge` is a declared fan-out (§4). It is assertable — unit 4's closing assertion bounds it, and §6b carries a non-blocking assertion that no holding produces more bridge rows than its filing declared managers. Had the fan-out been declared without a bound, unit 4 would not have cut and this section would name it.

---

## MISSING INPUTS

Required before ticket bodies are written.

| Input | Blocks | DoR criterion |
|---|---|---|
| Named consumers and their stated use | All units — a story cannot state who receives the outcome | 3 |
| Named accountable owner and support channel | All units | 2 |
| OKR reference (IP-OKR-###) | All units | — |
| Epic this rolls up to | All units | — |
| Acceptable unresolved attribution rate | Unit 4 — the monitor threshold cannot be written without it | 22 |
| Value-unit boundary determination rule | Unit 3 acceptance | §9 open item |
| Whether the SEC state/country reference already exists | Unit 1 — determines whether it is an enabler or a dependency | 14 |
| Cost envelope, versioning policy, change notification channel, retirement outline | Epic-level, not unit-level | 10, 11, 12, 28 |

**Readiness (§10): the specification does not currently pass gate 1.** Consumers are unnamed. Units may be proposed and reviewed; ticket bodies should not be written until that is closed.
