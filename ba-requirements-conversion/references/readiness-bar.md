# The Readiness Bar

Six preconditions for specifying a data product. Generic software Definition of Ready misses most of these — data work fails for reasons that story-point checklists do not catch.

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

**(c) The business has stated what correct looks like.**

Not "the data should be accurate" but something with a shape: what must reconcile and against what, what a null signifies where it appears, what totals are expected to agree.

The precondition is that the business named the condition — not that it has been written as a check. Converting it into an assertion is specification work. What fails here is silence: a requirement where nobody has said what would make the data wrong cannot be specified, because there is nothing to assert.

**(d) Grain, semantics, and historization are unambiguous.**

Grain: one row per *what*. Semantics: what each field means, including edge cases. Historization: how change over time is tracked — overwrite, versioned rows, effective dating.

This is the precondition most often silently assumed and most expensive to get wrong. A grain misunderstanding is not a bug fix; it is a rebuild.

**(e) Upstream platform dependencies are named and sequenced.**

What has to exist before this can be built, and in what order. Ingestion, transformation layers, access provisioning, other datasets.

**(f) Technical feasibility on the current platform.**

Buildable with what exists today — not with a platform capability that is planned, requested, or assumed.

## What this bar is not

**It is not the Definition of Ready.**

This bar asks whether the BA supplied enough to write a specification from. The Definition of Ready asks whether a finished specification is complete enough to build from. Different gates at different moments, and a requirement can clear this one comfortably and still be a long way from ready to build.

Do not assess a requirement against the Definition of Ready here. Most of its criteria concern content that does not exist yet — declared keys, build sequence, assertion bindings — and failing a requirement for lacking them is failing the BA for not having done the specification.

## Recording the assessment

For each precondition: pass or fail, with a specific reason.

Every failure becomes a numbered open question, owned by the BA, dated. A failure recorded without a corresponding question is a dead end — it tells a reader something is wrong without giving anyone a way to resolve it.
