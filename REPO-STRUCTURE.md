# Repository Structure

How this repository is organised, and why. Read before adding a directory.

## The organising principle

**Directories are the durable entity. Everything ephemeral is metadata or an index.**

A data product outlives the OKR that funded it, the release that shipped it, and the sprint it was built in. Filing a specification under `IP-OKR-142/` makes the path wrong the following year and forces a move that breaks every link pointing at it.

So: **the data product is the directory. The OKR is a field.** Any view that cuts across products — by OKR, by release, by quarter — is an index that points at products, never a directory that contains them.

The product path mirrors Unity Catalog. `products/<catalog>/<schema>/<product>/` is the same coordinate as the data itself, which means a path in this repository resolves to a path in the platform without translation.

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
├── products/                        ← mirrors Unity Catalog
│   └── <catalog>/
│       └── <schema>/
│           └── <product>/
│               ├── specification.md        canonical. The hub
│               ├── data-model.sql          DDL: comments, keys, constraints
│               ├── work-set.md             derived by decomposition
│               ├── tickets/                one file per ticket
│               ├── test-plan.md            derived from specification §6
│               ├── validation-report.md    derived from specification §6
│               ├── decisions/              curated, with rationale
│               └── _raw/                   NOT COMMITTED
│
├── docs/
│   ├── okr-map.md                   OKR → products. An index
│   ├── release-map.md               release → tickets → products. An index
│   ├── meetings/                    cross-team, curated
│   ├── ways-of-working/
│   └── _raw/                        NOT COMMITTED
│
└── releases/
    └── <release-id>/
        ├── release-plan.md
        └── validation-summary.md
```

## What goes where

| Artifact | Home | Why |
|---|---|---|
| Specification | `products/.../<product>/` | The product is the entity |
| Data model DDL | Alongside the specification | Emitted from it; they version together |
| Work set, tickets | Alongside the specification | Derived from it; traceable only if co-located |
| Test plan, validation report | Alongside the specification | Derived from §6 |
| Decisions about **this product** | `products/.../<product>/decisions/` | Product-scoped rationale |
| Decisions about **how we work** | `docs/ways-of-working/` | Cross-product |
| Meeting notes, product-specific | `products/.../<product>/decisions/` | Curated as decisions, not as minutes |
| Meeting notes, team-wide | `docs/meetings/` | Curated |
| Transcripts, original BA docs, email threads | `_raw/` — **not committed** | Inputs, not decisions |
| OKR grouping | `docs/okr-map.md` | An index, never a directory |
| Release grouping | `docs/release-map.md` and `releases/` | An index, never a directory |

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

## Skills

Skills sit at the repository root because Databricks discovers them by the presence of `SKILL.md`. `products/` and `docs/` are siblings and are ignored by that discovery.

**Verify this before committing to the layout.** If Databricks does not tolerate non-skill siblings at the skills root, the fallback is two repositories — skills in one, products and docs in the other — with the products repository cloned into the workspace separately.

### Boundaries

One skill per **moment in the lifecycle**, not per artifact type. Test a proposed new skill against this: if it fires at the same moment as an existing one, it is a reference file, not a skill.

| Skill | Moment | Location |
|---|---|---|
| `ba-requirements-conversion` | Intake — external requirement arrives | Local only. Processes raw material |
| `specification-authoring` | The canonical document is written or revised | Repository, Databricks-connected |
| `delivery-readiness` | Work is derived and readiness is gated | Repository, Databricks-connected |
| `delivery-assessment` | Delivered work is tested and assessed | Repository |
| `delivery-communication` | Something is said outward | Repository |

## Naming

- Products: the Unity Catalog name, unchanged. No aliases
- Tickets: `<unit>-<short-slug>.md`
- Releases: the release identifier used in Jira
- Assertion IDs: `<SOLUTION>-<CLASS>-<NNN>`, append-only, never renumbered
