# Catalog Comments

Applying dataset and column descriptions to Unity Catalog so data is findable.

The deliverable is **not** the descriptions — those are authored by the Product Owner and supplied with the ticket. The deliverable is an **automated, repeatable process** that applies them, lives in the repository, and carries forward as code is promoted to new environments.

A manual application satisfies the descriptions being present today and fails the actual goal. The process is the deliverable.

## Required inputs

Two CSV files, attached to the ticket. Reference them as the specification; never inline their contents.

**Dataset comments** — columns: `dataset`, `description`

**Column comments** — columns: `dataset`, `column`, `description`

Also required:
- Target environment
- Target catalog and schema
- OKR tag (IP-OKR-###)

If either file is missing, the ticket is not ready. Never generate descriptions.

## Structure

Follow `user-story.md`. The consumer outcome is discoverability:

> As a *[consumer]*, I need dataset and column descriptions available in Unity Catalog so that I can find and understand *[dataset]* without asking the delivery team.

### Out of scope

Standard exclusions for this ticket type:

- Authoring or curating the descriptions themselves. These are supplied with the ticket
- Any dataset or column not present in the attached files
- Descriptions for objects in environments beyond the stated target. The process carries forward on promotion; applying it elsewhere is not this ticket

### In scope

An automated process, held in the repository, that applies the supplied dataset and column descriptions to the named catalog objects and re-applies on promotion to a new environment.

## Acceptance criteria

Per `acceptance-criteria.md`. Assertion form.

**Coverage and fidelity**
- Every dataset listed in the dataset comments file has its Unity Catalog comment populated.
- Every column listed in the column comments file has its Unity Catalog comment populated.
- Each applied dataset comment matches the supplied description exactly.
- Each applied column comment matches the supplied description exactly.
- No dataset or column outside the supplied files has been modified.

**Process**
- The process that applies the comments is held in the repository, not run manually.
- The process is idempotent — re-running produces no change to already-correct comments and no duplication.
- On promotion to a new environment, the process applies the comments to that environment's objects without manual intervention.
- The source files the process reads are versioned in the repository alongside it.

**Verification**
- Applied comments are queryable from Unity Catalog metadata and can be compared against the supplied files.

That last criterion is what makes exact match checkable rather than asserted. State it so acceptance is a comparison, not an inspection.

## Rules

- Never generate, edit, or improve a supplied description. Exact match is the criterion; any change to wording breaks it.
- Reference the CSV files as attachments. Do not inline their contents into the ticket.
- Automation is the deliverable. A criterion stating the comments are present is insufficient on its own — pair it with the process criteria.
- Environment portability is asserted, not assumed. It is the reason this is a process rather than a one-time task.
- If a description is missing for a column present in the target table, that is a gap in the supplied file — raise it. Do not write one.
- Neutral and brief.
