# Data Contract

Projecting a specification into an Open Data Contract Standard (ODCS) contract, and
keeping the two in step without letting the contract become a second author.

The specification is the source. The contract is a **projection** of it — generated,
diffed and replaced, never hand-edited and never consulted as a fact. Everything below
exists to keep that direction of flow one-way.

---

## The governing rule

**The contract is a projection, never a source.**

If the contract needs a fact the specification does not state, the fix is in the
specification. Never fill the contract from the table, from the catalog, or from what
would make it validate. That is sourcing, and `verification.md` forbids it — the contract
is exactly the place where sourcing is most tempting, because a validator will name the
missing field and the platform can supply a plausible value in one query.

A contract that fails validation because the specification has a gap is **correct output**.
Report it as a gap with the owner named. Do not close it.

---

## Pinned reference

The standard is vendored, not fetched. Field names, required fields and enumerations are
resolved against the pinned copy in `references/odcs-<version>/` — never against memory
of the standard, and never against the rendered docs site.

Two authorities, and they are not interchangeable:

| Artifact | Use it for |
|---|---|
| The section markdown | What a field means, and which of several near-synonyms is the right target |
| The JSON Schema | Whether the emitted contract is valid. This is the only conformance test |

Since v3.1.0 the standard validates strictly — undefined fields are rejected. A field name
that cannot be confirmed in the pinned schema is **an open item, not a guess**. Emitting a
plausible field name that the schema does not define produces a contract that fails
validation for a reason that looks like a bug in the tooling rather than a gap in the
specification.

The contract is YAML. The JSON Schema validates the parsed document, not the file format;
output stays YAML end to end.

---

## When the contract is written

The specification's `Status` field is the gate. Drafting is a period of deliberate
instability — grain gets revised, columns get renamed, an entity turns out to be two.
Projecting each of those into the contract produces version churn on an artifact whose
whole value is that consumers can rely on it.

| Spec status | Contract behaviour |
|---|---|
| **Draft**, no contract exists | Not generated. Do not raise it. Edits proceed freely |
| **Draft**, contract exists | Not written. Contract-bearing edits accumulate in a pending list, reported **once at the end of a working session**, never per edit |
| **Ready** | Reconcile. Mandatory, automatic, before the status change completes |
| **Delivered** | The contract is published and consumers depend on it. Any contract-bearing edit is a version event — reconcile before the spec edit is reported complete |

**Inconsistency during drafting is not an error. Inconsistency at a gate is.**

The operator may also call for reconciliation at any point. An explicit request overrides
the gate; nothing else does.

---

## Which edits are contract-bearing

Only these sections project. An edit outside them never triggers anything.

| Spec | Projects to |
|---|---|
| Header table — Data Solution, Version, Status | Contract identity and version |
| Header — Unity Catalog location | Server and physical location |
| §1 Purpose, entity and grain | Description block |
| §1 Owner, support channel | Team and support |
| §2 Cadence, delivery windows, service levels | SLA properties |
| §4 Columns — logical type, meaning, units, nullable, classification | Schema properties |
| §4 Grain, keys and uniqueness | Primary key and uniqueness |
| §4 Relationships | Relationships (v3.1.0 and later) |
| §6a/6b assertions | Quality rules, declared only — see below |
| §7 Classification, compatibility promise, limitations and approved use | Description limitations, tags, custom properties |
| §8 Domain, ownership, classification tags, certification target | Domain, tags, custom properties |

Not contract-bearing: §3 sources, §5 build sequence, §6c runtime monitors, §9 open items,
§10 readiness. These describe how the product is produced and how delivery is run — the
contract states what consumers are promised.

**Resolve every target above against the pinned section markdown before emitting.** The
column names the specification uses and the field names the standard uses are close enough
to be mapped by eye and different enough to be mapped wrongly.

### Assertions

§6a and §6b assertions project as declared quality rules carrying the Assertion ID.

**The binding column never projects.** Engineering's check identifier is not specification
content, and it is not contract content either. The contract declares what must be true;
where the check lives stays in the specification's binding column, written at acceptance.

§6c runtime monitors do not project at all. A monitor is an operational control, not a
consumer promise.

---

## Reconciling

Never edit the contract in place. Regenerate it and diff — the diff *is* the change
detection, and it is deterministic in a way that remembering what changed is not.

1. **Run the consistency sweep** (`consistency-sweep.md`). A contract generated from an
   internally inconsistent specification propagates that inconsistency into a
   machine-readable artifact that downstream tools will trust. Sweep first, always.
2. **Regenerate** the contract from the current specification.
3. **Carry forward what the specification does not own** — see below.
4. **Diff** against the committed contract. Empty diff means say nothing and stop.
5. **Classify the change** and derive the version bump.
6. **Validate** against the pinned JSON Schema.
7. **Report** the diff, the bump and its justification. Then write.

### Carried forward, never regenerated

Regeneration will otherwise destroy these or churn them on every run:

| Field | Why |
|---|---|
| Contract `id` | A regenerated UUID produces a spurious diff every run and breaks any consumer reference to the contract |
| `servers` | Connection detail. Engineering owns it |
| `physicalType` per column | The specification declares the *logical* type. The physical type is engineering's choice of how to carry it |
| Contract creation timestamp | Set once |
| Hand-curated custom properties | Anything an operator added deliberately that no spec field projects to |

Merge only over spec-derived paths. If a carried-forward field conflicts with the
regenerated content — a `physicalType` that cannot carry a newly changed `logicalType` —
that is a **finding to report**, not a conflict to resolve. Engineering decides.

---

## Version bumps

The specification already declares what breaking means. Read it; do not judge it.

§4 **Breaking change classification** names which changes to that asset break consumers.
§7 **Compatibility promise** names what consumers may rely on us not doing without a
version. Between them the bump is determined:

| Change | Bump |
|---|---|
| Anything §4 classifies as breaking — column removed, renamed, retyped, constraint removed or tightened | **Major** |
| Additive — new column, new relationship, new assertion, loosened constraint | **Minor** |
| Description, meaning, units, classification tag, ownership, or any change with no structural effect | **Patch** |

Where §4's classification does not cover an observed change, that is a gap in §4. Report
it and ask; do not extend the classification by inference. The point of reading the bump
from the specification rather than judging it is defeated the moment the judgement moves
here.

A major bump against a **Delivered** specification triggers §7's notice window and change
notification. Report both alongside the bump. Do not action them.

---

## Version changes in the standard

Upgrading the pinned ODCS version is a reviewed change, never a background sync.

1. Vendor the new tag into a parallel `references/odcs-<new>/`. Keep the old one.
2. **Diff the JSON Schemas**, not the prose — new required fields, removed fields, changed
   enumerations. That is where breakage lives.
3. Update the mapping table above for renames and deprecations.
4. Regenerate every existing contract against the new version and diff. That diff is the
   blast radius, seen before anything is committed.
5. Bump `apiVersion` in the emitted contract.

Contracts declare their own `apiVersion`, so a mixed estate is valid during migration.
Retire the old reference folder when the last contract has moved, not before.

---

## What this never does

- **Never edits a contract by hand.** Regenerate, diff, replace
- **Never fills a field the specification does not carry.** The gap is the output
- **Never queries a table to resolve a contract field.** That is sourcing
- **Never bumps a version to make a diff look smaller,** or holds one back to avoid a
  notice window
- **Never writes during Draft** unless the operator asks
- **Never proposes a specification change to make the contract validate.** Report what is
  missing and who owns it
