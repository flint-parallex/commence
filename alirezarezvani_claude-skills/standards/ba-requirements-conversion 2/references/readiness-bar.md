# The Readiness Bar

Six preconditions for data delivery work. Generic software Definition of Ready misses most of these — data work fails for reasons that story-point checklists do not catch.

## How to use this

**This is a refinement guideline, not a stage-gate.**

A hard gate reintroduces waterfall: work queues up waiting for a checklist to clear, and the collaborative refinement that actually produces readiness stops happening. The bar's job is to structure a conversation, not to block one.

Practically: a requirement failing preconditions is not rejected. It is a requirement with named, owned, dated open questions attached, and delivery planning proceeds with those visible.

Framing matters here too. The same checklist used as a discipline reads as craft; used as a weapon it reads as obstruction, and it stops working the moment a counterpart perceives it that way.

## The six preconditions

**(a) A stable, versioned upstream data contract exists.**

The schema, semantics, and change policy of the source are agreed and versioned, so downstream work does not break silently when upstream changes.

*Where no contract mechanism exists in the organization:* record this as a **dependency**, not a failure. Holding a counterpart to a precondition their organization cannot satisfy is unusable, produces a document where everything fails on day one, and reads as obstruction rather than diligence. Note the absence, name what it puts at risk, and move on.

**(b) Source data is available, accessible, and profiled for quality.**

Available: it exists. Accessible: catalog grants are in place and the team can actually read it. Profiled: someone has looked at what is in it — null rates, cardinality, obvious anomalies.

Accessibility is the one most often assumed. "The data exists" and "we can query it" are different claims.

**(c) Acceptance criteria are expressible as automated data checks.**

Not "the data should be accurate" but a check that can run: row counts within a range, no nulls in a key column, referential integrity against a named source, a reconciliation that balances.

If a criterion cannot be written as a check, it cannot be verified, and the story cannot be called done in any meaningful way.

**(d) Grain, semantics, and historization are unambiguous.**

Grain: one row per *what*. Semantics: what each field means, including edge cases. Historization: how change over time is tracked — overwrite, versioned rows, effective dating.

This is the precondition most often silently assumed and most expensive to get wrong. A grain misunderstanding is not a bug fix; it is a rebuild.

**(e) Upstream platform dependencies are named and sequenced.**

What has to exist before this can be built, and in what order. Ingestion, transformation layers, access provisioning, other datasets.

**(f) Technical feasibility on the current platform.**

Buildable with what exists today — not with a platform capability that is planned, requested, or assumed.

## The INVEST lens

Beyond the six, check each requirement for the three that fail most often on data work:

- **Testable** — see (c). If it cannot be checked, it cannot be accepted.
- **Estimable** — enough is known to size it. Unknown grain or source usually means unestimable.
- **Small** — fits a sprint. Data stories inflate quietly; "load the dataset" is rarely one story.

## Recording the assessment

For each precondition: pass or fail, with a specific reason.

Every failure becomes a numbered open question, owned by the BA, dated. A failure recorded without a corresponding question is a dead end — it tells a reader something is wrong without giving anyone a way to resolve it.
