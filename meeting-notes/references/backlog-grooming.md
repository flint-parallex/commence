# Backlog Grooming

Use for backlog grooming or refinement sessions: the meeting walks through tickets or stories, discusses acceptance criteria, estimates, or readiness.

Grooming notes answer two questions for anyone who opens them: what state is each ticket in now, and what has to happen before the next grooming session so the work moves forward. The Before Next Grooming section is the heart of the page.

All rules in SKILL.md apply. This file adds the template, the section notes, one optional input, carry-over additions, and its own final checks. Steps 1–4 of SKILL.md (purpose, meeting map, outcomes, and curating for impact) run here too. Only product decisions (SKILL.md Step 6) get an ID and go to the decision log (`references/decision-log.md`). Ordinary agreements stay in the body of the notes. Follow the layering and formatting rules in SKILL.md (Structure): one point per line, a bold label never carries prose, and fragments instead of sentences. Free-text cells and bullets in this template follow the same rules.

---

## Additional input

- **Grooming agenda** — the list of tickets planned for the session. Optional. Supplying it turns on the Not Reached section.

---

## Template (use these headers verbatim, in this order)

```markdown
# Backlog Grooming — [Team or Product] — [YYYY-MM-DD]

Status as of [YYYY-MM-DD] · Current decisions: [decision log location]
**About:** Groomed [N] items: [N] ready, [N] need refinement, [N] split, [N] deferred. Focus: [plain-language theme, e.g., "issuer matching and data quality checks"].
**Relevant to:** [Workstreams, systems, or teams affected]
**Attendees:** [Name], [Name], [Name]
**Next grooming:** [YYYY-MM-DD / Not set]

## Key Takeaways
- [Up to 3 bullets: what changed in scope, priority, or readiness]

## Items Groomed
| Ticket | What it is | Outcome | Estimate | What changed |
|---|---|---|---|---|
| [ID] | [Plain-language one-line description] | [Ready / Needs refinement / Split / Deferred / Removed] | [Points or size / Not estimated] | [Acceptance criteria added, scope cut, split into X and Y, etc.] |

## Before Next Grooming
| Owner | What to do or bring | For ticket | Why it's needed | Due | Status |
|---|---|---|---|---|---|
| [Name / not assigned] | [Verb-first: clarify, confirm, provide, spike, size] | [ID — short name] | [What readiness gap it closes] | [YYYY-MM-DD] | [New] |

## Decisions
- **[What was decided]**: [reason or context, one clause] ([D-ID if a product decision])

## Open Questions
| Open question | Decision owner | Needed to decide | Target date |
|---|---|---|---|
| [The open call] | [Name / Owner: not assigned] | [Input required] | [YYYY-MM-DD / Not set] |

## Requests & Dependencies
| Requested from | Requested by | What | Needed for | Due | Status |
|---|---|---|---|---|---|
| [Name] | [Name] | [Deliverable or information] | [Ticket or outcome it unblocks] | [YYYY-MM-DD / No date set] | [Open / Provided YYYY-MM-DD] |

## Blockers
| Impediment | Unblock owner | Needs |
|---|---|---|
| [What is blocked and by what] | [Name / not assigned] | [What clears it] |

## Other Action Items
| Owner | Action | Due | Status |
|---|---|---|---|
| [Name / not assigned] | [Actions not tied to readying a ticket] | [YYYY-MM-DD / No date set] | [New] |

## Not Reached
| Ticket | What it is | Moves to |
|---|---|---|
| [ID] | [One line] | [Next grooming / Not set] |

## Carried Over from Previous Grooming
| Item | Owner | Open since | Meetings open | Status this meeting |
|---|---|---|---|---|
| [Prep item or ticket] | [Name] | [YYYY-MM-DD] | [N] | [Done YYYY-MM-DD / Ticket now Ready / In progress / Not discussed / Reassigned to Name / Closed — decided by Name] |
```

---

## Section notes

- **About** — the readiness counts are the at-a-glance summary. Count only items actually discussed. The Focus clause names the theme in plain language, not ticket IDs.
- **Relevant to** — as in SKILL.md: the teams or systems affected. If nothing beyond the attendees is affected, write "Attendees only."
- **Next grooming** — use the date if it was said or is known from the meeting context. Otherwise write "Not set."
- **Items Groomed** — one row per ticket discussed, in the order discussed.
  - **What it is** — a plain description a non-attendee understands. Never just the ticket title if the title is jargon.
  - **Outcome** — use only the five values listed.
    - **Ready:** the team agreed it can be pulled into a sprint.
    - **Needs refinement:** discussed, but something must happen first. Every Needs refinement ticket must have at least one row in Before Next Grooming. If the meeting named no next step, add a row with "Owner: not assigned" and "What to do or bring: Next step not identified."
    - **Split:** list the new tickets in What changed, with IDs if they were given.
    - **Deferred** or **Removed:** say who decided it in What changed. If removing or deferring it changes product scope, also add it to Decisions, with an ID, as a No-go (Removed) or a Defer (Deferred, with a date or trigger). Routine re-sequencing stays in Items Groomed.
  - **Estimate** — record only estimates stated on the record. Never infer one.
- **Before Next Grooming** — every task someone must complete, or every input someone must bring, for a ticket to move toward Ready. This includes clarifying acceptance criteria, confirming a data source, getting a sample file, running a spike, getting a design, or getting a stakeholder answer.
  - **Due** — defaults to the day before the next grooming date when no other date was stated. If there's no next grooming date either, write "No date set."
  - **Why it's needed** — names the gap in plain language, e.g., "acceptance criteria don't say how duplicate issuers are handled."
- **Other Action Items** — anything committed in the session that isn't about readying a ticket.
- **Not Reached** — include this only when a grooming agenda was supplied. List tickets on the agenda that weren't discussed. Otherwise leave the section out.
- **Carried Over from Previous Grooming** — include this only when Previous Meeting Notes were supplied.

---

## Carry-over additions

Run SKILL.md's carry-over logic, plus:

- **Also collect from the previous notes:**
  - Every Before Next Grooming row that isn't Done.
  - Every Items Groomed ticket whose outcome was Needs refinement.
  - Every Not Reached ticket.
- **Ticket status.** A ticket that was groomed again this meeting gets the status "Ticket now [new outcome]" in Carried Over. It also gets a row in Items Groomed.
- **Prep item status.** A Before Next Grooming item reported done on the record gets "Done YYYY-MM-DD." One not mentioned stays "Not discussed."

---

## Final checks (grooming)

1. Does every Needs refinement ticket have at least one Before Next Grooming row?
2. Do the About counts match the Items Groomed table?
3. Is every estimate one that was stated on the record?
