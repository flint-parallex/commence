# Assertion Authoring

Section 6. Three classes, declared separately.

They run at different times, against different inputs, and are owned differently. They are never one combined table — a merged list loses the distinction between what blocks a merge and what observes production, which is the distinction that determines when a failure is actionable.

## The three classes

| Class | Runs | Against | Can block a merge |
|---|---|---|---|
| **6a · Unit** | In CI, before anything is materialised | Static inputs | Yes |
| **6b · Data** | After the asset builds | Real data | Yes |
| **6c · Runtime monitor** | In production, continuously | Live data | **No** |

Placing an assertion in the wrong class is not a formatting error. A correctness rule filed as a monitor cannot gate anything; a production observation filed as a data assertion will block builds for conditions that are not defects.

## Every hazard becomes an assertion

Criterion 19. This is the conversion that matters most.

A hazard written as prose is something a reader can miss. Written as an assertion, it is a gate.

| Written as a note | Written as an assertion |
|---|---|
| Fan-out is expected when joining holdings to the bridge | No holding row produces more bridge rows than its filing declared managers |
| The value column changed units in 2023 | Every row has a non-null value unit, and that unit is one of THOUSANDS, DOLLARS |
| Some manager references will not resolve | Every bridge row marked resolved has a matching manager declared on the same filing |
| CUSIPs are filer-supplied and may be malformed | CUSIP is nine characters in all rows |

**Sweep the specification for prose hazards before finishing.** Any sentence in sections 3, 4 or 5 warning a reader about something is an assertion that has not been written yet. If it survives only as prose, it is not a gate.

## Which class

Ask what the assertion tests and when it can be evaluated.

**6a — logic, testable without data.** Parse rules, derivation rules, classification rules. Each case of the rule is a separate assertion, including the cases that produce nothing. An input that should yield no output is a test, not an omission.

**Write 6a as input/output pairs.** A parse or derivation rule stated in prose has to be interpreted before it can be checked; stated as a pair it can be lifted into a test without anyone deciding anything. This is criterion 31's worked example, and for logic rules the worked example is the whole assertion.

| Input | Expected output | Severity |
|---|---|---|
| `""` (empty) | No references | Blocking |
| `"3"` | One reference: 3 | Blocking |
| `"2, 5, 11"` | Three references: 2, 5, 11 | Blocking |
| `"2-5"` | No references. No failure raised | Blocking |
| `"4, Barclays"` | One reference: 4 | Blocking |
| `"see attached"` | No references. No failure raised | Blocking |

The pairs are the specification. Engineering writes the test; the binding records where it lives.

**Cover the negatives, the boundaries, and the ugly.** A pair set containing only well-formed inputs specifies a rule that has never met real data. Include: empty, the single case, the multiple case, the malformed case that must not fail, the mixed case, and any shape discovery surfaced (see `verification.md`). Where a shape was found in the data and no pair covers it, the rule is not written yet.

**6b — the built asset.** Uniqueness, keys, referential integrity in both directions, nullability, accepted values, reconciliation to a declared total, bounded fan-out, per-stage row conservation. Referential and conservation assertions are owned by `referential-integrity.md`. If it can only be evaluated once real data exists but must be true every time, it belongs here.

**6c — production over time.** Volume, freshness, growth, schema stability, and the continued holding of a 6b rule in production. Where a correctness rule appears in both 6b and 6c, **both instances trace to the specification** (criterion 22). Measure, evaluation window, calibration and alert fields are owned by `runtime-monitors.md`.

## Severity

Blocking or non-blocking, per assertion, on 6a and 6b (criterion 21).

**Blocking** — the product is wrong if this fails. Keys, uniqueness, referential integrity, anything a consumer would compute an incorrect result from.

**Non-blocking** — the product is usable but a defect is present. Reconciliation to a filer-supplied total, format checks on a field that is not validated at source, bounded fan-out where the bound is a sanity check.

The test: **would a consumer using this data get a wrong answer, or an incomplete one?** Wrong is blocking.

**The mechanism that implements a failure is engineering's** and is routed under criterion 6. Declare the severity; never write quarantine, halt, or warn-and-continue into the specification.

## Threshold source

Criterion 22. Required for every runtime monitor, and the field that determines whether the monitor can be automated.

| Source | Meaning | Implementation |
|---|---|---|
| **Specification** | The threshold is declared in this document | Manual, always |
| **Observed history** | The threshold derives from what the data has done | Out-of-the-box tooling |

A monitor whose threshold traces to the specification must be implemented manually **even where tooling would watch the same column**. The column is not what distinguishes them; the oracle is. "The tool already covers that" is the natural and wrong conclusion.

Observed history is never used as a test oracle. A monitor built from what the pipeline currently emits will pass on the day the product silently breaks.

## Assertion IDs

Every assertion carries an ID, assigned by the author and never changed.

**Format:** `<SOLUTION>-<CLASS>-<NNN>` — class is `U` for unit, `D` for data, `M` for monitor.

**Append-only.** A retired assertion keeps its ID and is struck through. IDs are never reused and never renumbered, because a renumbered ID silently re-points every historical check result at a different requirement.

The ID is what makes a check result traceable back to the assertion that required it. Without it, a rollup of check results can report that something failed but not what the failure means, and the severity has to be re-derived from the tool rather than read from the specification.

## Binding

The binding column stays blank in this document.

We write the assertion and its ID. Engineering writes the check's own identifier — the dbt test name, the expectation name, the monitor name — at acceptance. The pair is what makes the trace work in both directions: the specification can be asked which checks implement it, and a check result can be asked which assertion it satisfies.

**The binding is an identifier, not a description.** "Covered by the holdings dbt tests" cannot be joined to anything. `dq_holdings_unique_accession_sequence` can. A binding that does not name something a system can resolve is not a binding.

The binding is verified to exist at acceptance (criterion 23) — that verification is the only substitute available for intent-drift detection, because a specification bound to a check fails visibly when it stops being true.

**Never populate a binding.** A binding written by the author records where a check was expected to live, not where it lives, and it will read identically to a real one.

**An unbound assertion is unknown, not passing.** Anywhere assertion state is aggregated, a missing binding must surface as unknown. Treating it as healthy means the assertions nobody implemented are the ones that never raise a problem.

## What good looks like

- Every assertion names the field and the rule. No unverifiable verbs.
- Expected values come from the specification, never from production data and never from what the pipeline currently outputs.
- 6a covers the negative cases, not only the positive ones.
- Every prose hazard elsewhere in the document has a corresponding assertion here.
- Each assertion sits in exactly one class, and each class is populated or explicitly empty.
