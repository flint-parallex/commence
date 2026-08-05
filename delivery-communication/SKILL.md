---
name: delivery-communication
description: Writes stakeholder-facing delivery communications for the data engineering team — sprint plans, release plans, quarterly plans, status reports, grooming agendas, and incident reports. Use when publishing what a sprint or quarter delivers, communicating a release, reporting delivery status or progress against goals, preparing a stakeholder grooming agenda, or writing up a production incident. Triggers on "sprint plan", "what are we delivering", "status update", "write up the incident" without an explicit request.
---

# Delivery Communication

Artifacts that **describe** the delivery pipeline, written for people outside the delivery team.

## Scope

**In:** sprint plans, release plans, quarterly plans, status reporting, progress against goals, stakeholder grooming agendas, incident reports.

**Out:** tickets, epics, spikes, acceptance criteria, decomposition, readiness assessment, technical pre-reads. Those belong to `delivery-readiness`.

The test: does this artifact **enter** the delivery pipeline, or **describe** it?

## The governing principle

**Published detail is surface area.**

`delivery-readiness` is exhaustively detailed because engineers build from it. This skill is the opposite discipline. A ticket manifest lets any reader reconstruct what slipped, what was cut, and what changed scope. A deliverable-level summary does not, and it answers the reader's actual question better.

The test for any line: **does the reader need this to know what they are getting?**

Ticket keys, ticket counts, sprint attribution, velocity, capacity, burndown detail, assignments, and scope-change history all fail it. They invite questions that do not serve the reader and expose composition that is nobody's business outside the team.

**Composition is internal.** Ticket-level detail is kept in working notes, never published. If a published artifact and an internal one cover the same release or sprint, they derive from one source — divergence between them destroys trust in both.

## Voice

**Concise.** Most of these artifacts are one page. If one does not fit, it is carrying detail it should not.

**Outcome-level.** What a consumer receives, in consumer language. Not what is inside it, not how many pieces, not who built it.

**Neutral.** Dependency status, gaps, and blockers stated as fact. "Contract version pending from the platform team" — never "blocked because platform hasn't delivered." This holds without exception, including in incident reports, and especially there.

**Unannotated.** A date that moved is stated as a date. Do not explain why it changed, do not narrate the history, do not add reassurance.

## Audience shaping

Every artifact declares its audience, and the audience determines what it contains — not just its tone.

| Audience | Wants to know | Never needs |
|---|---|---|
| Business stakeholders, consumers | What they are getting, when, where | Ticket detail, team composition, internal sequencing |
| Business analysts | Where their dataset stands, what is pending and from whom | Other products' status |
| Leadership | Progress against commitments, dependencies, risk | Ticket counts, velocity |

Where the same underlying data serves two audiences, produce two artifacts rather than one artifact with sections to skip. A reader given a document containing content aimed at someone else reads the whole thing as not for them.

## Core principles

**Ask, never invent.** Dates, metrics, names, deliverable content. An invented specific is worse than a stated gap.

**Link, never copy.** Resolve canonical pages live via `references/confluence-map.md`. Never hard-code a URL.

**Publish in storage format.** Confluence storage format per `references/confluence-formatting.md`, using the appropriate macros. Never post raw Markdown. Checklists are real task lists with assignees.

**One source, two views.** Never maintain a published and an internal version separately.

**No forecast without basis.** A date, a percentage, or a confidence statement is reported from something. If there is no basis, say what is known and what is not.

## References

- `sprint-plan.md` — one page: goal, three themes, delivering by OKR, dependencies
- `release-plan.md` — deliverable-level, grouped by product. Internal/published split defined here
- `incident-report.md` — the incident template, with eight controls on writing from transcripts and chats
- `quarterly-plan.md` — *to be written*
- `status-report.md` — *to be written*
- `grooming-agenda.md` — stakeholder-facing agenda. *To be written.* Note: the developer-facing technical pre-read belongs to `delivery-readiness`
- `confluence-formatting.md` — storage format and macros
- `confluence-map.md` — live link resolution and fallback

## Procedure

1. Identify the artifact and its audience.
2. Read the matching reference in full.
3. Collect required inputs. Ask for anything missing.
4. Resolve Confluence links live via `confluence-map.md`.
5. Write the artifact at outcome level.
6. Before publishing, check every line against the test: does the reader need this to know what they are getting? Remove anything that fails.
7. Convert to storage format per `confluence-formatting.md`.

## The line this skill holds

These artifacts are read by people who were not in the room, sometimes long after, and sometimes in a context where accountability is being determined.

**State facts about the work.** Never characterize a team, a person, or a decision. Never explain a delay by attributing it. Never add reassurance about what will not recur.

A record that states what happened holds indefinitely. A record that argues does not.
