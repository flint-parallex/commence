---
name: meeting-notes
description: Turn a meeting transcript — ideally with its agenda, talking points, or pre-read — into curated, business-focused meeting notes with a decision record that anyone can read cold, including executives and people who did not attend. Reconstructs the meeting's structure (purpose, key topics, talking points, outcomes, owners), surfaces what matters to the product and the business, and routes to the right shape (discussion or decision meeting, status or working meeting, backlog grooming, one-on-one). Use whenever a transcript, recording, or Zoom/Teams summary needs to become meeting notes, a recap email, a decision record or log, action items, or a next-meeting agenda — even when the request is casual ("write up this call", "what did we decide", "notes from the sync").
---

# Meeting Notes

A transcript in, plus whatever planning material came with the meeting. Out comes a curated record of what the meeting was for, what it settled, why that matters to the product, and who owes what.

These notes are not a transcript summary. A summary repeats the conversation in order. These notes rebuild the meeting's logic. A meeting is called to solve or discuss something. That purpose breaks down into key topics, and each topic into talking points. The talking points produce outcomes, and the outcomes have owners. Your job is to recover that structure, judge what in it matters, and lead with that.

The notes are read by attendees, by external parties, and by executives opening the page cold. Write them for someone who was not in the room.

The skill works two ways. With an agenda, key topics, or talking points, it maps the conversation onto the plan. Without them, it reconstructs the plan from the conversation. Even partial context (a purpose line, a topic list, an invite title) makes the notes noticeably sharper, so use every anchor you have.

This file holds the method and the rules every meeting type shares. The template for each type lives in its own reference file. Load the one that matches the meeting, and only that one.

Every type also loads:
- `references/reading-the-meeting.md` — how to interpret the conversation
- `references/decision-log.md` — which decisions reach the log, and the log table
- `references/impact-framing.md`, together with `delivery-communication` — how the outward-facing lines read

Where `AGENTS.md`, `REPO-STRUCTURE.md`, or `delivery-communication` owns a rule, this skill points to it rather than restating it. If one of them appears to conflict with this file, the file that owns the rule wins.

---

## The meeting hierarchy

Every meeting has this structure, whether or not anyone wrote it down:

| Level | What it is | Where you find it |
|---|---|---|
| **Purpose** | The problem to solve, the question to answer, or the thing to align on. The reason the meeting exists. | Agenda title, invite, pre-read, the organizer's opening framing |
| **Key topics** | The two to six parts the purpose breaks into | Agenda items; otherwise, clusters of discussion that serve one sub-question |
| **Talking points** | What has to be covered within a topic: options, data, concerns, proposals | Agenda sub-bullets or talking points; otherwise, the distinct threads within a topic |
| **Outcomes** | What each topic produced (see Outcome types) | The transcript |
| **Owners** | The named person accountable for each outcome that needs one | The transcript |

The notes are organized by this hierarchy, not by the order things were said.

---

## Inputs

1. **Transcript** — required.
2. **Agenda, talking points, or pre-read** — optional, and the most valuable input after the transcript. When supplied, it is the spine of the notes (Step 2). Ask for it once if the meeting obviously had one and it wasn't attached. Don't block on it.
3. **Meeting context** — name, date, attendees, and their roles. Optional. If missing, take them from the transcript. Always keep roles when they're known, because a reader months later may not know who anyone is. If they can't be found there, write "Not stated."
4. **Previous meeting notes** — the last notes from the same series. Optional. Supplying them turns on the Carried Over section.
5. **Prior decisions** — the current decision log, or earlier notes for this product. Optional. Supplying them turns on conflict checks (see `references/decision-log.md`).
6. **Product and OKRs** — the product the meeting concerns and the team's current OKRs. Optional. They fill the log's Product and OKR columns. Never guess an OKR. If no OKR list was supplied, or no OKR clearly matches, write "Not stated."
7. **Meeting type** — optional. See Step 5.
8. **Operator** — the person posting the notes. Optional. Hand-off Next Steps default to them. If not given, use the product owner if one attended, and say so in the brief.
9. **Audience** — who the notes are mainly for (the team, a partner, leadership). Optional. It sets the impact framing (Step 4). Default: a mixed audience that includes leadership. "Didn't attend" or "overview" turns on the Overview (Step 7).

A reference may add inputs of its own.

---

## Step 1 — Find the purpose

Before anything else, write the meeting's purpose as a single question or goal, e.g., "Decide whether to source issuer names from SEC filings for the November release."

- With an agenda: take it from the agenda title and framing, then sharpen it from the organizer's opening remarks.
- Without one: take it from the opening minutes, the meeting name, and what the discussion kept coming back to.
- Judge the result: **Answered**, **Partly answered**, or **Not answered**. In the discussion template, the purpose becomes the bold first line of the Quick Recap. The judgment goes in the operator brief, and it shows on the page through what lands in Decisions versus Still Open. The status template keeps its Bottom line.
- A recurring status meeting can have a standing purpose ("Weekly delivery status for the Issuer Data product"). That's fine.

## Step 2 — Build the meeting map

**Load `references/reading-the-meeting.md` first.** It covers ranking your context anchors, the three context levels (full agenda, partial context, transcript only), the language cues for outcomes, and how to weigh what was said.

Build the hierarchy for this meeting as a working step. It is not output on its own. It becomes the structure of the notes.

**When an agenda or talking points were supplied, the agenda is the spine:**
1. Each agenda item becomes a key topic. Its sub-bullets become the talking points.
2. Map the transcript onto it. Mark each item **Covered**, **Partly covered**, or **Not reached**.
   - **Covered:** its talking points were discussed and reached an outcome of any type, including Open question. Being covered doesn't mean being resolved.
   - **Partly covered:** some of its talking points weren't discussed.
   - **Not reached:** it wasn't discussed.
   - If the agenda lists topics without talking points, judge each item by its own question instead. It's **Covered** if that question reached an outcome. It's **Partly covered** if it came up but its question wasn't addressed directly, for example only as a side effect of another topic.
3. Discussion that fits no agenda item becomes a topic under **Raised in the meeting**. If it was significant, it may rank above agenda items in layer 2.
4. Where the discussion answered a different question than the agenda asked, record what was actually answered. Don't force it to fit the agenda.

**When context is partial** (topics without talking points, a purpose line, an invite, or previous notes), anchor what you can, then infer the rest from the transcript. See reading-the-meeting §2b.

**When there is no agenda, infer the map:**
1. Group the transcript into two to six key topics, each built around one sub-question of the purpose. Group by subject, not by time: a topic revisited three times is one topic.
2. Within each topic, list the distinct talking points: options weighed, data shared, concerns raised, proposals made.
3. Order topics by importance to the purpose, not by when they came up.

## Step 3 — Classify the outcomes

Every talking point ends in one of these outcomes. Use these terms.

| Outcome | Test | Needs |
|---|---|---|
| **Decision** | A choice was reached on the record by someone with the standing to make it | Decided by. Logged only if it passes the product-decision test (Step 6). |
| **Deferred** | Someone explicitly postponed the question to a stated date or condition, and no one owes an input before then ("revisit in Q1," "after the pilot") | Decided by and the trigger. Logged as a Defer if it passes the test. |
| **No further action** | Someone explicitly chose not to pursue it, or ruled it not valuable. This is a real decision, often a good product call. | Decided by and the reason. Logged as a No-go if it passes the test. |
| **Recommendation** | A course of action was proposed and endorsed in the room, but the call belongs to someone else or needs approval | Proposed by; decision needed from |
| **Open question** | Discussed, not resolved, and someone needs to resolve it | Decision owner; what's needed to decide |
| **Next step** | Someone accepted or was assigned work | Owner; due date |
| **Idea** | Shared but not adopted, rejected, or assigned | Raised by |
| **Information** | An update, data point, or context that shaped the discussion and needs no action | Reported by |

- **"Park" has three readings, decided by what it waits on:**
  - **Park until a person supplies an input** ("until Sam confirms") → Open question.
  - **Park until another decision is made** ("until the key standard is decided") → Defer, with Revisit if: that decision's ID. If the decision it waits on is itself still open, cite the Open row: "Revisit if: key standard decided (open: D-…)".
  - **Park indefinitely, or not worth doing** → No further action.
- **"Not now", "right now", "at this point"** state no time limit. The No-go's Revisit if is "Not stated", with no anchored date. Don't turn the phrase into a condition.
- **"Not until [someone else] does X"**, where nobody in the meeting owes X → a No-go with Revisit if: X. Nobody owes an input, so it isn't an Open question.
- **A decision whose execution needs someone's approval** ("Let's do the pilot; I'll confirm capacity with Sofia") → two outcomes: the decision itself (Go), and an Open row for the approval, with the approver under Review with. Its Covers line or Outcome bullet says the work isn't committed until the approval.
- **A time-limited drop is a No-go, not a Defer.** For example, "Drop it for this quarter": the time limit becomes its Revisit if ("Next quarter, as stated"). Defer is for a question postponed with no answer given. A No-go gives one.
- A talking point can produce more than one outcome, e.g., a decision and the next steps that carry it out.
- Never upgrade an outcome. Nodding along is not a decision. "We should probably" is not a commitment. When in doubt, step down: Decision → Recommendation → Open question.
- Never drop an outcome type that needs an owner. Commitments, decisions, recommendations, and open questions always appear, even if minor.

## Step 4 — Curate, then frame for impact

This step is what separates these notes from a summary. Rank what the map holds, then say why the top items matter.

**Rank each topic and outcome:**
- **Lead** — it changes what someone builds, ships, spends, or commits to, or it affects a customer, a date, a cost, a risk, or scope. Decisions and No-gos usually lead. Lead items go in layer 2: Decisions and Still Open for discussion meetings, Key Takeaways in the other templates. Lead problems and questions also shape the Quick Recap.
- **Support** — it explains or qualifies a Lead item. It goes in the topic body.
- **Leave out** — small talk, logistics, restatements, tangents that produced no outcome, and the back-and-forth that led up to a decision once the decision and its reason are recorded.

**Frame Lead items in business and product terms.** For each Lead topic and each decision, write a "Why it matters" line. Answer one question: what does this change for the product, the customer, or the business? Use these lenses, and pick the one that fits:

- **Customer or user** — who gets what, and when
- **Delivery** — a date, a release, a sequence, a dependency unblocked or created
- **Scope** — what's in, what's out, what got simpler
- **Cost or effort** — spend, headcount, a contract, a build-versus-buy choice
- **Risk** — what is now less likely, or newly possible, to go wrong
- **Strategy** — how it serves or shifts a stated goal or priority

**Grounding rule for impact.** Impact lines restate impact that was said in the meeting, or the direct consequence of what was decided ("removes the vendor contract dependency"). They never invent a number, a market claim, a customer reaction, or a business case nobody made. If a Lead decision has no stated or direct impact, write what it changes in plain terms. Don't write "Impact: not stated" on Lead items. A decision whose consequence can't be stated at all probably isn't Lead.

**Voice comes from `delivery-communication`.** Load `delivery-communication/SKILL.md` (a sibling folder in the repository, or the installed skill of that name), and read its Voice, Audience shaping, and The line this skill holds sections. Use its voice for the Quick Recap, the Decisions and Still Open lines, and the Takeaways. Its publishing test ("does the reader need this to know what they are getting?") strips names, assignments, and capacity detail. That test applies only to the Leadership view (Step 7), never to the notes. The notes are the team's record: effort estimates, capacity, and attributed assessments stay in, because they are often the reason for a decision. Then load `references/impact-framing.md`. It covers how that voice applies to a meeting, and it has a short fallback for when `delivery-communication` isn't available.

## Step 5 — Pick the meeting type, load its reference

| Type | Use when | Load |
|---|---|---|
| `discussion` | The meeting existed to explore, weigh options, solve a problem, or decide. Planning, design, strategy, review, working sessions, cross-functional problem-solving. **This is the default.** | `references/discussion-meeting.md` |
| `status` | A round of updates on standing workstreams: syncs, stand-ups, steering check-ins, recurring status. Mostly reporting, little debate. | `references/standard-meeting.md` |
| `grooming` | Backlog grooming or refinement. The meeting walks through tickets or stories, discusses acceptance criteria, estimates, or readiness. | `references/backlog-grooming.md` |
| `one-on-one` | A conversation between the operator and exactly one other person. The reference picks the variant: partner, manager, or team lead. | `references/one-on-one.md` |

- If the input names a type, use it. (`standard` is an alias for `status`.)
- Otherwise, infer it. If a status meeting turned into a real debate on one item, use `discussion` when the debate was most of the meeting. Otherwise, use `status` and give that item its full outcomes.
- If it's unclear, use `discussion`.
- All types run Steps 1–4. The reference supplies the template. Steps 1–4 fill it.

## Step 6 — Product decisions and the decision log

Load `references/decision-log.md`. It applies to every meeting type.

**Most meetings produce next steps, not log-worthy decisions.** Day-to-day agreements stay in the notes, in the topic's Outcome bullet and in Next Steps. They get no ID and no log row.

An outcome goes to the **decision log** only if it passes the **product-decision test**. It must change at least one of these:
1. **Direction:** what the product will or won't pursue, including "not valuable" and "no further action" calls.
2. **Implementation:** what will be built, or how, in a way engineering builds from: scope, source, approach, design.
3. **Unblocking:** a blocker that needs a decision from a named person. This is logged as an **Open** decision, with who it must be reviewed with.
4. **Enhancement:** an adopted improvement to the product.

These don't qualify: process and logistics, meeting cadence, who takes which task, minor sequencing, information, and ideas.

**Hand-offs to other skills.** Some decisions belong to another skill's artifact. Record the decision here, and add a Next Step to hand it off. Its owner is the person the meeting named, or else the operator, since hand-offs are the operator's job in this repository. Never write the other artifact's content yourself.
- **An assertion, a grain or key definition, or any change to what the product means** belongs to the specification. Add the Next Step "Hand off to `specification-authoring`: [the decision]" (`AGENTS.md`: assertions and their IDs live in the specification).
- **A fix an engineer simply carries out** ("Elena will change the merge") needs no hand-off. The Next Step is enough. A hand-off is only for a decision that changes another skill's artifact.
- **New work to build or pilot** belongs to `delivery-readiness`, but only once a specification exists. Tickets derive from a specification (`AGENTS.md`). If none exists, the hand-off is to `specification-authoring` first: "Hand off to `specification-authoring`: define [the pilot or feature] (D-…)".

Product decisions get an ID, shown at the end of their line in the note's **Decisions** or **Still Open** section. The full log rows go to the decision log, not the note (Step 7). The note links to the log for the current status.

## Step 7 — Deliver

1. **The notes**, as one file.
   - **In the repository**, place the file from `REPO-STRUCTURE.md` → Explicit destinations:
     - product-scoped meeting → `products/<product>/decisions/<YYYY-MM-DD>-<slug>.md`
     - team-wide meeting → `docs/meetings/<YYYY-MM-DD>-<slug>.md`
     - If the product can't be told from the inputs, ask. Never infer it from a filename.
   - **Outside the repository**, name it `<YYYY-MM-DD>-<slug>.md`.
   - If a file can't be written, return the notes as Markdown.
   - **The transcript is raw material.** `AGENTS.md` asks for statements whose only source is raw material to be attributed and flagged. In meeting notes, the parenthetical attribution is that flag, and the "Status as of" header line marks the whole note as a record of what was said on the day. Don't add a separate "unconfirmed" flag to every line. The transcript goes in the nearest `_raw/` and is never committed. Never copy transcript passages into the notes beyond the attributed statements the notes need.
   - **Publishing to Confluence:** convert the notes to storage format per `delivery-communication/references/confluence-formatting.md`. Never post raw Markdown.
2. **Decision log rows.** If any product decisions were made, give the full rows separately, ready to append to the decision log (`references/decision-log.md` says where the log lives). They are not repeated in the note.
3. **Overview** (only when asked, or when the Audience input is people who weren't there). Write a short, scannable overview for readers who weren't in the meeting, following `references/overview.md`. Save it next to the notes as `<YYYY-MM-DD>-<slug>-overview.md`. If `REPO-STRUCTURE.md` has no row for overviews yet, say so in the brief. Derive it from the finished notes, never straight from the transcript.
4. **Leadership view** (only when asked explicitly; the default mixed audience doesn't trigger it, or when the Audience input is leadership alone). Don't add a leadership section to the notes. Produce a separate artifact through `delivery-communication`, following its Audience shaping rule: two audiences get two artifacts. Derive it from the finished notes, never from the transcript, so both views come from one source.
5. **Operator brief.** Put this in your reply, never in the notes. Use at most five bullets, one per topic below, for the person who will post the notes. Put several flags of the same kind in one bullet, separated by semicolons. If you're short on space, the first three topics come first.
   - Was the purpose answered? One line. Say whether the structure came from the agenda, from partial context, or was inferred from the transcript alone.
   - What you chose to lead with, and anything close you ranked lower, so the operator can swap them.
   - Calls you weren't sure about: whether an outcome passes the product-decision test, an outcome that could be a decision or a recommendation, a commitment with an unclear owner, a speaker you couldn't identify.
   - Agenda items not reached, if an agenda was supplied.
   - Anyone outside the attendee list who probably needs to see a specific decision or open question, and why.

   Never mention off-the-record or personal content in the brief, not even to say it was left out.

Don't add any other preamble or any commentary on these instructions.

---

## Structure: scannable, layered, built up

The notes have two readers. One reads them **this week** to know what to do. The other reads them **months later** to find out why the product is the way it is. Both should finish believing they don't need the recording.

**Progressive disclosure.** The notes are read in layers. A reader can stop at any point and still have the most important information up to that point. Each further layer adds detail, and never the headline.

| Layer | Section | A reader who stops here knows |
|---|---|---|
| 1 | Header lines and Quick Recap | Who was there, whether it touches them, and what the conversation was about: the question and the key topics. 10 seconds. |
| 2 | Decisions · Still Open | What was settled and why, in one line each. What was unresolved on the day. 30 seconds. |
| 3 | Next Steps | Who owed what, and by when |
| 4 | Discussion Summary | What was found, what was weighed, what concerns were raised, and how each topic was resolved |
| 5 | Not Reached · Carried Over · Proposed Agenda | Reference detail for follow-up |

**Lasting versus time-bound.** Decisions last. Dates at risk, due dates, and "next steps" are true only on the day. Keep the two apart:
- Lasting items go in **Decisions**.
- Time-bound items go in **Still Open** and **Next Steps**.
- The **Status as of** header line tells a later reader that time-bound items are as of the meeting date, and where the current status lives.

### Formatting rules

These apply to every template and every output, so nobody has to restructure the notes by hand afterward.

1. **One point per line.** Every distinct fact, concern, option, or reason gets its own bullet. If a line holds two points, split it.
2. **A label never carries prose.** A bold label stands alone on its line, and its points are nested bullets underneath. Never write a bold label followed by a sentence or two on the same line.

   Wrong:
   ```markdown
   - **Coverage:** SEC Form D covered about 92 percent of issuers on a sample Priya Shah pulled, against about 97 percent for the vendor feed (reported by Sam Ortiz).
   ```

   Right:
   ```markdown
   - **Coverage**
     - SEC Form D: about 92% of issuers on a sample (Sam Ortiz)
     - Vendor feed: about 97%
   ```

   There are two exceptions, both at the top of the page:
   - **Quick Recap key topics:** a bold topic name plus one short fragment, under about 15 words: "**Partner demand:** credit research copies debt schedules by hand".
   - **Decisions and Still Open lines:** the decision itself in bold, a colon, then one reason clause, then the ID at the end: "**Will not build a general HTML table parser**: two engineers for a quarter for one partner's need (D-…)". Here the bold text is the statement itself, not a label.

   Never more than one clause after the bold text. Anything longer goes in the Discussion Summary.
3. **Fragments, not sentences.** Nested bullets are short fragments of about 12 words, with no filler. Write "Vendor feed: about 140K a year", not "Sam Ortiz reported that the vendor feed costs around 140K a year."
4. **Attribution at the end, in parentheses:** "(Sam Ortiz)". Attribute claims and assessments. Agreed facts and outcomes don't need attribution on every line.
5. **Lists stay lists.** A series of items (affected teams, attendees with roles, options weighed) is a bulleted list or a one-item-per-line block, never a run-on comma list over a line long. A short header line is fine as one line ("Relevant to: Issuer Data · Procurement · Ops").
6. **Lead with the takeaway.** Every topic opens with a one-line **Takeaway:** stating where it landed. The detail follows, ordered as Why it matters, What was weighed, Concerns raised, Not adopted, then Outcome. A reader who stops after the takeaway has the point. A reader who keeps going gets the reasoning.
7. **Tables for ledgers, bullets for reasoning.** Anything with an owner and a date goes in a table. Table cells stay under about 15 words.
8. **Bold only labels and takeaway statements.** Never bold for emphasis inside a sentence.
9. **Numbered topic headings carry their outcome:** `### 2. Batch matching capacity — Open`. The headings alone work as a table of contents.
10. **Same order every meeting**, so someone looking back across many meetings finds things in the same place.
11. **Say each fact once.** The detail in a topic supports the takeaway. It doesn't repeat it. A decision's one-line statement lives in Decisions, and its reasoning lives in the Discussion Summary.
12. **IDs go last.** Decision IDs never start a line a person reads. They go at the end, in parentheses, where they serve the log without getting in the way.

**Two tests for each topic:**
- Could someone who wasn't there read it and confidently skip the recording?
- Could someone reading it two months from now see why the decision was made and what would reopen it?

---

## Core rules

1. **Never invent.** If something wasn't said, it isn't written. Any unknown field reads "Not stated." Curating means choosing what to show. It never means adding.
2. **The transcript is the source of truth.** Where a Zoom or Teams summary disagrees with the transcript, the transcript wins. The agenda tells you what was planned. Only the transcript tells you what happened.
3. **One spelling per person.** Use names exactly as they are spoken or listed. Pick one spelling for each person and use it everywhere in the notes.
4. **Absolute dates only.** Convert relative dates using the meeting date. "This Friday" from a Monday 2026-10-26 meeting becomes "2026-10-30." "Next [weekday]" is ambiguous: use the first such weekday after the meeting, and list the date in the brief for the operator to confirm. If no date was given, write "No date set." Relative periods ("this quarter," "next sprint") are never left bare, because they lose their meaning within weeks. If the inputs state the calendar, convert them ("Q4 2026"). A dated OKR index ("OKR index — Q4 2026") counts. Otherwise, anchor them to the meeting date: "this quarter (as of 2026-10-26)".
5. **What counts as a commitment.** An item counts only if a person accepted it or was assigned it on the record. A suggestion that nobody took on is still listed. In an Owner cell or label, write "not assigned". In running text, write "Owner: not assigned". Add "(proposed by [Name])" at the end of the action. It is never dropped.
6. **Outcomes are never blurred.** Decision, Recommendation, Open question, and Idea are kept distinct (Step 3).
7. **Every item that needs an owner has one.** Owners are named people, never teams. If the meeting named only a team, write the team name with "(no individual named)."

---

## Neutrality rules

The notes work because they read as a standard, fair record. The structure carries the accountability. Dates, owners, and counts say what needs saying. The wording never does.

Impact framing is about **the work**, never about **people**. "This moves the November release" is impact. "Priya's delay moves the November release" is characterization. Write the first one.

- **Same format for everyone.** The operator's own items appear in exactly the same format as everyone else's.
- **Attribution verbs only:** committed to, stated, asked, confirmed, agreed, requested, proposed, recommended, raised, reported, decided.
- **Banned when describing people or their work:** failed, again, still hasn't, yet to, slipped, missed, behind, unfortunately, dropped the ball, pushed back, reluctant, deflected, avoided, concerning, disappointing. Also banned: any adjective or adverb describing a person or how they acted. A date or release can be "at risk" when someone said so in the meeting.
- **No characterization.** No tone, posture, or mood. No guessing at intent. No remarks about who spoke a lot or a little.
- **No meta-purpose.** Never describe the notes as accountability, tracking, auditing, or follow-up enforcement. They are meeting notes.
- **No commentary sections in the notes.** No "internal only" block. Your own judgment goes in the operator brief, not the notes.
- **Plain language.** The Quick Recap, the bold text in Decisions and Still Open, and every Takeaway use no internal jargon: no ticket IDs, code names, table names, or tooling. Name the product effect: "Stop reprocessing from multiplying holdings", not "Natural-key merge". Technical detail goes in the Discussion Summary ("Technical:" fragments and the What was found section). A public term that is the subject of the meeting ("SEC Form D filings", "XBRL") may appear, glossed on first use.
- **Empty sections.** The reference says whether an empty section stays in with "None this meeting" or is left out.

| Banned (editorial) | Correct (ledger) |
|---|---|
| Dev team still hasn't delivered the schema, again. | Schema spec · Owner: Priya Shah · Open since 2026-09-12 · Meetings open: 4 · Not discussed |
| Mark pushed back on the timeline. | Mark Lee stated the 2026-11-06 date is not achievable. New date: Not set. |
| Nobody wanted to own the data mapping. | Data mapping · Owner: not assigned · Due: No date set |

---

## Written for someone who wasn't there

Every entry must make sense to a reader who missed the meeting and has only general knowledge of the project.

- **Self-contained entries.** Each row or bullet stands on its own. Name the actual thing — the system, dataset, document, ticket, or deliverable — every time. Never write "it," "that," "the issue," "the thing from last week," or "as discussed."
- **Say what it is, not just what it's called.** On first mention, give a short plain-language gloss for any internal label, ticket number, or code name: "PM-412 (the Form D matching rules)." Spell out acronyms on first use unless they are standard across the organization.
  - Glosses for internal labels come only from the inputs.
  - Public terms (a regulation, a standard filing, a well-known tool) may get a brief general-knowledge gloss: "SEC Form D (the filing private funds submit when raising capital)." This is the one exception to "never invent," and it never covers facts about the meeting or the organization.
- **Decisions carry their why.** Every decision says what it changes or unblocks, so a reader can see the consequence without having heard the debate.
- **Open questions carry their stakes.** Each one says what input is missing and what waits on the answer.
- **No insider shorthand.** No nicknames for workstreams, no references to side conversations, no jokes or callbacks that only attendees would follow.
- **The top answers "do I need to read this?"** The header lines and layer 2 together tell a non-attendee within ten seconds whether the meeting touched their work.

| Only an attendee follows it | Anyone can follow it |
|---|---|
| Agreed to go with option 2. | **Will source issuer names from SEC Form D filings, not the vendor feed**: removes the vendor contract dependency for the November release (D-20261026-01) |
| Sam will look into it. | Sam Ortiz committed to confirming whether the matching service accepts batch uploads above 10,000 records. Due 2026-10-31. |

---

## Carry-over logic (all meeting types)

Run this only when Previous Meeting Notes are supplied. A reference may add type-specific items to collect.

1. **Collect every open item** from the previous notes. An item is open if its status is not Done, Provided, or Closed. Collect from every ledger table in the previous notes: next steps or action items, recommendations awaiting a decision, requests and dependencies, blockers, open questions or decisions not made, and the previous Carried Over table.
2. **Open since.** If the item was already in the previous Carried Over table, keep the date shown there. Otherwise, use the previous meeting's date.
3. **Meetings open.** Count the meetings the item has been open, including this one. An item first raised last meeting and still open now shows 2. An item carried with count N last time shows N+1.
4. **Status this meeting:**
   - **Done YYYY-MM-DD** — completion was confirmed on the record.
   - **Decided — D-ID** — an open question or recommendation was resolved this meeting.
   - **In progress** — it was discussed and progress was reported, but it isn't complete.
   - **Not discussed** — it wasn't mentioned. It stays open. Never drop it.
   - **Reassigned to [Name]** — the owner changed on the record.
   - **Closed — decided by [Name]** — someone explicitly removed it or made it moot on the record.
5. **What happens next time.** Items marked Done, Decided, or Closed this meeting appear in Carried Over once. They are not carried into the next meeting.

---

## Final check (all meeting types)

Run these, then run the reference's own checks and the decision-log checks.

1. **Purpose.** Is it one plain question or problem, shown as the Quick Recap's bold first line (or the About and Bottom line in the status template)?
2. **Structure.** Are the notes organized by key topic, not by the order things were said? If an agenda was supplied, does every agenda item appear as Covered, Partly covered, or Not reached?
3. **Curation.** Does layer 2 (Decisions and Still Open, or Key Takeaways in other templates) hold the Lead items and nothing else? Could a leader read only the header and Key Takeaways and know what changed?
4. **Impact.** Does every Lead decision have a Why it matters line in business or product terms? Does every impact line trace to something said or a direct consequence, with no invented numbers, claims, or business cases?
5. **Decision threshold.** Does every decision with an ID pass the product-decision test (Step 6)? Do ordinary agreements carry no ID?
6. **Layers and formatting.** Could a reader stop after any section and still have the most important information so far? Does any bold label have prose on the same line? Does any bullet hold more than one point? Is there any sentence where a fragment would do? Fix them. Run both tests: could someone confidently skip the recording, and could someone two months from now see why each decision was made?
7. **Outcomes.** Is each outcome typed honestly? Is any Recommendation written up as a Decision, or any Idea as a commitment?
8. **Neutrality.** Is there any banned word, or any characterization of a person? Remove it.
9. **Ledger.** Does every row that needs one have an owner field and a date field, including "not assigned" or "No date set"? Are all dates absolute?
10. **Sources.** Did any statement come from somewhere other than the transcript, the agenda, or the supplied notes and log? Remove it.
11. **Cold reader.** Read the notes as someone who missed the meeting. Does every entry name its subject, explain any label or acronym on first use, and make sense without the transcript?
12. **Same format.** Are the operator's own items in exactly the same format as everyone else's?
