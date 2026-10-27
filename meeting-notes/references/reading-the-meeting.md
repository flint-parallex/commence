# Reading the Meeting

Load this for every meeting type, before you build the meeting map (SKILL.md Step 2). It covers how to interpret a conversation so the notes say what the meeting meant, not just what was said.

The goal is notes that need little or no editing. Most editing happens for four reasons:
- the notes followed the conversation's order instead of its logic
- they gave every point equal weight
- they recorded hedged talk as decisions, or real decisions as discussion
- they buried the conclusion

Each section below prevents one of these.

---

## 1. Center yourself before you read

Decide what you're listening for before you read the transcript. Context is what lets you do that, so collect every anchor you have and rank them by strength:

| Anchor | Strength | What it gives you |
|---|---|---|
| Agenda with talking points | Strongest | Purpose, topics, sub-questions, and the order of importance |
| Agenda with topics only | Strong | Purpose and topics. Infer the talking points. |
| Pre-read, doc, or ticket the meeting was about | Strong | Purpose and the options on the table |
| A line from the operator ("this was about X") | Medium | Purpose. Infer the topics. |
| Meeting invite title or description | Medium | Purpose, often vague |
| Previous notes or prior decision notes | Medium | Open items this meeting was likely meant to close, and decisions not to re-litigate |
| Meeting name only | Weak | A guess at the purpose |
| Transcript only | None | Everything is inferred (see 2c) |

Use every anchor you have, not just the strongest. Previous notes plus an invite title together are much better than either one alone.

## 2. The three context levels

### 2a. Full agenda: map the transcript onto the plan

- Each agenda item is a slot. Read the transcript and file every exchange into a slot.
- Treat the agenda's talking points as the questions the meeting meant to answer. For each one, the notes record the answer, or record that it wasn't answered.
- Agenda wording is the planner's. Outcome wording comes from the transcript. Never assume an agenda item was resolved because it was listed.

### 2b. Partial context: anchor what you can, infer the rest

This is the most common case. You have topics without talking points, a purpose line without topics, or an agenda the meeting only partly followed.

- **Anchor first.** Build the known part of the map from the context you have.
- **Fill the gaps from the transcript.** Under each anchored topic, infer the talking points from the threads of discussion. Then file any discussion that fits no anchor (see the gap test below).
- **Let the anchors weight the notes.** Topics the organizer planned are presumed important unless the meeting clearly treated them as minor. Unplanned topics have to earn a place by passing the Lead test (SKILL.md Step 4).
- **Gap test for unanchored discussion.** Does it serve the stated purpose? Then fold it into the nearest topic. Does it not serve the purpose, but it produced an outcome? Then put it under Raised in the Meeting. Neither? Leave it out.

### 2c. Transcript only: reconstruct the plan

1. **Find the purpose.** Read the first five minutes closely. Organizers usually state why everyone is there ("so the goal today is…", "we need to land…", "I wanted to get everyone together because…"). Then read the last five minutes. Wrap-ups say what the meeting thought it accomplished. If the opening and the close disagree, the close wins.
2. **Find the topics.** Look for transitions: "next," "moving on," "the other thing," "switching gears," "okay, on [X]," a new person presenting, or a screen share. Each transition is a likely topic boundary.
3. **Merge by subject.** A subject that comes up three times is one topic. Keep the latest state of it.
4. **Rank by weight.** Rank topics by how much time they took, how many people engaged, and whether they produced outcomes. A two-minute item that produced a decision outranks a twenty-minute item that produced nothing.
5. **Tell the operator.** In the brief, say that the structure was inferred, and that supplying the agenda next time will sharpen the notes.

## 3. Reading outcomes from language

Spoken language is hedged. These cues tell you which outcome a statement belongs to (SKILL.md Step 3). They are cues, not rules. Always read the surrounding exchange.

**A decision usually has:**
- an explicit closing phrase: "let's go with," "we're doing," "decided," "final answer," "that's the call," "agreed," "we'll lock that"
- from someone with the standing to make the call: the owner of the product, budget, or workstream, or the group explicitly agreeing
- with no objection left standing afterward

**It is not a decision when:**
- **Hedges carry it.** "We should probably," "I think we'd want to," "maybe we," "it might make sense to." That is a Recommendation or an Idea.
- **The call gets deferred.** "Let's run that by [Name]," "I'll need to check with," "pending approval." That is a Recommendation, with the decision owner named. If it passes the product-decision test, it is logged as an **Open** row (decision-log.md). Exception: when the person with the standing to decide states the decision and only its *execution* needs someone's approval ("we'll do the pilot; I'll confirm capacity with Sofia"), it's a Go plus an Open row for the approval (SKILL.md Step 3).
- **Something is left unanswered.** A question is asked and the topic moves on. That is an Open question.
- **The agreement is a social reflex.** "Yeah," "sure," or "makes sense" in response to a long explanation, with no clear proposal, records nothing.
- **Someone reverses it later.** A later "actually, wait…" or "let me think about that" undoes an earlier yes. The latest position wins.

**A commitment usually has:**
- a first-person future: "I'll," "I can take that," "I'll have it by"
- or an assignment someone accepted: "[Name], can you…" answered with "yes," "on it," or "will do"
- An unanswered assignment ("[Name], can you…" followed by silence or a topic change) is listed with "Owner: not assigned" and the asker recorded as having requested it.

**A No further action outcome usually sounds like:**
- "not worth it," "let's drop it," "park it indefinitely," "not a priority," "nobody's asking for that," "we're not doing that"
- Record the reason given. That reason is what makes the No-go useful later.

**A Recommendation usually sounds like:**
- "our recommendation is," "we'll propose to [Name]," "the team's view is"
- or consensus in the room on something that needs sign-off from someone who isn't there

## 4. Weighing what was said

Not every turn carries the same weight.

- **Conclusions over process.** When a topic reached an outcome, record the outcome and its reason in one line. Leave out the path that got there, unless a rejected option matters later. If it does, it goes in Alternatives considered.
- **The latest position wins.** A conversation revises itself. Record where it ended, not where it started.
- **Substance over volume.** One sentence of new data can outweigh ten minutes of agreement. Ask what new information or commitment each exchange added.
- **Signals for the business.** Customer quotes, numbers, dates, cost figures, risks named, and dependencies are high-value. Keep them even when they were said in passing.
- **Repeated concerns.** If the same concern came up from several people, and it was not resolved, it's an Open question even if nobody phrased it as one.
- **Leave out:** greetings, logistics ("can you see my screen"), meta-talk about the meeting itself, jokes, and restatements of something already captured.

## 5. Uncertain transcripts

**A transcript is raw evidence, not a decision record.** It contains positions that were floated and withdrawn, and nothing in the text marks which survived (`REPO-STRUCTURE.md`, Raw material). Three rules follow from that. They are adapted from the transcript controls in `delivery-communication/references/incident-report.md` (#4, #6, #10). Apply them as written here; you don't need to load that file.

- **A floated idea is not a finding.** A theory, estimate, or "I think it's because…" counts as fact only if it was confirmed in the meeting. Otherwise, attribute it ("Sam Ortiz stated he is not sure…") or record it as an Idea.
- **Second-hand claims.** A figure or claim relayed from someone who wasn't there is attributed to both people: "(Paul Novak, as reported by Leah Brooks)". It can appear in the Quick Recap or a Decisions line, attributed. List the original source in the brief under people to see the note, so they can correct it.
- **Keep what was established separate from what was suspected.** "The date is at risk if the batch limit is below 10,000 (stated by Sam Ortiz)" is suspected. Don't write it as "The date is at risk."
- **Flag what the sources don't support.** If a template field can't be filled from the inputs, write "Not stated." If a whole section can't be, say so in the operator brief. Never fill it from inference.

### Speakers and garbled passages

- **Unidentified speakers.** Transcripts label them like "Speaker 2." Identify them from context (names used in address, role cues, the attendee list). If you can't, write "an attendee" and flag it in the brief. Never guess a name onto a commitment or decision.
- **Garbled or ambiguous passages.** Leave them out of the notes. If an outcome depends on one, flag it in the brief.
- **Conflicting statements with no resolution.** Record an Open question that states both positions, attributed. Don't pick a side.

## 6. Drive to the point

Write conclusion-first at every level:

- **Page:** Quick Recap (what it was about), then Decisions and Still Open (where it landed), then Next Steps, then the detail.
- **Topic:** the outcome tag in the heading, then Why it matters, then the talking points that support it.
- **Bullet:** the result first, then the attribution and reason. Write "Will use SEC filings for issuer names — decided by Dana Kim; removes the vendor dependency." Don't write "Dana Kim, after discussion of several options, decided…"

The test is whether a reader could stop after any line and still have the most important thing that line's level had to say.
