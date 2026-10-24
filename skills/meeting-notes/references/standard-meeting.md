# Standard Meeting

The default template. Use it for any team, working, or cross-functional meeting that doesn't have its own reference.

All rules in SKILL.md apply. This file adds the template, the section notes, and one optional input.

---

## Additional input

- **Series structure** — an ordered list of standing workstreams or layers for the series. Optional. When given, Topics Discussed follows this order (see Section notes).

---

## Template (use these headers verbatim, in this order)

```markdown
# [Meeting Name] — [YYYY-MM-DD]

**About:** Discussed [A], [B], and [C].
**Relevant to:** [Workstreams, systems, or teams affected by what was decided or discussed]
**Attendees:** [Name], [Name], [Name]

## Key Takeaways
- [Up to 3 bullets: what changed, or what is now true, because of this meeting]

## Decisions Made
| Decision | Decided by | Context |
|---|---|---|
| [What was decided] | [Name(s)] | [One line: why or what it changes] |

## Decisions Not Made
| Open question | Decision owner | Needed to decide | Target date |
|---|---|---|---|
| [The open call] | [Name / Owner: not assigned] | [Input, data, or approval required] | [YYYY-MM-DD / Not set] |

## Action Items
| Owner | Action | Due | Status |
|---|---|---|---|
| [Name / not assigned] | [Verb-first action] | [YYYY-MM-DD / No date set] | [New / In progress / Done] |

## Requests & Dependencies
| Requested from | Requested by | What | Needed for | Due | Status |
|---|---|---|---|---|---|
| [Name] | [Name] | [The deliverable or information] | [What it unblocks] | [YYYY-MM-DD / No date set] | [Open / Provided YYYY-MM-DD] |

## Blockers
| Impediment | Unblock owner | Needs |
|---|---|---|
| [What is blocked and by what] | [Name / not assigned] | [What clears it] |

## Carried Over from Previous Meeting
| Item | Owner | Open since | Meetings open | Status this meeting |
|---|---|---|---|---|
| [Item] | [Name] | [YYYY-MM-DD] | [N] | [Done YYYY-MM-DD / In progress / Not discussed / Reassigned to Name / Closed — decided by Name] |

## Topics Discussed
**[Topic]** — [Starting point]. [What was discussed]. Outcome: [result, or "No outcome reached"].
```

---

## Section notes

- **About** — one sentence, two or three topics, written so an executive outside the project understands it.
- **Relevant to** — the workstreams, systems, or teams whose work is affected by a decision, action, or open question from this meeting. This is the line a non-attendee scans to decide whether to keep reading. Use the names those teams and systems already go by. If nothing beyond the attendees is affected, write "Attendees only."
- **Key Takeaways** — at most three bullets. These are deltas, not a recap. If nothing shifted, write "None this meeting."
- **Action Items** — list only items created in this meeting. Earlier items go in Carried Over.
- **Requests & Dependencies** — any ask from one person or party to another for information, access, a deliverable, or a review. These are kept separate from Action Items so a reader can see at a glance who is waiting on whom.
- **Carried Over** — include this section only when Previous Meeting Notes were supplied. Otherwise leave it out completely.
- **Topics Discussed** — two lines at most per topic. Each topic opens with where things stood coming into the meeting (one clause), then what was discussed and where it landed.
  - When a series structure is supplied, list its workstreams in order.
  - Under any workstream that didn't come up, write "Not discussed this meeting."
  - Slot any new topic into its logical position.

---

## Final checks (standard)

No checks beyond those in SKILL.md.
