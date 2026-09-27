# Epic

An epic is the **charter for a business problem**. It states what is wrong, what solving it is worth, how success will be measured, and what it will take to resource. It decomposes into stories, tasks, and subtasks.

This is not the agile convention of bundling a body of consumer work. The unit is the business problem, and one business problem may span several data products. An epic is not one-to-one with a specification.

Write an epic when there is a business problem with a named consumer, a measurable outcome, and enough work to span multiple tickets. If it fits in one ticket, it is not an epic.

**The initiative is not authored.** Everything rolls up to the single platform-level initiative, which exists for SVP reporting. Name it in the header and write nothing for it.

Rendered per `org-formats/epic.md`.

## The business context gate

**Stop and flag if the inputs are not sufficient to write the business-facing sections.**

The summary, business problem, and business impact are business-facing. They must come from a business source — a requirement, a BA document, a stakeholder conversation. **Never derive them from technical content.** Given only a description of code to be fixed, a pipeline to be built, or a table to be delivered, a business problem can always be constructed that sounds plausible and is not grounded in anything a stakeholder said.

Sufficient business context means all four are known:

1. **Who the consumer is** — a named group, not "the business"
2. **What they cannot do today**, stated without reference to implementation
3. **The consequence** of that gap — what is slower, manual, unreliable, or unavailable
4. **What they will do** with the deliverable

The test: **can the business problem be stated without mentioning any table, pipeline, system, or code?** If not, the context is technical and the business sections cannot be written from it.

When context is insufficient, do not draft the sections and do not fill them with plausible language. State it:

> Insufficient business context to write the business problem and business impact. Missing: *[which of the four]*. The inputs provided describe *[the technical work]*, which is not a business problem. Provide the requirement or BA source, or these sections should be drafted with the business owner.

Everything else in the epic — tables, dependencies, stakeholders, DoD — can be written from technical inputs. Only the business-facing sections are gated.

## Required inputs

Ask for any of these that are missing. Do not invent them — especially named people, table names, dates, and metrics.

- Initiative this epic rolls up to
- Business problem being solved, and for whom
- Every data product this epic spans, and the specification for each
- Tables or datasets delivered, with grain
- Named business owner, BA, consumer, upstream source owner, segment
- Target delivery date, if one exists
- Quarter this epic belongs to
- Quantifiable outcomes the data product must achieve
- Resource requirements — for requirements, and for testing, separately
- Known dependencies, including any external to the delivery team

## Structure

### Header fields
- **Initiative:** the platform initiative. Fixed
- **Quarter:** required on every epic
- **Assignee:** the Product Owner
- **Target delivery date:** or `none`, where no deadline exists
- **DoD gate:** prod (default for a data product epic)

### Summary
What this epic delivers and why it matters. Two to four sentences. This carries the strategic framing — do not add a separate strategic value section.

### Business problem
What is broken, missing, or unavailable today, and for whom. Present tense, factual. Not a description of the solution.

### Business impact
What this enables once complete. Bulleted. Outcome-framed — what becomes possible, not what gets built.

### Scope boundary
What this epic does **not** include. Name adjacent work that could reasonably be assumed in scope and is not, and where it goes instead.

This section is required. It is where scope creep is managed and where "can you also add..." gets resolved by reference rather than by negotiation.

### Data products delivered

An epic may span several. One row per data product, each linked to its own specification — the epic references them and never restates their content.

| Data product | Specification | Tables | Grain | Business description |
|---|---|---|---|---|

Business description is what the table represents to a consumer, not its technical structure.

Where an epic spans more than one specification, each specification keeps its own grain, assertions and data contract. The epic is the business case they share, not a merged model.

### Dependency sequence

Order of completion, not just a list of relationships. For each dependency state:
- What must complete before what
- Whether it is internal to the delivery team or external
- Current status: confirmed, pending, or blocked

Mark external dependencies explicitly. Those are the ones that slip.

### Key stakeholders

| Role | Name |
|---|---|
| Business owner (Data Strategy) | |
| Business analyst | |
| Consumer | |
| Upstream source owner | |
| Delivery team | CAP Data Engineering |
| Segment | |

Delivery team is always CAP Data Engineering. Upstream source owner is required — it is the party accountable for the data contract every downstream ticket depends on, and naming it at epic level surfaces the gap before any ticket is written.

### Success criteria

How success will be measured after delivery. Quantifiable outcomes only, expressed as numbers where possible — coverage, volume, freshness, latency, match rate, completeness.

These are not acceptance criteria. Acceptance criteria gate a merge; success criteria answer whether the initiative was worth doing, and are assessed once it is in use. A success criterion that can be checked by running a test is an acceptance criterion in the wrong place.

Delivery date belongs in the header. Stakeholder sign-off belongs in the Definition of Done. Keep this section to measurable product outcomes so it does not become a parking area.

### Resource requirements

Stated separately for **requirements** and for **testing**. These are distinct commitments and the organization tracks them as such — one line covering both is an incomplete epic.

Name what is needed from whom: BA time for requirements authoring, business tester availability for UAT, and any party outside the delivery team whose capacity this depends on.

Unknown is a valid answer and is reported as an open question. A guessed figure is not — it becomes someone's committed capacity the moment it is written.

### Definition of Done

**Link the canonical DoD. Do not copy it.** Resolve the live page via `references/confluence-map.md`. Declare the target gate in the header.

Write inline only criteria genuinely specific to this data product. Everything standard is inherited from the canonical DoD by reference.

Standard criteria the canonical DoD covers — reference, do not restate:
- Delivery complete against the defined scope
- Data quality standards met for the dataset
- Catalog configuration: registered in the data catalog, consumer read permissions granted, AWS Glue activated, Unity Catalog column and data descriptions populated, column naming aligned to the column naming standard, table naming aligned to the table naming standard
- Documentation and monitoring: baseline monitors in place, alerts routed to the correct channels, pipeline monitors configured and observable in Datadog
- Stakeholder acceptance: sign-off from the named Data Strategy business analyst

If the canonical DoD is unreachable, apply the fallback in `references/confluence-map.md`. Do not substitute a copied version.

## Rules

- Two "why" sections only — business problem and business impact. Strategic framing lives in the summary. Do not add a third.
- Scope boundary is required, not optional.
- Quarter label and Product Owner assignee are required on every epic.
- Resource requirements name requirements and testing separately.
- An epic spanning several data products links each specification. It never merges or restates their models.
- Copy no standard that has a canonical home. Link it.
- Name real people in stakeholders. If a role is unfilled, write `unassigned` — never a placeholder or a guess.
- State dependency and stakeholder status as neutral fact. Never as a blame narrative.
- Title per `ticket-titles.md`. The title names the thing; the summary and business problem carry the outcome. Six words, fifty-five characters.
- Tag to an OKR (IP-OKR-###). Ask for it if not provided.

## Worked example

**Title:** Form D dataset

**Initiative:** Investment Platform
**Quarter:** Q4 2026
**Assignee:** Product Owner
**Target delivery date:** [date]
**DoD gate:** prod

**Summary.** Delivers Form D filing data as a governed data product, giving Data Strategy direct access to private-markets filing data currently unavailable in a consumable form. Establishes the pattern other filing-type products follow.

**Business problem.** Form D filing data is not available to Data Strategy in a queryable, governed form. Analysis requiring private-markets issuer and offering data cannot be performed without manual extraction.

**Business impact.**
- Data Strategy can query Form D issuer and offering data directly
- Private-markets coverage becomes available for downstream analysis
- Establishes the delivery pattern for subsequent filing-type data products

**Scope boundary.** Does not include historical backfill beyond the stated window, entity resolution against existing issuer identifiers, or downstream reporting surfaces. Entity resolution routes to the matching initiative.

*(Remaining sections completed per the structure above.)*
