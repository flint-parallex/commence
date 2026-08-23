# pipeline_library

The data quality check library provided by the capability team. Consumed by every data product's unit tests.

**This is a capability reference, not a specification.** It records what the library provides and what it does not, so that a specification citing it can see the constraint.

**Internal.** Not published to the engineering repository. A document there stating what the capability team's library lacks reads as criticism of their product; the same fact routed as capability feedback, from the PO seat, is ordinary.

## Known gaps

**No referential or coverage check exists in `pipeline_library`.** Every check function is single-column and single-DataFrame. Cross-table reconciliation has no library equivalent.

**`compare_datasets` does not compare rows.** It compares schema and profile summary statistics. It cannot identify which records are missing. Confirmed against library source, 2026-08-04.

**Consequence:** every stage reconciliation is written locally as an anti-join on business keys. This is why `shared/sec_connector.py` exists rather than being a set of library calls.

**Open capability feedback:** the DQ framework has no referential or coverage check function. All five products in production require stage-to-stage reconciliation. Candidate for upstream contribution once proven locally across multiple datasets. Route as capability feedback; do not raise it as a defect.

---


## Consequence for specifications

A specification citing this capability in §3 should record, in the coverage column, whether what it relies on is actually provided. Where it is not, the check lands in `shared/` locally — see `delivery-assessment/references/check-authoring.md` for placement.

## Raising a gap

Route as capability feedback, never as a defect. A gap proven across multiple products is a candidate for upstream contribution: prove it on one dataset, validate against the others, then propose. Offering a shared capability before it has caught a real defect is a proposal; offering it after is a contribution.
