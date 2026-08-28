# Consistency Sweep

Cross-section invariants. A specification can be complete field by field and still
contradict itself, because the template's sections were filled at different times and
nothing in the document forces them to agree.

Ordered by how often each catches something real.

---

## The rule that makes this safe

**The sweep reports. The operator decides.**

Where two sections disagree, the sweep never picks a winner. Which statement is right is a
decision about what the product is, and it belongs to the same seat that made it the first
time. A sweep that silently reconciled §1 to §4 would be making that decision invisibly —
the same failure as sourcing a grain from a table, one layer up.

Report the pair, name the sections, ask.

---

## When it runs

| Trigger | Scope |
|---|---|
| Status → **Ready**, and before any contract reconciliation | Full sweep. Mandatory |
| After an edit to §1 grain, §4 columns, §4 keys, or §4 relationships | Checks 1–5 only. These have the widest blast radius |
| On request | Full, or the named section |
| Status → **Delivered** | Full sweep |

Not after every edit. Mid-draft inconsistency is the normal condition of a document being
written — flagging it continuously trains the operator to skim past the flag, which costs
the check its value at the gate where it matters.

---

## 1. Grain stated twice, differently

§1 declares the entity and what one row means at Solution level. §4 declares a grain per
asset. These drift apart whenever an asset is added or the grain is refined in one place.

Check each §4 asset grain against §1. A Solution-level grain of *one row per account* and
an asset grain of *one row per account per effective date* is not a contradiction if the
asset is deliberately finer — but it must be **stated as** finer, not left to read as the
same sentence twice.

**Related:** the grain sentence and the declared key must agree. *One row per account per
date* with a key of account alone is one of the two statements being wrong, and which one
determines whether there is a fan-out or a typo.

## 2. Relationships without both assertions

`referential-integrity.md` requires every §4 relationship to carry **both** an orphan and a
loss assertion in §6. This is the check that most often finds a real omission, because
adding a relationship feels finished when the cardinality is written.

For each row in every §4 relationships table, confirm both assertions exist and reference
that relationship. Report the direction that is missing, not just that one is.

## 3. Stages without a conservation assertion

Every stage in §5 requires a conservation assertion in §6. Same failure mode as check 2 —
a stage feels complete when its required end state is written.

## 4. Assertions referencing columns that do not exist

An assertion naming a column absent from §4, or naming a column whose §4 logical type
cannot support the assertion, is unrunnable. Both usually mean a column was renamed in §4
after the assertion was written.

Check the reverse too: a column with a `Classification` marking it sensitive and no
assertion anywhere touching it is not necessarily wrong, but it is worth a question.

## 5. Assertion IDs

IDs are assigned once and never change. Confirm: no duplicates, no ID referenced anywhere
in the document that is not declared in §6, and no ID that has changed since the last
committed version of the specification. A changed ID silently breaks the trace to whatever
engineering bound to it.

## 6. Delivery window arithmetic

§2's processing window is the interval between upstream availability and required delivery
time. It is computed, not asserted. Recompute it and report a mismatch.

**Related:** the SLO must sit tighter than the published SLA on every measure. Equal values
mean there is no room to detect and correct before a promise breaks, which is the whole
reason the two columns exist. Report equality as a finding, not just inversion.

## 7. Breaking-change classification against the compatibility promise

§4 names which changes to an asset break consumers. §7 names what consumers may rely on us
not doing without a version. These are written in different sections, months apart, and
they routinely disagree.

A §7 promise of *backward — additive only* alongside a §4 classification that treats an
additive column as breaking is a contradiction that will surface as a version dispute at
the worst possible moment. Report the pair.

This check gates the contract's version-bump rule, which reads from both. Run it before any
reconciliation.

## 8. Classification stated in three places

Column-level in §4, Solution-level in §7, tags in §8. A column classified sensitive in §4
with no corresponding tag in §8, or a Solution-level classification less restrictive than
its most restrictive column, is a real exposure and not a documentation nit.

## 9. Consumers against approved use

A §1 consumer whose stated use appears in §7's *not approved for* column. This is rare and
serious — it means the Solution is documented as unfit for something a named consumer is
already recorded as doing with it.

## 10. Open items that have been answered

§9 items resolved in the body of the document but never closed in §9. Report the item, the
section that appears to answer it, and ask whether it is closed. Do not close it — an
answer written elsewhere is not the same as the owner accepting it.

## 11. Residue

Unfilled template tokens (`<...>`), `NOT SUPPLIED` markers, and leftover HTML guidance
comments. These are correct output while drafting and are findings at a gate.

`NOT SUPPLIED` in a contract-bearing field will make the projected contract fail
validation. Report it as such, so the failure is understood before it happens rather than
diagnosed afterwards.

---

## Output

A numbered list. Per finding: the sections that disagree, both statements as written, and
the question the operator needs to answer. No proposed resolutions.

Where the sweep is clean, say so in one line. A silent pass and a sweep that was never run
look identical, and at a gate that difference matters.
