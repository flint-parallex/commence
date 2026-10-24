---
name: meeting-notes
description: Turn a team meeting transcript into Confluence-ready meeting notes that every attendee, external party, and executive can read — including people who did not attend. Use whenever a meeting transcript, recording transcript, or Zoom/Teams summary needs to become meeting notes, a recap, a decision log, or action items.
---

# Meeting Notes

One transcript in. One set of meeting notes out, ready to post to Confluence.

The notes are read by every attendee, by external parties, and by executives opening the page cold. They are written for someone who was not in the room. A reader who knows the project generally, but missed this meeting, must be able to decide from the top lines whether it matters to them, and then understand every entry without the transcript.

The notes are a plain, fair record of what was decided, what was not, and who owes what to whom.

---

## Inputs

1. **Transcript** — required.
2. **Meeting context** — name, date, attendees. Optional. If missing, take them from the transcript. If they can't be found there, write "Not stated."
3. **Previous Meeting Notes** — the last notes from the same meeting series. Optional. Supplying them turns on the Carried Over section.
4. **Series structure** — an ordered list of standing workstreams or layers for the series. Optional. When given, Topics Discussed follows this order (see Section notes).

---

## Delivery

Return the notes as one file: `meeting-notes_YYYY-MM-DD_<meeting-slug>.md`. If a file can't be written, return the notes as Markdown.

No preamble. No commentary on the meeting or on these instructions. Only the notes.

---

## Core rules

1. **Never invent.** If something wasn't said, it isn't written. Any unknown field reads "Not stated."
2. **The transcript is the source of truth.** Where a Zoom or Teams summary disagrees with the transcript, the transcript wins.
3. **One spelling per person.** Use names exactly as they are spoken or listed. Pick one spelling for each person and use it everywhere in the notes.
4. **Absolute dates only.** Convert relative dates using the meeting date. "Next Friday" becomes "2026-10-30." If no date was given, write "No date set."
5. **What counts as a commitment.** An item counts only if a person accepted it or was assigned it on the record.
   - A suggestion that nobody took on is still listed, with "Owner: not assigned."
   - It is never dropped.
6. **Decisions Made vs. Decisions Not Made.**
   - Reached on the record → Made.
   - Discussed but not resolved → Not Made.
   - Never blur the two.
7. **Every item has an owner field.**
   - Owners are named people, never teams.
   - If the meeting named only a team, write the team name with "(no individual named)."

---

## Neutrality rules

The notes work because they read as a standard, boring, fair record. The structure carries the accountability. Dates, owners, and counts say what needs saying. The wording never does.

- **Same format for everyone.** The operator's own items appear in exactly the same format as everyone else's.
- **Attribution verbs only:**
  - committed to, stated, asked, confirmed, agreed, requested, proposed, raised, reported.
- **Banned:**
  - failed, again, still hasn't, yet to, slipped, missed, behind, unfortunately, dropped the ball, pushed back, reluctant, deflected, avoided, concerning, disappointing.
  - Any adjective or adverb describing a person or how they acted.
- **No characterization:**
  - No tone, posture, or mood.
  - No guessing at intent.
  - No remarks about who spoke a lot or a little.
- **No meta-purpose.** Never describe the notes as accountability, tracking, auditing, or follow-up enforcement. They are meeting notes.
- **No commentary sections:**
  - No "internal only" block.
  - No issues or opportunities commentary.
- **Plain language.**
  - The About line uses no jargon at all.
  - Elsewhere, technical terms are fine where attendees used them.
- **Empty sections stay in.** Write "None this meeting." Every meeting in a series has the same structure.

**Neutral vs. banned, in practice:**

| Banned (editorial) | Correct (ledger) |
|---|---|
| Dev team still hasn't delivered the schema, again. | Schema spec · Owner: Priya Shah · Open since 2026-09-12 · Meetings open: 4 · Not discussed |
| Mark pushed back on the timeline. | Mark Lee stated the 2026-11-06 date is not achievable. New date: Not set. |
| Nobody wanted to own the data mapping. | Data mapping · Owner: not assigned · Due: No date set |

---

## Written for someone who wasn't there

Every entry must make sense to a reader who missed the meeting and has only general knowledge of the project.

- **Self-contained entries.** Each row or bullet stands on its own. Name the actual thing — the system, dataset, document, or deliverable — every time. Never write "it," "that," "the issue," "the thing from last week," or "as discussed."
- **Say what it is, not just what it's called.** On first mention, give a short plain-language gloss for any internal label, ticket number, or code name: "PM-412 (the Form D matching rules)." Spell out acronyms on first use unless they are standard across the organization.
- **Decisions carry their why.** The Context column states what the decision changes or unblocks in one line, so a reader can see its consequence without having heard the debate.
- **Open questions carry their stakes.** "Needed to decide" says what input is missing, and the row makes clear what waits on the answer.
- **Topics give the starting point, then the outcome.** Each topic opens with where things stood coming into the meeting (one clause), then what was discussed and where it landed.
- **No insider shorthand.** No nicknames for workstreams, no references to side conversations, no jokes or callbacks that only attendees would follow.
- **The top answers "do I need to read this?"** The About line, Relevant to line, and Key Takeaways together tell a non-attendee within ten seconds whether the meeting touched their work.

| Only an attendee follows it | Anyone can follow it |
|---|---|
| Agreed to go with option 2. | Agreed to source issuer names from the SEC Form D filings instead of the vendor feed. Context: removes the vendor contract dependency for the November release. |
| Sam will look into it. | Sam Ortiz committed to confirming whether the matching service can accept batch uploads above 10,000 records. Due 2026-10-31. |

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

### Section notes

- **About** — one sentence, two or three topics, written so an executive outside the project understands it.
- **Relevant to** — the workstreams, systems, or teams whose work is affected by a decision, action, or open question from this meeting. This is the line a non-attendee scans to decide whether to keep reading. Use the names those teams and systems already go by. If nothing beyond the attendees is affected, write "Attendees only."
- **Key Takeaways** — at most three bullets. These are deltas, not a recap. If nothing shifted, write "None this meeting."
- **Action Items** — list only items created in this meeting. Earlier items go in Carried Over.
- **Requests & Dependencies** — any ask from one person or party to another for information, access, a deliverable, or a review. These are kept separate from Action Items so a reader can see at a glance who is waiting on whom.
- **Carried Over** — include this section only when Previous Meeting Notes were supplied. Otherwise leave it out completely.
- **Topics Discussed** — two lines at most per topic.
  - When a series structure is supplied, list its workstreams in order.
  - Under any workstream that didn't come up, write "Not discussed this meeting."
  - Slot any new topic into its logical position.

---

## Carry-over logic

Run this only when Previous Meeting Notes are supplied.

1. **Collect every open item** from the previous notes. An item is open if its status is not Done, Provided, or Closed. Collect from:
   - Action Items
   - Requests & Dependencies
   - Blockers
   - Decisions Not Made
   - Carried Over
2. **Open since.**
   - If the item was already in the previous Carried Over table, keep the date shown there.
   - Otherwise, use the previous meeting's date.
3. **Meetings open.** Count the meetings the item has been open, including this one.
   - An item first raised last meeting and still open now shows 2.
   - An item carried with count N last time shows N+1.
4. **Status this meeting:**
   - **Done YYYY-MM-DD** — completion was confirmed on the record.
   - **In progress** — it was discussed and progress was reported, but it isn't complete.
   - **Not discussed** — it wasn't mentioned. It stays open. Never drop it.
   - **Reassigned to [Name]** — the owner changed on the record.
   - **Closed — decided by [Name]** — someone explicitly removed it or made it moot on the record.
5. **What happens next time.**
   - Items marked Done or Closed this meeting appear here once.
   - They are not carried into the next meeting.

---

## Final check before returning

1. Do the notes contain any banned word or any characterization of a person? Remove it.
2. Does every Action Item, Request, Blocker, and Decision Not Made have an owner field and a date field filled in, including "not assigned" or "No date set" where those apply?
3. Are all dates absolute?
4. Did any statement come from somewhere other than the transcript or the supplied notes? Remove it.
5. Is every section present, with "None this meeting" where it's empty? Is Carried Over left out when no previous notes were supplied?
6. Read the notes as someone who missed the meeting. Does every entry name its subject, explain any label or acronym on first use, and make sense without the transcript? Does the top of the page tell that reader whether this meeting touched their work?
7. Are the operator's own items in exactly the same format as everyone else's?
