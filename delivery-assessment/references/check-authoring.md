# Check Authoring

Where a data quality check should live, what the framework cannot do, and how checks connect to acceptance criteria.

**Internal.** The repo-facing `unit_tests/checks_reference.md` lists what is available and how to use it. This file holds the judgment layer — placement decisions and the connection to acceptance criteria. It is not published to the engineering repository.

**What the library cannot do** is recorded in `capabilities/pipeline-library/reference.md`, not here. Read it before deciding placement: a gap there is the reason a check lands in `shared/` rather than being a library call.

That content stays internal for the same reason this file does. A document in the engineering repo stating what the capability team's library lacks reads as criticism of their product; the same fact routed as capability feedback, from the PO seat, is ordinary.

---

## Where a check should live

Decide in this order.

**1 · `pipeline_library`** — where a library check already covers it. Never reimplement something the library provides.

**2 · `shared/generic.py`** — where two or more datasets could need it. Parameterize on table, column, and threshold. Most checks belong here.

**3 · `shared/sec_connector.py`** — where it asserts a property of the SEC connector rather than of a dataset. Applies to every SEC-derived product unchanged.

**4 · `datasets/`** — only where the rule exists for exactly one product and cannot be expressed by parameterizing anything.

**The test:** if two datasets could ever need it, it is shared. A file in `datasets/` should contain configuration and calls — table names, form types, column names, thresholds. **If logic appears in a dataset file, it belongs in `shared/`.** Very few checks are genuinely bespoke, and that is the sign the layering is right.

**Then upstream.** A check proven across multiple datasets is a candidate to contribute to `pipeline_library`, after which it becomes an import rather than something maintained locally. Prove it on one dataset, validate it against the others, then propose. Offering a shared capability before it has caught a real defect is a proposal; offering it after is a contribution.

---

## Applying this to acceptance criteria

Every acceptance criterion asserting something about delivered data should map to an available check.

**Where a criterion cites an assertion ID**, the assertion is already declared in specification §6 and the criterion is a reference, not a restatement. The mapping question is whether a check exists that implements it.

**When writing a criterion**, check it against `unit_tests/checks_reference.md`. If nothing maps, one of two things is true:

- The criterion needs rewriting to be checkable — see `acceptance-criteria.md`
- A new shared check is needed, and it belongs in `shared/`

**A criterion that cannot be expressed as a check is a defect in the criterion**, not a reason to write it vaguely.

**In ticket wording, state what must be true — never which function to call.**

> Every filing marked as processed exists in the L1 table.

Not: *"Call `processed_missing_from_l1`."*

The reference tells engineering what exists. The ticket states the requirement. Naming the function prescribes implementation, breaks when the library changes, and puts the PO on the wrong side of the what/how line.

---

## Tests versus monitors

Owned by `../../specification-authoring/references/assertion-authoring.md`, which defines three classes rather than two — unit, data, and runtime monitor — with severity and threshold source per class.

The rule that matters here: **never derive one from the other.** A test threshold taken from observed history is a monitor wearing the wrong label, and it will pass on data that is already wrong.
