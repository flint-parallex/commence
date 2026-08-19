# AGENTS.md

Working context for this repository. Read `REPO-STRUCTURE.md` before creating any file.

**This file points; it does not restate.** Where anything here appears to contradict a skill file, the skill file wins and this file is wrong. Do not copy rules out of skill files into here.

## What this repository is

Delivery artifacts and the skills that produce them, for data products built on Databricks. It is operated by one product owner. Engineering builds from what leaves here.

## The pipeline

```
BA material  →  requirements doc  →  specification.md  →  work set  →  tickets  →  releases
                                            │
                                            ├──→  data-model.sql        (catalog DDL)
                                            ├──→  checks/               (assertion SQL)
                                            ├──→  test-plan.md
                                            └──→  validation-report.md
```

**The specification is the hub.** Everything downstream derives from it and references it rather than restating it. If you are about to duplicate specification content into another artifact, reference it instead.

## Which skill, when

One skill per moment in the lifecycle. If a task fires at the same moment as an existing skill, it is a reference file, not a new skill.

| Moment | Skill |
|---|---|
| External requirement arrives; consolidate and find the gaps | `ba-requirements-conversion` |
| Author or revise a specification, data model, semantics, assertions | `specification-authoring` |
| Derive work, gate readiness, write tickets, plan releases | `delivery-readiness` |
| Test and assess delivered work | `delivery-assessment` |
| Say something outward | `delivery-communication` |

## Invariants

These cut across every skill. Each has an owner file; go there for the full rule.

**Ask, never invent.** Entity, grain, keys, consumers, thresholds, business reasons — if it was not supplied, ask. A stated gap is visible; an invention is not. → `specification-authoring/SKILL.md`

**Verify and discover, never source.** Real tables may be queried to test a declared fact and to find content no rule covers. They may never be queried to decide what the fact should be. → `specification-authoring/references/verification.md`

**The specification is the oracle, not the table.** Where a declared fact and the data disagree, that is a finding to report, not a correction to apply. Never propose a specification edit in the same output as a verification failure. → same file

**Observed history is never a test oracle.** A threshold derived from what the pipeline currently emits cannot fail. It will pass on the day the product silently breaks. → `specification-authoring/references/runtime-monitors.md`

**The binding column stays blank.** The author declares the assertion and its ID. Engineering records where the check lives, at acceptance. An unbound assertion is *unknown*, never passing. → `specification-authoring/references/assertion-authoring.md`

**Assertion IDs are append-only.** Never reused, never renumbered. A renumbered ID silently re-points every historical check result at a different requirement. → same file

**No implementation in a specification.** No joins, no operation order, no materialisation, partitioning, scheduling or tuning. The test: would this statement change if engineering chose a different implementation? → `specification-authoring/SKILL.md`

**One entity, one specification.** A document covering two Solutions has organised around a source system instead of a product. Split it. → `delivery-readiness/references/definition-of-ready.md`

**Cut at the grain assertion.** Work units end where a grain assertion can be evaluated — not at table or pipeline-stage boundaries. → `delivery-readiness/references/decomposition.md`

**Refuse rather than fabricate.** "This does not decompose," "the reading cannot be determined here," "the material does not support this" are correct outputs. A plausible artifact built over a gap is worse than a stated gap. → every skill

## Referencing

**Cite the specification at a commit, never at a branch.** A ticket pointing at `main` silently changes meaning when the specification is revised, and at acceptance nobody can tell which version was built against.

**Cite upstream products and capabilities by name and commit, never by path.** A product that moves catalogs then costs a one-line edit rather than a hunt.

## Raw material

`_raw/` directories are never committed and are never in a skill's reference path. They hold transcripts, original requirement documents and email threads — evidence, not decisions, containing positions that were floated and withdrawn with nothing marking which survived.

Author from curated material. Where raw material is the only source for a statement, attribute it and flag it for confirmation.

## Before creating a file

1. Check `REPO-STRUCTURE.md` → Explicit destinations. Place from the table.
2. If no row matches, work the classifier in that file.
3. If neither resolves it, **say so and stop.** Do not create a directory. A new top-level directory is a change to `REPO-STRUCTURE.md` and requires a decision.

## Do not

- Restate specification content in a downstream artifact. Reference it.
- Populate a binding, a severity nobody stated, or a threshold derived from data.
- Write epics, stories or tickets from a requirements document. They derive from a specification.
- Create `notes/`, `misc/`, `working/`, `archive/` or `tests/` anywhere.
- Fill a template field to avoid leaving it blank.
- Commit anything under a `_raw/` path.
