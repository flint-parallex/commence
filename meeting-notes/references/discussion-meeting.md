# Discussion Meeting

The default template. Use it for meetings held to explore, weigh options, solve a problem, or decide: planning, design, strategy, reviews, working sessions, cross-functional problem-solving.

The top of the page follows the pattern people already know from Zoom and Teams meeting summaries: a quick recap, the key topics, then next steps. Decisions are added to it. Context comes first, so readers know what the conversation was about before they read what it decided. The detail below follows the layers and formatting rules in SKILL.md (Structure).

All rules in SKILL.md apply. This file adds the template, a worked example, the section notes, and its own final checks.

---

## Template (use these headers verbatim, in this order)

```markdown
# [Meeting Name] — [YYYY-MM-DD]

[Name] ([Role]) · [Name] ([Role]) · [Name] ([Role])
Relevant to: [Team or product] · [Team or product]
Status as of [YYYY-MM-DD] · Current decisions: [decision log location]

## Quick Recap
**[The question or problem the meeting was about, as one bold line]**
- **[Key topic]:** [what it was about, one short fragment]
- **[Key topic]:** [one short fragment]
- **[Key topic]:** [one short fragment]

## Decisions
- **[What was decided]**: [the reason or context, one clause] ([D-ID])
- **[What was decided]**: [the reason or context] ([D-ID])

## Still Open
- **[What is unresolved]**: [who decides or answers, and with whom] ([D-ID if logged])
- **[Unowned risk]**: owner not assigned

## Next Steps
| Owner | Action | Due |
|---|---|---|
| [Name / not assigned] | [Verb-first action, short] | [YYYY-MM-DD / No date set] |

## Discussion Summary

### 1. [Key topic] — [Decided / Open / No further action / Actioned / Idea / Informational]
**Takeaway:** [Where it landed, one line]

- **Why it matters**
  - [Impact, one fragment]
- **What was found** / **What was weighed**
  - [Fact or option] ([Name])
- **Concerns raised**
  - [Concern] ([Name])
- **Not adopted**
  - [Idea] ([Name])
- **Outcome**
  - [Decided by Name, agreed by Name / Left open: owner Name / Dropped: reason]
  - Revisit if: [condition] (only when stated)

### Raised in the Meeting

#### [N+1]. [Off-agenda topic] — [Outcome tag]
[Same format as the topics above.]

## Not Reached
- [Agenda item] → [moves to next meeting / YYYY-MM-DD / Not set]

## Carried Over from Previous Meeting
| Item | Owner | Open since | Meetings open | Status this meeting |
|---|---|---|---|---|
| [Item] | [Name] | [YYYY-MM-DD] | [N] | [Done YYYY-MM-DD / Decided ([D-ID]) / In progress / Not discussed / Reassigned to Name / Closed by Name] |

## Proposed Agenda for Next Meeting
1. **[Topic]**
   - Question: [what must be answered]
   - Brings it: [Owner / not assigned]
```

---

## Worked example: the top of the page

The HTML-tables value meeting, written in this template:

```markdown
# US Fundamentals — HTML Tables Discussion — 2026-10-27

Dana Kim (Product Owner) · Leah Brooks (Partner Lead) · Arjun Rao (Engineering Lead) · Mei Tan (Data Analyst)
Relevant to: US Fundamentals · Credit research · ESG product team · Q4 engineering plan
Status as of 2026-10-27 · Current decisions: docs/decision-log.md

## Quick Recap
**Is it worth extracting untagged tables from filings, beyond what XBRL gives us, for partner products?**
- **Partner demand:** credit research copies debt schedules by hand, about 200 hours a quarter
- **ESG interest:** "nice to have," with no use defined yet
- **Build cost:** a general parser takes two engineers for a quarter, with 60% clean output
- **Alternative:** inline XBRL tags plus table structure, for a narrow pilot

## Decisions
- **Will not build a general HTML table parser**: two engineers for a quarter, with only 60% clean output, for one partner's need (D-20261027-01)
- **Will pilot debt maturity schedules for S&P 100 companies using inline XBRL tags**: targets credit research's manual copying; not committed until capacity is confirmed (D-20261027-02)
- **ESG table data stays off the roadmap until ESG defines what it would build**: not a no forever (D-20261027-03)

## Still Open
- **Can three engineering weeks come out of the Q4 plan for the pilot?**: Dana Kim to confirm with Sofia (D-20261027-04)
- **What the pilot must deliver to replace the manual work**: criteria from credit research via Leah Brooks, due 2026-10-30
```

A reader who stops after Quick Recap knows what the conversation was about. A reader who stops after Decisions knows what was settled and why. The IDs sit at the end of each line, where they serve the log without getting in the reader's way.

## Worked example: one topic in the Discussion Summary

Wrong: labels with prose running after them, several points per line.
```markdown
- **Coverage:** SEC Form D covered about 92 percent of issuers on a sample Priya Shah pulled, against about 97 percent for the vendor feed (reported by Sam Ortiz).
```

Right: takeaway first, labels alone, one fragment per point.
```markdown
### 1. Issuer name source — Decided
**Takeaway:** SEC Form D filings replace the vendor feed for issuer names

- **Why it matters**
  - Removes the 140K-a-year vendor contract
  - Trade-off: about 5 points less coverage
- **What was weighed**
  - Form D coverage: about 92% on a sample (Sam Ortiz)
  - Vendor coverage: about 97% (Sam Ortiz)
  - Vendor cost: about 140K a year (Sam Ortiz)
- **Outcome**
  - Decided by Dana Kim; agreed by Sam Ortiz, Priya Shah
  - Revisit if: the gap list shows a top-50 issuer
```

---

## Section notes

### Header (three plain lines, no labels needed)
- **Line 1:** attendees with roles, separated by " · ". If there are more than six, use "[N] attendees" and list them under Discussion Summary.
- **Line 2:** Relevant to. Name teams and products in the words readers use.
- **Line 3:** Status as of the meeting date, plus where current decisions live (`docs/decision-log.md` in the repository). This tells a later reader that Still Open and Next Steps describe the meeting day.
- **Agenda coverage:** add a fourth line only when an agenda was supplied: "Agenda: 2 of 3 covered · Partly: [item] · Not reached: [item]".

### Quick Recap
The context a reader needs before reading the decisions. It answers "what was this conversation about?", the way a Zoom recap does.
- **The bold first line** is the meeting's one main question or problem, from SKILL.md Step 1. Never stack several questions into it. Pick the one the others serve, and let the key topics carry the rest. Write a question when the meeting weighed something ("Is it worth…?", "Should we…?"). Write a problem statement when it addressed one ("Some funds' positions are overstated, up to 2x").
- **Key topics:** three to five bullets, one per key topic from the meeting map, in the order of the Discussion Summary.
  - Each is a short bold topic name plus one fragment, on one line, under about 15 words. This is the only place a label carries text on the same line. Keep it to one fragment and one point.
  - The label names the topic as a noun ("Partner demand", "Build cost", "Overstated positions"), not as a question or a choice (not "Build or not"). If a topic needs two points, it is two topics.
  - Key topics are the **substance** of the conversation, not the agenda's section titles. An agenda item called "Data quality issues" that covered four problems gives four key topics, one per problem, up to five in total. The agenda titles become the Discussion Summary headings.

    Wrong:
    ```markdown
    - **Data quality issues:** some positions overstated up to 2x; history and private holdings unreliable
    - **Fix plan:** an interim fix for duplication versus a lasting key standard needing an owner
    ```
    Right:
    ```markdown
    - **Overstated positions:** up to 2x on some bonds; analysts deduplicating by hand
    - **Month-end at risk:** run took 7 hours, against 90 minutes in August
    - **Unreliable history:** point-in-time views before September can't be trusted
    - **Missing private holdings:** dropped before loading; a regulatory reporting gap
    - **No owner for a key standard:** the root fix needs a cross-product owner
    ```
  - Write them in business and product terms (impact-framing.md). In a technical meeting, the key topics are the **problems as consumers feel them**, not their technical causes: "**Overstated positions:** up to 2x on some bonds", not "**Duplicate rows:** surrogate key per load".
  - Don't put decisions here. The key topics say what was discussed. Decisions say where it landed.

### Decisions
- One line per decision: the **decision in bold**, a colon, then the reason or context as one clause, then the ID in parentheses at the end.
  - The bold part is the decision as a plain statement in product terms: "Will not build…", "Will pilot…", "Stop reprocessing from multiplying holdings…". Never lead with the ID, and never with technical mechanics.
  - The clause after the colon carries the most important reason, scope limit, or condition: the "why" a reader needs to trust the decision. Keep the whole line under about 30 words.
  - Detail beyond one clause goes in the Discussion Summary: the full trade-off, the technical form, and what a fix doesn't cover.
- **Include:** every product decision (SKILL.md Step 6), with its ID. Also include any ordinary agreement that changes what a reader does, without an ID.
- **Revisit if:** when one was stated, add it as one nested bullet: `  - Revisit if: [condition]`. That is the only nested line allowed here.
- If nothing was decided, write "Nothing decided this meeting."

### Still Open
Everything unresolved on the meeting day, in one list: open product decisions (with ID), ordinary open questions (no ID), and unowned risks.
- Same line shape as Decisions: the **open item in bold**, a colon, then who decides or answers (and with whom, and by when), then the ID if it's logged.
- **Unowned risk:** "**[Risk in product terms]**: owner not assigned". This is often the most important line on the page, so list unowned risks first.
- **A partner's problem** we're responding to is not an open item. It belongs in Quick Recap, and the response belongs in Decisions.
- If nothing was open, write "Nothing open at the end of the meeting."

### Next Steps
- Short, verb-first actions. Items nobody took on still appear, per Core rule 5.
- No Serves column. When an action carries out a decision, end the action with the decision's ID in parentheses: "Write up the pilot approach (D-…-02)".
- If there are none, write "None this meeting."

### Discussion Summary
The "smart chapters": one section per key topic, in the same order as Quick Recap.
- Every topic opens with **Takeaway:** on its own line.
- Sub-labels appear only when they have content, in this order:
  - Why it matters (Lead topics only)
  - Coming in
  - What was found (diagnosis: what's wrong, how bad)
  - What was weighed (options and trade-offs)
  - Concerns raised
  - Not adopted
  - Not covered (partly covered items only)
  - Outcome
- Every sub-label is alone on its line, with nested fragments underneath. Use up to six fragments per sub-label. If you need more, split the sub-label. Never merge two points onto one line.
- **Worth-it decisions:** under What was weighed, start fragments with plain (not bold) "Value:" and "Cost:" prefixes, attributed. Under Outcome, add a "Verdict:" fragment in the decider's words.
- **Technical decisions:** under Outcome, add plain-prefix fragments: "Technical:" for the precise change. Partial fixes add "Covers:" and "Doesn't cover:" (impact-framing.md). These prefixes are fragment starters inside nested bullets, not bold labels.
- **Outcome tags:** use the strongest one, in this order: Decided > No further action > Open > Actioned > Idea > Informational. For a partly covered item, write `(partly covered)` before the tag.
- **Not adopted** means an option someone explicitly passed on, or that nobody took up. An option still being assessed goes under What was weighed, marked "(not yet assessed)".
- **Raised in the Meeting** holds off-agenda topics that produced any outcome other than Information. Leave the section out when there are none.
- **No IDs in the Discussion Summary.** The decision is already in Decisions with its ID.

### Other sections
- **Not Reached:** include it only when agenda items were missed.
- **Carried Over:** include it only when previous notes were supplied.
- **Proposed Agenda:** build it only from items in Still Open, ordered by what blocks the most. Leave it out if nothing is open. A stakeholder-facing agenda belongs to `delivery-communication`.

---

## Final checks (discussion)

1. **Quick Recap.** Could someone read only the bold question and the key topics and know what the conversation was about? Are the key topics in product terms, not technical causes, and free of decisions?
2. **Decisions.** Does every line lead with the decision in plain words, follow it with the reason in one clause, and end with the ID? Is any ID at the start of a line, or any line over about 30 words?
3. **Still Open.** Are unowned risks first, marked "owner not assigned"? Is every open product decision there with its ID?
4. **Formatting.** In the Discussion Summary, does any bold label have text after it on the same line, other than **Takeaway:**? Does any nested bullet hold two points? Fix them.
5. **Tags.** Does every topic's outcome tag match its Takeaway and Outcome?
6. **Two months later.** Could a reader see from Quick Recap plus Decisions why each decision was made?
