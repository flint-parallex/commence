# Executive Update

An update on a delivery or validation effort, produced as **two outputs**:

1. **The detailed update** — a Confluence page, created as a child of the test plan so it appears in the page tree and builds a dated history.
2. **The email** — short, carrying the headline and linking to the detailed page.

Read by leadership and business stakeholders who are not tracking the work day to day.

**Business language throughout, in both.** No table names, no tooling, no pipeline mechanics. If a sentence only makes sense to someone who knows the implementation, rewrite it.

## Required inputs

- Current status and whether dates hold
- What has changed since the last update, and when that was
- What is being validated or delivered, in business terms
- Progress signals — numbers where they exist
- Open risks and what is being done about them
- Next milestones with dates

---

# Output 1 · Detailed update (Confluence child page)

Created as a child of the test plan. Title it with the date: `Validation Update — 4 August`.

Published in Confluence storage format per `confluence-formatting.md`.

## Structure

### Subject and status line

> **US Fundamentals — Validation Update, 4 August**

Open with a status indicator and one line:

🟢 On track · 🟡 At risk · 🔴 Delayed

> 🟡 **At risk** — validation is progressing, target date moves to 22 August.

**If the news is a delay, it goes here.** Not in paragraph four. A reader who stops after two lines must have the headline, and burying it is what damages credibility rather than the delay itself.

### Narrative

Two or three sentences. What is happening, in plain terms, and what it means for the people reading.

No technical depth. No process explanation. If a delay is being reported, state the cause in business terms — scope of what needed correcting, volume of data affected — never in implementation terms.

### What changed since last update

Bulleted. What the team has completed since the last checkpoint. Facts with outcomes, not activities.

> Reprocessed the full historical range; all affected records now reflect corrected values.

Not: *"Continued working through reprocessing."*

State the date of the previous update so the interval is clear.

### What we are validating

High-level, business-facing. What is being checked and what it protects against.

One line per area — this is the summary version, not the three-line treatment used in a validation report. A reader should finish this section understanding the scope of what is being verified.

### How we are validating

Where confidence comes from. Automated checks, test coverage, what runs continuously versus what was a one-time verification.

This section exists to answer the unasked question — *how do you know?* — and it is where automation and test coverage belong. Keep it to what a business reader can evaluate: how much is checked automatically, how often, and what would catch a recurrence.

### Signals

A table. The quantitative movement since last checkpoint.

| Measure | Last update | Now |
|---|---|---|

Use it for what moved — records corrected, checks passing, coverage, error counts, items cleared. If a number has not moved, include it anyway; an unchanged number is information.

### Risks and mitigation

| Risk | Impact | Mitigation | Owner |
|---|---|---|---|

Open risks only. State each as a factual condition with what is being done about it. Neutral — never a blame narrative, never a characterization of another team.

If a risk has been cleared since the last update, it belongs in *what changed*, not here.

### Next

What happens next, and the milestones being targeted with dates.

Be specific about dates or say explicitly that a date is not yet set. A vague next section is what generates the follow-up question this document exists to prevent.

---

# Output 2 · The email

Five to eight lines. Derived from Output 1, never a second version of it.

The email exists to get the headline read. The page carries the evidence.

## Structure

**Subject** — product, artifact, date. `US Fundamentals — Validation Update, 4 August`

**Status line** — the indicator and the headline, including any date change.

**Two sentences** — where things stand, and what it means for consumers.

**Next** — what happens next and by when. If there is a near-term checkpoint, say what it is.

**Link** — to the detailed page, with a line naming what is behind it.

> Full update — validation progress, signals, and open risks: [link]

A bare "more detail here" gets ignored. Name the contents so a reader can judge whether to open it.

## The rule that governs the email

**A reader who never clicks the link must not be misinformed.** Most will not click.

So the status, any date change, and the consumer impact are in the email body. The link carries detail and evidence — never the headline.

Nothing else goes in. No signals table, no risk table, no validation breakdown. If the email runs past eight lines, content is being duplicated from the page.

---

## Rules

- Both outputs come from one set of facts. The email is derived from the page, never written separately.
- Status indicator and the headline first, in both. Never bury a delay.
- The email must stand alone for a reader who does not click through.
- Business language throughout. No table names, no tooling, no pipeline mechanics.
- Facts with outcomes, not activities. "Reprocessed the full range," not "worked on reprocessing."
- Present tense for in-progress work, past tense only for what is confirmed complete.
- State what is outstanding. A report listing only progress reads as complete when it is not.
- Include unchanged numbers in the signals table.
- Neutral fact on risks and delays. No blame, no characterization of other teams, no reassurance the evidence does not support.
- Do not explain the history of how a problem arose. State current state and forward plan.
- The detailed page is one screen or close to it. Anything longer belongs in the validation report.
- Each update is a new child page, dated. Never overwrite the previous one — the history is the value.
