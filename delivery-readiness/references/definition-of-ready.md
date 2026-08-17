# Definition of Ready

**Data Solution, Data Asset and Ticket levels · v0.3**

A specification is ready when the work of interpretation is finished. Not when it is written, not when it is thorough, but when nothing material is left for anyone else to infer.

This is the standard we hold our own specifications to. It measures what we produce, not what we receive. A separate standard covers what a requirement must contain before we can specify against it; the two are deliberately distinct, and no criterion below depends on anyone else supplying anything. Every item here we can satisfy by our own work.

Each criterion carries a **Met when** line. A criterion is not satisfied by having thought about it, or by an adjacent sentence implying it. It is satisfied by a named answer a reader can locate.

---

## What this is not

This is not a gate. Enabling work, exploration and spikes proceed regardless of where a specification sits against this list, and nothing here is a reason to hold a team from starting.

The Data Solution level is nonetheless firm, for one reason: these specifications are read by automation as well as by people. Ambiguity that an engineer resolves with a question becomes, for an agent, a silent and confident wrong answer. A specification that a person could work around is not the same as one a pipeline can consume.

---

## Relationship to the specification template

The Data Solution Specification template is the instrument of this standard. Every field in it exists because a criterion here requires it, and every criterion here has a field to be written in. That correspondence is load-bearing: it is what allows completeness of the template to *be* readiness, with no separate review.

The correspondence therefore has to be maintained deliberately. When this standard gains a criterion, the template gains a field in the same change. A criterion with nowhere to be written is a criterion nobody satisfies.

Two standards are referenced, never restated: the bi-temporal and versioning standard governs system time, business time and temporal semantics; the audit-field standard governs tracking columns. Where a criterion below touches either, it points at them.

---

## The three gates

Everything below serves these. Where the detail and the gates disagree, the gates hold.

**1 · No clarifying question is required to begin.** Requirements are where defects originate — empirical work places between half and two thirds of all defects in the requirement phase, and the input engineers most often report missing is acceptance criteria specific enough to derive a pass or a fail. A specification that generates questions has moved its cost downstream, not removed it.

**2 · Someone who did not write this could maintain the product from it.** The rebuild test, extended across time rather than across people. If the answer is no, the intent is under-specified — we have recorded what the product does without recording why it is shaped that way.

**3 · This agrees with its neighbours.** Two Data Solutions describing the same business concept do not define it twice. Divergence here is not a defect in either product; it is a defect in the platform, and it originates at specification.

Thirty-three completed fields can still describe something nobody can build from. The gates are what the fields serve.

---

## Level 1 · Data Solution

What must be true of the whole delivered Solution.

### A · Intent and audience

**1. Purpose is stated.** One paragraph of intent, in business terms.
*Met when:* a reader outside the domain can say what decision or process the Solution serves.

**2. A named accountable owner and support channel are defined.** A person, not only a team alias.
*Met when:* a named individual and a reachable channel are both recorded.

**3. Named consumers and their stated use are listed.** This is the item most often omitted and most often regretted. Without it a Solution breaks silently the moment a second consumer arrives, and fitness for purpose cannot be assessed at all.
*Met when:* each consumer is named with the use they intend, specifically enough to test fitness against.

**4. Limitations, known gaps, and approved and unapproved uses are stated.** A consumer will otherwise assume a fitness we never offered. What a Solution is *not* for is part of what it is.
*Met when:* at least one unapproved use is stated.

**5. Gaps in our own specification are recorded.** A known unknown that is written down is a scoped risk. The same unknown left implied reads as coverage.
*Met when:* the specification carries an open-gaps list, or states that there are none.

**6. Questions belonging to engineering are recorded and routed, not answered.** Some questions are above ticket scope and establish precedent across pipelines — the mechanism for handling records that fail a blocking assertion is the standing example. Recording the question is ours. Answering it is not.
*Met when:* each open item carries a date, an owner and a status, and no proposed answer.

### B · Service and change

**7. Service levels are defined as SLOs, and the published promise as SLAs** — freshness, availability, timeliness. The SLO is what we hold ourselves to; the SLA is what a consumer is told. The SLO sits tighter, and the gap between them is the room we have to detect and correct before a promise breaks.
*Met when:* each measure has both a target and a promise, and the target is the tighter of the two.

**8. The required refresh cadence is stated as a requirement.** What consumers need. Not the schedule that achieves it.
*Met when:* the cadence is expressed as consumer need, with no schedule, trigger or orchestration named.

**9. The compatibility promise is stated** — which changes consumers may rely on us not making without a version. Backward, forward, full, or their transitive forms. We state the promise; how it is enforced is engineering's.
*Met when:* one named mode is chosen, not implied.

**10. Versioning and deprecation policy is defined, including the notice consumers are owed.** Change is inevitable. A policy is what makes it legitimate rather than a surprise.
*Met when:* the trigger for a version bump and the notice window are both stated in units of time.

**11. The change-notification channel is defined.** Consumers learn of change before it reaches them.
*Met when:* the channel is named and reaches every consumer listed at criterion 3.

**12. A retirement outline is stated** — how the Solution would be deprecated, who is notified, what depends on it. *(Synthesis.)*
*Met when:* the sunset window and the notification list are stated, even if retirement is remote.

### C · Sources and dependencies

**13. Every source is declared with its contract reference and its stability, and the Solution's behaviour when a source changes.** Shape, semantics or availability — quarantine, fail closed, last known good. Stated as a required outcome, never as a mechanism. *(Absorption behaviour: synthesis.)*
*Met when:* each declared source has a contract reference, a stability assessment, and a stated behaviour for all three change types.

**14. Canonical references and shared dimensions are declared, with their authoritative source and who governs it** — referenced, never redefined. Engineering is never left to choose which reference is canonical.
*Met when:* every shared concept links to its governed definition rather than restating it.

**15. Identifier conventions for shared entities are stated.** Two Solutions that key the same entity differently cannot be joined, however well each is defined.
*Met when:* the identifier used for each shared entity is named, with its source of truth.

### D · Build sequence and intent

*This is the layer no open standard covers and no tool produces. It is also what gate 2 rests on: the reason a rule exists is the part that cannot be recovered from reading the built pipeline.*

**16. Each stage of the build states its required end state, the reason the rule exists, its expected effect, and what it depends on.** The end state is a statement about the data, never about the operation that produces it. The reason is the business justification. *(New in v0.3 — the standard previously required only that material decisions carry a reason, which understated this.)*
*Met when:* no stage's required end state reads as SQL, and no stage's reason restates its effect.

**17. The full sequence is summarised in order, showing what each stage removes or derives and why.** Readable at a glance. This is also what makes row counts that do not reconcile to source explicable rather than a support burden.
*Met when:* every filter that removes source rows appears in the summary, in business terms.

**18. Material decisions outside the build sequence carry their reason.** Not every choice — the ones a maintainer would otherwise reverse by accident.
*Met when:* each constraint that looks arbitrary has one sentence saying why it is there.

### E · Correctness and assertions

**19. Every foreseeable hazard appears as an assertion, not as a note.** Expected fan-out from a one-to-many relationship, written as a warning, is something a reader can miss. Written as an assertion on uniqueness, it is a gate. *(New in v0.3.)*
*Met when:* no hazard identified anywhere in the specification exists only as prose.

**20. Assertions are declared in three classes, separately.** Unit assertions test logic against static inputs before anything is materialised. Data assertions are evaluated against real data after an asset builds. Runtime monitors observe production and cannot block a merge. They run at different times, against different inputs, and are owned differently, so they are never one combined table. *(New in v0.3.)*
*Met when:* each assertion sits in exactly one class, and each class is populated or explicitly empty.

**21. Severity is declared per assertion** — blocking or non-blocking. The mechanism that implements a failure is engineering's and is routed under criterion 6.
*Met when:* every assertion carries a severity, and none carries a handling mechanism.

**22. The threshold source is named for every runtime monitor** — specification or observed history. Where a correctness rule appears as both a data assertion and a monitor, both instances trace to the specification. Observed history is never used as a test oracle. *(New in v0.3.)*
*Met when:* no correctness threshold is sourced from observed history.

**23. Every assertion has somewhere for its binding to be recorded, and we verify the binding exists at acceptance.** We write the assertion; engineering writes where the check lives. This is the only substitute available for intent-drift detection: a specification bound to a check fails visibly when it stops being true.
*Met when:* the binding column is present and unwritten by us at hand-off, and populated at acceptance.

### F · Shape, access and trust

**24. The Solution models a single well-defined business entity at a stated grain.** This is what makes reuse possible later. It also means no per-consumer derived views live inside the Solution, and one Solution per specification.
*Met when:* the entity is nameable in one noun phrase and no consumer-specific view sits inside the boundary.

**25. The consumption interface is stated at the logical level.** What consumers read — the asset set, the semantic model, the published extract. Not the platform that serves it.
*Met when:* a consumer can tell what they connect to without asking us.

**26. Access and entitlement requirements are stated.** Who may read what, on what basis, and what is restricted, masked or withheld from whom. We state the required outcome; the mechanism is engineering's.
*Met when:* each consumer's entitlement is stated, and every restricted column or partition is named.

**27. Classification and sensitivity are defined.**
*Met when:* the Solution carries a classification, and any deviation at asset level is noted.

**28. Expected volume, growth, query patterns and cost envelope are stated as requirements.** *(Synthesis — established in general non-functional-requirements practice, uncommon in published data practice.)*
*Met when:* volume and growth carry numbers, and the envelope would still hold at twice the expected volume — or the specification says it would not.

**29. Catalog metadata is specified and the Solution is discoverable.** Descriptions come from the asset level; classification tags, ownership, domain and certification target are confirmed here.
*Met when:* someone who does not know the Solution exists could find it by searching for the concept.

**30. Trust status is set in the catalog.** Certified or deprecated. Consumers should not have to ask us whether a Solution is reliable.
*Met when:* the flag is set, not defaulted.

### G · Integrity of the specification itself

**31. Non-obvious rules and edge cases carry a worked example.** Concrete examples are what make acceptance criteria derivable, and they are the cheapest defence against a plausible misreading.
*Met when:* every rule with an edge case has an example showing input and expected result.

**32. The specification is versioned and dated, and names the version of this standard it was written against.**
*Met when:* version, date, criteria version and what changed are all visible.

**33. The three gates pass.**

---

## Level 2 · Data Asset

What must be true of each asset within the Solution.

**1. Grain is defined.** One row is exactly what. Every aggregation downstream rests on this sentence.
*Met when:* the grain is one sentence naming the entity and every qualifier that makes a row distinct.

**2. Every column's semantics are defined** — name, logical type, meaning, nullability. Meaning is the field only this seat can supply, and it is the content the catalog entry is written from later.
*Met when:* no column requires a reader to infer its meaning from its name.

**3. Units and precision are stated for every measure; temporal columns conform to the bi-temporal standard.** Currency, scale and rounding are stated here. System time, business time and which event a timestamp records are governed by that standard and referenced, not restated.
*Met when:* no numeric column leaves units, scale or rounding to convention, and every temporal column maps to a named role in the bi-temporal standard.

**4. Enumerated domains are stated with their allowed values, and unknown is distinguished from missing.** Null, blank and a sentinel do not mean the same thing, and consumers will pick whichever reading suits them.
*Met when:* every coded column has its permitted set and its representation of unknown.

**5. Keys and uniqueness are defined.**
*Met when:* the business key and any surrogate are both named, with the uniqueness that holds.

**6. Relationships are declared with their cardinality and the implication of that cardinality for the result.** The implication is in scope. The join that implements it is not.
*Met when:* each relationship states what its cardinality means for row counts and for unmatched references.

**7. Column-level classification and sensitivity are defined where applicable.**
*Met when:* every column carrying personal or restricted data is marked.

**8. Quality expectations are bound to columns** — null, valid value, duplicate and row-count thresholds. These become the data assertions at Solution level, which is where their severity and binding are recorded.
*Met when:* each expectation names a column and a threshold, and appears as a declared assertion.

**9. History behaviour is named where history matters** — which changes overwrite and which accumulate, per the bi-temporal and versioning standard.
*Met when:* the strategy is named per attribute group, not per asset by default.

**10. Historical coverage is stated.** From when history exists, and what the asset says about the period before that. *(Synthesis.)*
*Met when:* the earliest reliable period is stated, along with any known break in it.

**11. Late arrival, restatement and deletion behaviour are stated.** Whether a figure can change after publication, how far back, and how a deleted source record is represented. *(Synthesis.)*
*Met when:* a consumer can tell whether yesterday's number is final.

**12. Breaking change is classified for the asset.** Removing a column, renaming one, changing its type, or removing or tightening a constraint breaks consumers. Additive change does not. Stating which applies here is what lets the compatibility promise at Level 1 mean anything.
*Met when:* the asset states which of its declarations are load-bearing for consumers.

---

## Level 3 · Ticket

Short by design. When the specification is ready, the ticket carries very little.

**1. Links to the specification section it delivers.**

**2. Acceptance criteria expressible as an automated check.**

**3. Scope boundary stated** — what this ticket does not include, where a reader might assume it does.

**4. Dependencies identified, and their current state known.**

**5. Test data or fixtures identified.**

**6. Non-trivial transformation logic flagged for unit testing.**

Everything else the ticket used to carry now lives in the specification, once, where it can be maintained.

---

## Practices that support the gates

Not criteria — we cannot satisfy them alone, and a standard we hold ourselves to should not depend on anyone else's calendar. They are the cheapest ways we know to make the gates pass.

- **Review before hand-off, with a test perspective present.** Author, builder and tester reading the same specification catches the ambiguity the author cannot see. We request it; we do not require sign-off.
- **Read the specification back as questions.** Anything answerable only by asking us is a gate 1 failure.
- **Update the specification in the same change as the product.** The discipline that substitutes for tooling that does not exist.

---

## What this standard excludes

These are not oversights. They cannot be satisfied by our own work, so they do not belong in a standard we hold ourselves to.

- **The business rules and calculation logic behind column semantics.** We name the concept; the rule comes from the requirement.
- **Governance of canonical and reference data.** We declare the dependency. Governing the shared entity is a platform concern.
- **Platform capabilities** — registry provisioning, catalog features, compute.
- **Test-suite architecture and framework selection, and the handling mechanism for failed assertions.** We state the assertion and its severity; engineering chooses the mechanism.
- **Transformation logic, DDL, materialisation, join, deduplication and incremental strategy, partitioning, clustering, orchestration and scheduling, tuning.**
- **Access mechanism** — roles, grants, masking implementation. We state who may see what; how it is enforced is engineering's.
- **Estimation, points, velocity, capacity and ceremony.**
- **Vision, roadmap, business-need validation and cross-team prioritisation.**

The line running through this list: a specification states what must be true of the result. Anything that would change when the implementation changes belongs to whoever changes it.

---

## What we do not claim

**Published readiness checklists from working data teams are scarce.** This standard is assembled from open specifications and adjacent engineering practice, not adopted from a proven model. That is worth saying plainly.

**Five criteria are synthesis**, marked above: upstream-change behaviour, cost and performance as stated requirements, retirement at readiness, historical coverage, and restatement behaviour. Each is defensible and cheap. None is established practice.

**The build-sequence and assertion criteria are ours, not the field's.** Open standards cover the interface layer well and the intent layer not at all. Sections D and E are the part of this standard with no external anchor, which is also why they are the part that carries the value.

**No mature detection exists for drift between a specification of intent and a built pipeline.** Schema drift is detectable. Intent drift is not. Criterion 23 is the closest available substitute: bind what can be enforced, so divergence surfaces as a failed check rather than a discovered surprise.

**Catalog certification is binary.** Certified or deprecated. It is not a maturity tier, and bronze, silver and gold describe a modelling pattern rather than a level of trust.

**Semantic interchange standards are early.** The vendor-neutral work on shared metric and dimension definitions is incubating, not adopted. Criteria 14 and 15 are written to survive without it.

---

## Using a model against this standard

The highest-value use is finding what is missing. Ask a model what a specification leaves under-specified, then answer the questions ourselves. Use it to enumerate questions; never to supply coverage.

**One rule holds absolutely: a model may draft the interface layer from code. It never authors intent.** A model reading a pipeline records what the pipeline currently does — defects included — and presents it as what was intended. Section D is exactly where this fails: the reason a rule exists is not recoverable from the rule. Intent comes from us and from the consumer, or it does not exist.

Two failure modes to watch in generated drafts: output that looks exhaustive and omits the hard cases, and output that could describe any Solution. Both are caught by the same two checks:

- Could this be built from alone? Guards against a specification that reads complete and is not.
- Could this sentence be moved to a different product unchanged? If yes, it describes nothing.

---

## Compact checklist

**Data Solution** — purpose · owner and support · consumers and use · limitations and unapproved uses · gaps in our spec · items routed to engineering · SLOs and SLAs · refresh cadence as requirement · compatibility promise · versioning and deprecation notice · notification channel · retirement outline · sources with contract, stability and change behaviour · canonical references and shared dimensions · identifier conventions · stage end states with reasons · filter chain summary · decisions carry reasons · hazards as assertions · three assertion classes · severity per assertion · threshold source named · binding verified at acceptance · single entity at stated grain · consumption interface · access and entitlement · classification · volume, growth, cost envelope · catalog metadata and discoverability · trust status · worked examples · versioned and dated · gates pass.

**Data Asset** — grain · column semantics · units, precision, temporal roles · domains and unknown-versus-missing · keys · relationships with implication · column classification · quality expectations bound to columns · history behaviour · historical coverage · late arrival, restatement, deletion · breaking change classified.

**Ticket** — spec link · automatable acceptance criteria · scope boundary · dependencies · test data · unit-test flags.

---

*v0.3. Extended when a real Solution requires something this standard does not cover; versioned when it changes.*

---

## Change record

### v0.2 to v0.3

**Absorbed from the specification template**, which required these before the standard did:

- Section D, build sequence and intent — stage end state, reason, expected effect, dependency, and the filter-chain summary. Replaces v0.2's single "material decisions carry their reason" criterion, which is retained but narrowed to decisions outside the sequence.
- Section E, assertions — hazards as assertions rather than notes; the three-class split; severity per assertion; threshold source named with observed history barred as an oracle; binding recorded by engineering and verified at acceptance. Absorbs v0.2's "bound to enforceable artefacts" criterion, which said less.
- Sources now carry a contract reference and a stability assessment. Canonical references carry their governing owner.
- Relationships now carry the implication of their cardinality, not the cardinality alone.
- Open items split in two: gaps in our own specification, and questions routed to engineering with date, owner and status and no proposed answer.

**Removed:** the asset-level exclusions criterion. The filter-chain summary at criterion 17 covers it better and in the right place.

**Referenced rather than restated:** system time, business time and history behaviour now point at the bi-temporal and versioning standard. Units, scale and rounding remain here; temporal semantics do not.

**Added:** a section stating the template correspondence explicitly, since completeness of the template standing in for readiness depends on it.

**Count:** Solution level 26 to 33, grouped A to G. Asset level 13 to 12. Ticket unchanged at 6. Synthesis criteria remain five.

### Template changes this version requires

The template needs seven fields it does not currently have, for criteria 15, 25 and 26 at Solution level and criteria 3, 4, 10 and 11 at Asset level: identifier conventions; consumption interface; access and entitlement; precision and scale on the column table; allowed values and the representation of unknown; historical coverage; and late arrival, restatement and deletion. Its readiness table also still says twenty-five fields.

### v0.1 to v0.2

Solution level regrouped and given verification lines. Added: open questions; identifier conventions; binding to enforceable artefacts; consumption interface; access and entitlement; discoverability; decisions carry reasons; worked examples; versioned and dated. Asset level added units and time conventions; domains and unknown-versus-missing; historical coverage; late arrival, restatement and deletion; exclusions. Ticket level added scope boundary.
