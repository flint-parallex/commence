# User Story

The default ticket type for delivering a functional change a business consumer receives.

Write a user story when the work produces something a consumer can use. If the work is purely internal engineering with no consumer-facing outcome, it is a task, not a story — do not force a story frame onto it.

## Required inputs

Ask for any missing. Never invent table names, grain, sources, keys, or dates.

- The epic this rolls up to
- The consumer — who receives the outcome
- The functional change: what the system does after this that it does not do now
- Source system, tables, target tables, grain
- Data model specification (link, not inline)
- Known technical constraints
- What the epic or requirement states as the desired outcome, and what inputs have actually been provided to achieve it

That last input is what makes the out-of-scope section derivable rather than recalled.

## Structure

### Overview *(H2)*

**Story statement.** Business-outcome focused. The user is the business consumer, not the engineer building it.

> As a *[consumer]*, I need *[outcome]* so that *[business value]*.

**Do not write the engineer as the user.** "As a data engineer, I want monitoring capabilities so I get alerted" is wrong — that describes the capability being built, not the outcome the business receives. The correct frame for the same work: "As a Data Strategy consumer, I need confidence that the dataset is complete and current so that analysis is not run on stale or partial data."

The capability is what the engineer builds. The outcome is what the business gets. The story states the outcome.

**Description.** Two sentences. What functionality this delivers, and what the consumer receives when it is done. Optional, but include it wherever the outcome is not self-evident from the story statement — it is usually the section that makes the ticket legible in grooming.

### Scope of Work *(H2)*

#### Out of scope *(H3)* — required

**Derive this. Do not only record what was verbally excluded.**

Method:

1. Take the outcome the requirement or epic states
2. Take the inputs, sources, and tools actually provided to achieve it
3. Where the stated outcome is broader than the provided means can deliver, **the residual is out of scope**
4. Name the residual explicitly and state where responsibility for it sits

This is the section that prevents a deliverable being judged against an outcome it was never given the means to reach. Where an epic states a desired end state and the ticket has been given only partial means to it, the gap between them is named here rather than discovered at acceptance.

Also record: standard exclusions (for a first build, empirical monitors requiring production data history), and anything explicitly excluded in conversation.

If nothing is out of scope, write "None." The section is never omitted.

#### In scope

A short statement of what is included. **Not a checklist.**

A checklist positioned near the acceptance criteria gets treated as acceptance criteria, and task lines like "configure Datadog monitors" end up masquerading as gates. Keep it to a few lines, and note that completion is verified by the acceptance criteria.

Do not overstate. In scope describes the deliverable; the AC verify it. If in scope claims more than the AC verify, the ticket cannot be closed cleanly.

#### Known constraints

Technical constraints only. Omit the section if there are none.

> Source data is only available via the public website; no API exists.
> Must use the existing ingestion pattern.

**An open approach question is not a constraint.** If implementation approach is undecided, the story is not ready per `definition-of-ready.md` — it is a spike. Do not record an unresolved approach question here.

### Data model

Reference the specification artifact. Do not inline the column list.

### Acceptance criteria *(H2)*

Per `acceptance-criteria.md`. That file owns the categories, the writing rules, and the task-versus-criterion test. Do not restate them here.

Categories applicable to a user story, generated only where relevant to the change:

- Data model / schema conformance
- Data quality
- Complete data capture
- History and bitemporality *(where the table is bitemporal or SCD)*
- Scheduling
- Pipeline observability
- Error handling and alerting

Runtime monitors do not gate a merge. They are a deliverable of the unit, verified post-deploy — the criterion is that the monitor is deployed and its binding recorded. Split by threshold source per `decomposition.md`.

**For pipeline-heavy stories,** group the criteria under subheadings — table build, pipeline observability, error handling — so the list stays readable. Grouping is presentational; the rules are unchanged.

## Rules

- The user is the consumer. Never the engineer.
- Out of scope is required and derived, not only recalled.
- In scope is a statement, not a checklist.
- Constraints are technical. An undecided approach means spike, not story.
- Reference the data model; do not inline it.
- No duplication between parent and subtask, or between epic and story.
- Tag to an OKR (IP-OKR-###). Ask for it if not provided.
- Outcome-framed title: what the story achieves, not the activity.
- Neutral and brief. No padding, no restating the requirement back. A long ticket slows delivery as surely as a vague one.
