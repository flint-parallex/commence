# Repository Structure

How this repository is organised, and why. Read before adding a directory.

## The organising principle

**Directories are the durable entity. Everything ephemeral is metadata or an index.**

A data product outlives the OKR that funded it, the release that shipped it, the sprint it was built in, and the catalog it currently sits in. Filing a specification under `IP-OKR-142/` makes the path wrong the following year. Filing it under its Unity Catalog path makes the path wrong the day the product moves to a derived catalog — a relocation that is not a change to the product at all.

So: **the data product is the directory, and nothing else is.** The Unity Catalog location is a field in the specification header. The OKR is a field. Any view that cuts across products — by OKR, by release, by catalog, by quarter — is an index that points at products, never a directory that contains them.

The product and its underlying assets are also the unit that generates work: decomposition cuts across the assets that must exist together to close a grain assertion, and that set is a property of the product rather than of where its schema happens to sit.

## Layout

```
commence/
├── REPO-STRUCTURE.md
├── .gitignore
│
├── <skill folders at root>          ← discovered by Databricks
│   specification-authoring/
│   delivery-readiness/
│   delivery-assessment/
│   delivery-communication/
│   shared/
│
├── products/                        ← flat. One directory per data product
│   └── <product>/
│       ├── specification.md              canonical. The hub
│       ├── data-model.sql                DDL: comments, keys, constraints
│       ├── work-set.md                   derived by decomposition
│       ├── tickets/
│       ├── test-plan.md                  derived from specification §6
│       ├── validation-report.md          derived from specification §6
│       ├── samples/                      sample data, evidence for §6a and discovery
│       ├── decisions/                    curated, with rationale
│       └── _raw/                         NOT COMMITTED
│
├── capabilities/                    ← cross-product, reusable. NOT data products
│   └── <capability>/
│       ├── reference.md                  what it is, what it constrains
│       └── usability-guidelines.md       pulled from Confluence
│
├── incidents/                       ← the incident is the durable entity
│   └── <incident-id>/
│       ├── report.md
│       ├── communications/               generated drafts and exec comms
│       └── _raw/                         NOT COMMITTED
│
├── releases/
│   └── <release-id>/
│       ├── release-plan.md
│       └── validation-summary.md
│
├── communications/                  ← periodic, cross-product outward artifacts
│   ├── executive-updates/
│   └── sprint-plans/
│
└── docs/
    ├── okr-map.md                   OKR → products. An index
    ├── release-map.md               release → tickets → products. An index
    ├── meetings/                    cross-team, curated
    ├── ways-of-working/
    └── _raw/                        NOT COMMITTED
```

## What goes where

| Artifact | Home | Why |
|---|---|---|
| Specification | `products/<product>/` | The product is the entity |
| Data model DDL | Alongside the specification | Emitted from it; they version together |
| Work set, tickets | Alongside the specification | Derived from it; traceable only if co-located |
| Test plan, validation report | Alongside the specification | Derived from §6 |
| Sample data | `products/<product>/samples/` | Evidence for §6a pairs and for discovery. Same lifecycle as the spec, same review |
| Decisions about **this product** | `products/<product>/decisions/` | Product-scoped rationale |
| Decisions about **how we work** | `docs/ways-of-working/` | Cross-product |
| Meeting notes, product-specific | `products/<product>/decisions/` | Curated as decisions, not as minutes |
| Meeting notes, team-wide | `docs/meetings/` | Curated |
| BA meeting transcripts, original requirement docs | `products/<product>/_raw/` — **not committed** | Inputs, not decisions |
| Pipeline library capability, shared utility reference | `capabilities/<capability>/` | Cross-product. Not a data product |
| Incident report, exec comms, incident transcripts | `incidents/<incident-id>/` | The incident is the entity; it frequently spans products |
| Catalog / schema location | Field in the specification header | Changes without the product changing |
| Executive update, sprint plan | `communications/` | Periodic and cross-product. Belongs to no single product, incident or release |
| OKR grouping | `docs/okr-map.md` | An index, never a directory |
| Release grouping | `docs/release-map.md` and `releases/` | An index, never a directory |

## Capabilities

A capability is a cross-product reusable thing that ends up in pipelines — a pipeline library component, a shared utility, a platform service the team consumes. It is **not** a data product: no grain, no consumers, no build sequence.

It gets a directory because specifications need to cite it the same way they cite an upstream product, and because a capability changing needs a findable blast radius.

**A capability directory holds references, not specifications.** If it accumulates a grain, declared assets and assertions, it has become a data product and moves to `products/`.

Cite capabilities from specification §3 by name and commit, never by path.

## Incidents

Incident material is shaped differently from everything else here. It is rarely scoped to one product — an incident often spans several, or concerns the pipeline rather than the data — and its raw material is the most sensitive in the repository. Incident transcripts carry candid remarks about people, vendors and other teams.

The incident is the durable entity. Where an incident clearly concerns one product, cross-reference it from that product's `decisions/` rather than filing it under the product.

**An incident is usually evidence that an assertion was missing.** The incident report cites the assertion ID that would have caught it, or records that none existed. That is what turns an incident record into a feedback loop into the specification rather than a document that is written and filed.

## Raw material

**`_raw/` is ignored by whole directory, not by pattern.** Pattern-based ignores fail silently in the wrong direction: anything that does not match gets committed and you find out afterwards. Whitelisting the container has no gaps.

Raw material is evidence, not decisions. Transcripts contain positions that were floated and withdrawn, and nothing in the text marks which survived. It stays local, available to the author, and out of both the repository and any skill's reference path.

**Meeting notes are curated as decisions with rationale, not as minutes.** The schema records what was decided; the decision note records what was considered and rejected. That is the only place *why* survives, and it is what stops a maintainer reversing a choice by accident.

## The specification is the hub

Everything downstream derives from it and references it rather than restating it.

```
intake (local)  →  specification.md  →  work-set.md  →  tickets/
                          │                                │
                          ├──────────→ test-plan.md        │
                          ├──────────→ validation-report.md│
                          └──────────→ data-model.sql      │
                                                           ↓
                                                    releases/
```

**Reference the specification at a commit, never at a branch.** A ticket pointing at `main` silently changes meaning when the specification is revised, and at acceptance nobody can tell which version was built against.

**Cite upstream products and capabilities by name and commit, never by path.** A product that moves catalogs, or a directory that is renamed, then costs a one-line edit rather than a hunt.

## Naming

- Products: stable, unambiguous, readable on its own. `13f-institutional-holdings`, not `filings`. Distinct enough that two products over the same source do not read as variants of each other
- Capabilities: the name used in the pipeline library
- Incidents: the incident identifier used in the tracking system
- Tickets: `<unit>-<short-slug>.md`
- Releases: the release identifier used in Jira
- Assertion IDs: `<SOLUTION>-<CLASS>-<NNN>`, append-only, never renumbered

## Skills

Skills sit at the repository root because Databricks discovers them by the presence of `SKILL.md`. `products/`, `capabilities/`, `incidents/`, `releases/`, `communications/` and `docs/` are siblings and are ignored by that discovery.

**Verify this before committing to the layout.** If Databricks does not tolerate non-skill siblings at the skills root, the fallback is two repositories — skills in one, everything else in the other — with the second cloned into the workspace separately.

### Boundaries

One skill per **moment in the lifecycle**, not per artifact type. Test a proposed new skill against this: if it fires at the same moment as an existing one, it is a reference file, not a skill.

| Skill | Moment | Location |
|---|---|---|
| `ba-requirements-conversion` | Intake — external requirement arrives | Local only. Processes raw material |
| `specification-authoring` | The canonical document is written or revised | Repository, Databricks-connected |
| `delivery-readiness` | Work is derived and readiness is gated | Repository, Databricks-connected |
| `delivery-assessment` | Delivered work is tested and assessed | Repository |
| `delivery-communication` | Something is said outward | Repository |

---

## Classifying a file

Work these in order. Stop at the first that applies.

**1 · Is it raw?**
Transcripts, original requirement documents, email threads, meeting recordings, anything received rather than authored. → the nearest `_raw/`, and it is not committed. Raw material never gets classified further; it does not move to a curated location by being useful.

**2 · Is it about an incident?**
Incident transcripts, incident reports, the exec communications generated from them. → `incidents/<incident-id>/`, raw inside its `_raw/`. This takes precedence over product scoping, even where the incident concerns exactly one product.

**3 · Is it a reusable capability reference?**
Applies across products, has no grain and no consumers, describes something the pipeline consumes rather than something the pipeline produces. → `capabilities/<capability>/`.
If it has a grain, declared assets and assertions, it is a product, not a capability.

**4 · Is it scoped to exactly one data product?**
→ `products/<product>/`, in the sub-location its type indicates: specification, DDL, work set, ticket, test plan, validation report, sample data, or decision note.
**Scoping test:** would this file still make sense if that product were retired? If yes, it is not product-scoped.

**5 · Is it an index across products?**
Groups products by OKR, release, quarter, catalog, or any other cut. → `docs/`, as a file. **Never as a directory.** An index that becomes a directory has made an ephemeral grouping into structure.

**6 · Is it about how the team works, rather than about any product?**
→ `docs/ways-of-working/` or `docs/meetings/`.

**7 · Anything else.**
Do not place it. List it as unclassified with what would be needed to decide.

### Rules for the classifier

**Do not default.** An unclassified file listed as a question is a better outcome than a wrong placement that has to be found later. There is no catch-all directory and one must not be created.

**Do not infer a product from a filename.** A file named `13f-notes.md` may be a decision note, a raw transcript, or a team meeting that mentioned 13F. Read it, or ask.

**A file mentioning several products is not multi-product.** Ask which product it is *about*. Where it is genuinely about the set rather than any member, it is an index or a ways-of-working document.

**Sample data is product-scoped even when it looks generic.** It is evidence for that product's §6a pairs and discovery survey, and it is reviewed alongside the specification.

**Where a file could be raw or curated, treat it as raw.** Curated material is authored deliberately and its author can confirm it. Misfiling curated material as raw costs a move; misfiling raw material as curated puts a withdrawn position into the record and, once pushed, into history.

**Flag anything already committed that belongs in `_raw/`.** It needs removing from history, not moving. That is a decision for the operator, not an action to take.

---

## Explicit destinations

Exact paths and filenames. Where a file matches a row here, place it there — do not re-derive from the classifier.

`<product>`, `<incident-id>`, `<release-id>` and `<capability>` are directory names per the Naming section. Dates are `YYYY-MM-DD`.

### Product artifacts

| File | Exact path |
|---|---|
| Specification | `products/<product>/specification.md` |
| Catalog DDL | `products/<product>/data-model.sql` |
| Proposed work set | `products/<product>/work-set.md` |
| Ticket | `products/<product>/tickets/<unit>-<slug>.md` |
| UAT / functional test plan | `products/<product>/test-plan.md` |
| Validation report | `products/<product>/validation-report.md` |
| Verification / discovery run output | `products/<product>/verification/<YYYY-MM-DD>-verification.md` and `-discovery.md` |
| Sample data | `products/<product>/samples/<descriptive-name>.<ext>` |
| Decision note, product-scoped | `products/<product>/decisions/<YYYY-MM-DD>-<slug>.md` |
| Curated meeting note, product-scoped | `products/<product>/decisions/<YYYY-MM-DD>-<slug>.md` |
| BA transcript, requirement doc, email thread | `products/<product>/_raw/` — **not committed** |
| Intake document as received | `products/<product>/_raw/` — **not committed** |

There is no `notes/`, `misc/`, `working/` or `archive/` directory under a product. Curated product material is a decision note or it is one of the named artifacts above.

### Capability artifacts

| File | Exact path |
|---|---|
| Capability reference | `capabilities/<capability>/reference.md` |
| Usability guidelines from Confluence | `capabilities/<capability>/usability-guidelines.md` |
| Any further capability document | `capabilities/<capability>/<slug>.md` |

### Incident artifacts

| File | Exact path |
|---|---|
| Incident report | `incidents/<incident-id>/report.md` |
| Executive communication about the incident | `incidents/<incident-id>/communications/<YYYY-MM-DD>-exec.md` |
| Confluence draft about the incident | `incidents/<incident-id>/communications/<YYYY-MM-DD>-<slug>.md` |
| Incident meeting transcript, raw notes | `incidents/<incident-id>/_raw/` — **not committed** |

### Release artifacts

| File | Exact path |
|---|---|
| Release plan | `releases/<release-id>/release-plan.md` |
| Validation summary | `releases/<release-id>/validation-summary.md` |
| Release communication | `releases/<release-id>/<YYYY-MM-DD>-<slug>.md` |

### Periodic communications

| File | Exact path |
|---|---|
| Executive update | `communications/executive-updates/<YYYY-MM-DD>.md` |
| Sprint plan | `communications/sprint-plans/<sprint-id>.md` |

### Cross-product documents

| File | Exact path |
|---|---|
| OKR index | `docs/okr-map.md` |
| Release index | `docs/release-map.md` |
| Team meeting note, curated | `docs/meetings/<YYYY-MM-DD>-<slug>.md` |
| Ways of working, process, standards | `docs/ways-of-working/<slug>.md` |
| Migration plan | `docs/migration-plan.md` |
| Team meeting transcript, raw | `docs/_raw/` — **not committed** |

### Skills

| File | Exact path |
|---|---|
| Skill entry point | `<skill-name>/SKILL.md` |
| Skill reference file | `<skill-name>/references/<slug>.md` |
| Content shared across skills | `shared/<slug>.md` |

### Not placed

Anything not matching a row above and not resolved by the classifier is listed as unclassified. **Do not create a directory to hold it.** A new top-level directory is a change to this document and requires a decision, not an inference.
