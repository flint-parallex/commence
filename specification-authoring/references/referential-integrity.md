# Referential Integrity

Assertions that a relationship holds — between a child and its parent, between a bridge and both its sides, and across each stage of a pipeline.

These are the assertions most often written in one direction only, and the missing direction is the one that catches silent loss.

## The two directions

**Orphan** — a child row whose parent does not exist. Catches bad loads, partial deletes, and attribution to a filing that was never accepted.

**Loss** — a parent whose expected children have gone. Catches silent upstream truncation, a partially-loaded batch, and a stage that dropped rows it did not declare it would drop.

**A child asset that loses ninety per cent of its rows still passes the orphan test perfectly.** Every remaining row resolves. Orphan checks say nothing about completeness, and a specification that carries only orphan checks has no defence against the failure mode that actually occurs.

**A count comparison is not a loss check either.** A hundred records dropped and a hundred duplicated nets to zero variance. Count variance detects gross loss; only a key-level comparison proves the right records landed. Declare both, and have the key-level check return the missing keys with their source so a breach is diagnosable rather than merely visible.

Write both directions. State both.

## Child assets

For every asset with a declared parent (section 4, Relationships).

| Assertion | Direction | Class | Threshold source |
|---|---|---|---|
| Every child key resolves to a parent | Orphan | 6b blocking, 6c | Specification |
| Every parent that must have children has at least one | Loss | 6b blocking where the specification declares it must | Specification |
| Children per parent is within the declared cardinality | Loss / fan-out | 6b | Specification |
| Children per parent is within its normal range | Loss | 6c | Observed history |

The third and fourth are different assertions and belong to different classes. A declared bound — *no filing has more attributions than it declared managers* — is specification-sourced and cannot be calibrated. A range — *holdings per filing is roughly what it has been* — is observed history and is calibrated per `runtime-monitors.md`.

**Where zero children is valid, say so explicitly.** A notice filing with no positions is correct, and an assertion that every parent has children would fail every one of them. State the condition under which zero is valid rather than leaving the assertion silently wrong.

## Bridge and junction assets

Both sides resolve, and neither side is silently lost.

| Assertion | Class |
|---|---|
| Every bridge row resolves on its first key | 6b blocking |
| Every bridge row resolves on its second key, or is marked unresolved | 6b blocking |
| No source row that should produce a bridge row lacks one | 6b blocking |
| Fan-out is bounded by the declared cardinality | 6b |
| Unresolved share is within the rate declared acceptable | 6c, specification |

**Retain and mark unresolved references; never discard them.** A reference dropped at build time is indistinguishable from one that never existed, and the difference is exactly what a consumer needs to judge completeness. Marking is what turns a silent reduction into a measurable rate.

## Pipeline processing assets

Intermediate and staging assets are where loss is least visible, because no consumer reads them and nothing downstream fails loudly when they thin out.

**The filter chain summary is the referential contract.** Section 5 declares, per stage, what that stage removes or derives (criterion 17). Each of those declarations is an assertion waiting to be written:

| Declared in section 5 | Assertion |
|---|---|
| Stage removes nothing | Row count out equals row count in |
| Stage removes records failing a stated rule | Row count out equals row count in, less rows failing that rule; no other rows are absent |
| Stage derives a value per row | Row count out equals row count in, and the derived column is non-null on all rows |
| Stage derives zero or more per row | Every input row producing at least one output has output; inputs producing none are the declared cases only |

The general form: **no stage removes rows it did not declare it would remove.** Write it per stage, not once at the end — a single end-to-end reconciliation tells you something was lost but not where.

**Terminal reconciliation.** The final asset reconciles to source through the declared chain. Where it does not, the discrepancy is explicable by the summary or it is a defect — that is the whole purpose of criterion 17.

## Class placement

| Question | Class |
|---|---|
| Must this be true every time the asset builds? | 6b |
| Must this continue to be true in production? | 6c, specification-sourced |
| Is this a range that varies normally? | 6c, observed history |
| Is this the parse or derivation logic itself? | 6a, against static inputs |

A referential rule appearing in both 6b and 6c is normal and expected. Both trace to the specification (criterion 22); neither is calibrated.

## What good looks like

- Every declared relationship in section 4 has both an orphan and a loss assertion.
- Every stage in section 5 has a conservation assertion matching its declared expected effect.
- Where zero children is valid, the condition is stated rather than the assertion omitted.
- Declared bounds and observed ranges are separate assertions in separate classes.
- Unresolved references are marked, and their rate is monitored against a declared threshold.
