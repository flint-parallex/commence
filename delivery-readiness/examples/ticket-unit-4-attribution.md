# Positions are attributable to the manager responsible for them

| Field | Value |
|---|---|
| Type | User story |
| Epic | DP-1180 — 13F Institutional Holdings *(invented)* |
| OKR | IP-OKR-142 *(invented)* |
| Target gate | UAT |
| Work unit | 4 of 4 — see `proposed-work-set.md` |

**Links:** `is blocked by` unit 2 (filing envelope) · `is blocked by` unit 3 (position disclosure) · `relates to` §9 open item — handling mechanism for blocking assertion failures

---

## Overview

**Story statement**

As an Equity Research Analytics consumer, I need each disclosed position tied to the manager responsible for it so that concentration and crowding measures reflect the manager who actually holds the exposure rather than the entity that filed on their behalf.

**Description**

A single 13F filing may report positions on behalf of several managers, and the filing identifies which manager holds each position using a sequence number that is meaningful only within that filing. This story delivers the attribution: every position that names a manager is linked to that manager's declaration on the filing's cover page, and every position that names one which cannot be matched is retained and marked rather than dropped. On completion, a consumer can attribute positions to managers within a filing and can measure how much of the disclosure could not be attributed.

---

## Scope of Work

### Out of scope

Derived from the outcome stated in the epic against the means this story is given.

**Cross-filing manager identity resolution.** The epic states that positions be attributable to the manager responsible. Within a filing, the means to do this are provided — cover page declarations carry a sequence number and, where the filer supplies one, a CIK. Across filings, they are not: `filing_manager_name` and `associated_filers.name` are free text and are not consistent across periods for the same entity, and no entity-resolution reference is in scope for this Solution. Attributing positions to the same manager across two filings is therefore not delivered here. Responsibility sits with the CIK entity reference Solution (§3).

**Attribution of positions naming an other reporting manager.** Managers declared as `OTHER_REPORTING_MANAGER` are declared without a sequence number and are not addressable from the information table (§4). Positions filed under this arrangement cannot be attributed by the means available, and the residual is not recoverable within this story.

**Attribution of positions whose other-manager field carries no integer.** The field is filer-supplied free text and is not validated at source. Content that carries no integer reference yields no attribution by design (§6a), and this is a limit of the source rather than of the implementation.

**Standard exclusion.** Observed-history monitor thresholds requiring production data history are set post-deploy, not at build.

### In scope

Attribution of positions to cover-page managers within a filing, including retention and marking of references that do not resolve. Completion is verified by the acceptance criteria below.

### Known constraints

Source data provides manager references only as free text in a single field. No structured alternative exists in the filing.

The `associated_filers_bridge` grain produces intentional fan-out from `holdings_detail` (§4). Consumers aggregating position value after joining to the bridge will overstate; this is a documented property of the model, not a defect to prevent here.

---

## Data model

Per specification §4, assets `associated_filers_bridge`, `holdings_detail`, `associated_filers`. Build sequence stages 5 and 6. Do not implement from this ticket; the specification is authoritative.

---

## Acceptance criteria

### Data model / schema conformance

1. `associated_filers_bridge` conforms to the model specified in §4, including column names, types and nullability.
2. `resolution_status` is present and non-null in all rows.

### Data quality

3. `associated_filers_bridge` contains no duplicate rows for (`accession_number`, `holding_sequence`, `manager_sequence_number`).
4. `accession_number` is not null in all rows.
5. `manager_sequence_number` is an integer in all rows.
6. Every bridge row's (`accession_number`, `holding_sequence`) exists in `holdings_detail`.
7. Every bridge row marked resolved has a matching (`accession_number`, `manager_sequence_number`) in `associated_filers`.
8. No bridge row resolves against a manager declared on a different filing.

### Complete data capture

9. Every position whose other-manager field contains at least one integer has at least one bridge row.
10. Positions whose other-manager field contains no integer have no bridge row, and this produces no error.
11. The count of bridge rows equals the count of integer references parsed from `holdings_detail.other_manager`. No reference is discarded.
12. References that do not match a declared manager are present with `resolution_status` recording non-resolution.
13. No holding row produces more bridge rows than its filing declared included managers.

### Parse behaviour

Per §6a. Each pair is a criterion; the expected output must hold for the stated input.

| Input to `other_manager` | Expected bridge rows |
|---|---|
| Empty or whitespace only | None |
| `"3"` | One, for manager 3 |
| `"2, 5, 11"` | Three, for managers 2, 5 and 11 |
| `"02"` | One, for manager 2 |
| `"2,,5"` | Two, for managers 2 and 5 |
| `"2, 2"` | One, for manager 2 |
| `"2-5"` | None. No failure raised |
| `"4, Barclays"` | One, for manager 4 |
| `"Barclays"` / `"see attached"` / `"N/A"` | None. No failure raised |
| `"0"` | One, retained with `resolution_status` recording non-resolution |
| `"-3"` / `"3.0"` | None. No failure raised |

### History

19. Bridge rows are immutable once written. A correction to a filing arrives as a new filing with its own accession number and produces its own bridge rows; existing rows are not updated.

### Pipeline observability

20. Run status, input row count, output row count and job duration are observable for the attribution build.
21. The count of unresolved references is observable per run.

### Monitoring

22. The runtime monitors specified for this unit in §6c are deployed: attribution identity holds, and unresolved attribution rate. The criterion is that each monitor is deployed and its binding is recorded in §6c — not that it has fired.

### Error handling and alerting

23. Given a filing whose cover page declares no managers and whose positions carry integer references, when attribution runs, then those references are written with `resolution_status` recording non-resolution and the run completes.

---

## Notes for grooming

This unit carries uncertain correctness. The parse rules are decided and specified (§6a), so the approach is not in question — but the proportion of references that fail to resolve cannot be determined before the build runs against real filings. The 2.0% unresolved threshold in §6c is invented placeholder content and must be replaced with an observed and agreed rate before this unit is accepted.

Estimation on this unit is less reliable than on units 2 and 3. Walk the parse pairs before sizing.

A discovery survey of `other_manager` content across the full filing history has not been run (§9). Any value shape it surfaces that the pairs above do not cover is an unwritten rule and returns to the specification — it is not an edge case for the developer to resolve at build.

---

*Fields marked (invented) are placeholder content. This ticket is a test of `delivery-readiness`, not an approved work item. The specification it derives from does not currently pass DoR gate 1.*
