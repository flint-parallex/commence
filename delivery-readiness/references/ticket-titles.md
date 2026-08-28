# Ticket Titles

Applies to every title this skill writes — epics, stories, spikes, tasks, and the Title
column of a proposed work set.

## The rule

**The title identifies. The story statement explains.**

A title is a label, not a summary. Its job is to let a reader find this ticket in a backlog
of forty rows, on a board column roughly forty characters wide, in a sprint report, and in
a Slack paste. Nothing else.

The outcome discipline is not relaxed — it moves. "As a *[consumer]*, I need *[outcome]* so
that *[business value]*" sits directly beneath the title and is where a reader learns what
the work is for. A title that restates it has said the same thing twice and made the
backlog harder to scan for the trouble.

If we are building a private assets dataset, the title is `Private assets dataset`.

## Constraints

| | |
|---|---|
| **Length** | Six words. Fifty-five characters. Both are ceilings, not targets |
| **Shape** | `<the thing>` for an epic. `<the thing> — <what happens to it>` for a story, where the thing alone would be ambiguous |
| **Uniqueness** | No two tickets under one epic share a title. If two would, the distinguishing word is the one that belongs in both |
| **Spikes** | Keep the prefix: `Spike - <the question being answered>` |

## The test

Read the title alone, with no epic and no description. Two questions:

1. **Which thing is this about?** A title that could belong to any ticket in the initiative has not identified anything.
2. **Could I find it again?** If the only way to locate this ticket is to open three and read their descriptions, the title failed.

## What the activity ban actually bans

The prohibition is on **implementation verbs**, not on verbs.

*Configure Datadog monitors*, *refactor the loader*, *wire up the DAG*, *set up Airflow* —
these name the engineer's activity and belong nowhere in a ticket title, for the same
reason implementation belongs nowhere in a specification.

*Ingest*, *load*, *monitor*, *reconcile* — these name what the product does. They are fine,
and they are usually the shortest true title available.

The instruction to write outcome-framed titles was aimed at the first list. It overshot
into full sentences, and that is what produced titles nobody can scan.

## Do not write

| Construction | Why |
|---|---|
| `available to <consumer>` | The consumer is a field. It does not belong in the label |
| `so that <value>` | That is the story statement's third clause |
| `As a <role>, ...` | The title is not the story statement |
| `capability`, `enablement`, `solution`, `framework`, `end-to-end` | Words that survive being deleted. Delete them |
| `data product` where `dataset` or the product name works | Category noun standing in for the thing |
| A restated requirement | The requirement is linked. Do not summarise it in the title |

## Before and after

| Written | Should be |
|---|---|
| Private markets Form D data product available to Data Strategy consumers | `Form D dataset` |
| Enable ingestion capability for Form D filings from EDGAR source system | `Ingest Form D filings from EDGAR` |
| Establish bitemporal history tracking capability for private asset holdings | `Bitemporal history for holdings` |
| As a Data Strategy consumer, confidence that the dataset is complete and current | `Form D completeness monitoring` |
| Configure Datadog monitors for freshness and volume on the Form D tables | `Form D freshness and volume monitors` |
| Implement end-to-end reconciliation framework between source and target | `Source-to-target reconciliation` |

The right-hand column loses nothing a reader needs, because every deleted fact is a field
on the ticket: the consumer, the epic, the outcome, the OKR, the specification link.

## In decomposition

The Title column of a proposed work set is where these titles are first written, and it is
the point at which a long title is cheapest to fix. The work set is read as a table — a row
whose Title wraps to three lines makes the whole set unreadable and defeats the purpose of
proposing it for review.

Same ceiling. Same test. A title that will not fit the column is a signal the unit may not
be cleanly cut.
