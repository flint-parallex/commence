# Status Meeting (`status`, alias `standard`)

Use for status-style meetings: a round of updates on standing workstreams, such as syncs, stand-ups, steering check-ins, and recurring status. For meetings held to explore or decide, use `references/discussion-meeting.md` instead.

All rules in SKILL.md apply, including Steps 1–4 (purpose, meeting map, outcomes, and curating for impact). This file adds the template, the section notes, and one optional input. Only product decisions (SKILL.md Step 6) get an ID and go to the decision log (`references/decision-log.md`). Ordinary agreements stay in the body of the notes. Follow the layering and formatting rules in SKILL.md (Structure): one point per line, a bold label never carries prose, and fragments instead of sentences. Free-text cells and bullets in this template follow the same rules.

---

## Additional input

- **Series structure** — an ordered list of standing workstreams or layers for the series. Optional. When given, Topics Discussed follows this order (see Section notes).

---

## Template (use these headers verbatim, in this order)

```markdown
# [Meeting Name] — [YYYY-MM-DD]

Status as of [YYYY-MM-DD] · Current decisions: [decision log location]
**About:** Discussed [A], [B], and [C].
**Bottom line:** [One sentence, in business terms: the most important change since last meeting / "No material change."]
**Relevant to:** [Workstreams, systems, or teams affected by what was decided or discussed]
**Attendees:** [Name] ([Role]) · [Name] ([Role])

## Key Takeaways
- [Up to 3 bullets: what changed, or what is now true, because of this meeting]

## Decisions
- **[What was decided]**: [reason or context, one clause] ([D-ID if a product decision])

## Open Questions
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
### [N]. [Topic] — [Outcome tag]
**Takeaway:** [Where it stands, one line]

- **Update**
  - [One fact per line] ([Name])
- **Outcome**
  - [D-ID / Agreed: … / Open question / No outcome reached]
```

---

## Section notes

- **About** — one sentence, two or three topics, written so an executive outside the project understands it.
- **Relevant to** — the workstreams, systems, or teams whose work is affected by a decision, action, or open question from this meeting. This is the line a non-attendee scans to decide whether to keep reading. Use the names those teams and systems already go by. If nothing beyond the attendees is affected, write "Attendees only."
- **Bottom line** — use the impact framing from SKILL.md Step 4. In a status meeting, the most important change is often a date, a risk, or a dependency that moved.
- **Key Takeaways** — at most three bullets, Lead items only. These are deltas, not a recap. If nothing shifted, write "None this meeting."
- **Action Items** — list only items created in this meeting. Earlier items go in Carried Over.
- **Requests & Dependencies** — any ask from one person or party to another for information, access, a deliverable, or a review. These are kept separate from Action Items so a reader can see at a glance who is waiting on whom.
- **Carried Over** — include this section only when Previous Meeting Notes were supplied. Otherwise leave it out completely.
- **Topics Discussed** — numbered headings with outcome tags, then a **Takeaway:** line and labeled groups of one-line fragments (see discussion-meeting.md for a worked example). Open with where things stood coming into the meeting.
  - When a series structure is supplied, list its workstreams in order.
  - Under any workstream that didn't come up, write "Not discussed this meeting."
  - Slot any new topic into its logical position.

---

## Final checks (standard)

No checks beyond those in SKILL.md.
