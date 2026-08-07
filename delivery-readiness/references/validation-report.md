# Validation Report

Turns a test plan and its progress into two outputs: a **business-facing validation summary** for stakeholders, and a **team message** for the delivery channel.

Reads from the test plan (`capability-test-plan.md` or a data product functional test) and from progress notes, meeting records, and results.

The plan states what will be checked. This states what has been checked, what it protects consumers from, and what remains.

---

## Source handling

**Extract outcomes, not discussion.** Meeting notes contain deliberation, competing options, and things that were considered and dropped. A validation report carries conclusions.

**Never quote individuals.** Extract what was determined, not who said it.

**Report against the plan's own exit criteria**, not against a general sense of progress. If the plan defines phases with entry and exit criteria, the report states which are met. That is what makes "on track" a fact rather than an opinion.

**Where progress notes do not establish a result, it is outstanding.** Never infer that a check passed because it was discussed.

---

## Output 1 · Validation summary (stakeholder-facing)

### Where we are

Two or three sentences. Phase, how far through, whether the plan's dates still hold. A reader who stops here should know whether to be concerned.

### What we are validating

**The core section.** One block per validation area, three lines each:

**Name it in business language.** What is being verified, not how.

**What we are verifying.** One plain sentence. No mechanism, no tooling, no technique.

**What it protects against.** The specific thing a consumer would see if this were wrong — missing records, incorrect values, stale data, errors in a field they use.

That third line carries the section. Written abstractly, "why it matters" becomes filler about data quality. Written as *here is the failure this catches*, a business reader immediately understands the stake.

Example of the shape, not content to reuse:

> **Completeness of filing movement**
> We are verifying that every filing present at the source appears in the delivered dataset, across the full historical range.
> Protects against consumers analysing an incomplete population — filings silently absent from results, with no indication anything is missing.

**Never describe a check by its technique.** A stakeholder does not need to know it is a count reconciliation or a hash comparison. If a description only makes sense with the technique in it, it is not yet written for this audience.

### Results so far

What has been confirmed. Past tense, and only for results actually established.

### Outstanding

What has not been validated yet, with expected timing. Required — a report listing only what passed reads as complete when it is not, and that is the version that comes back later.

### Issues found and resolved

What was found, what was done, dated. Shows movement, and it is the section most often omitted because attention has moved to the next problem.

### Next

What happens in the next reporting period, and which exit criteria remain.

---

## Output 2 · Team message

Three or four bullets. **Facts, not activities.**

> "Continuing validation on the historical range" — activity. Tells a reader nothing.
> "Load integrity confirmed across the full historical range; two subsets pending reprocessing, expected Thursday" — fact. Tells a reader whether to worry.

Cover:
- What is confirmed
- What is in progress, and when it is expected
- What is outstanding, with owner
- One line on what is next

No preamble, no summary of the summary.

---

## Tense discipline

**Present tense for what is being checked. Past tense only for what is confirmed.**

Mixing them implies validation is further along than it is. That is the sentence someone quotes back later, and it is the most common way a status report becomes inaccurate without containing a single false statement.

---

## Placement

Validation summaries append as dated entries beneath the test plan on its canonical page in the consumer space. A reader lands on one page carrying the agreed exit criteria and the current status against them.

Do not create a separate status page. Separating status from criteria is how the two drift.

---

## Rules

- Every validation area gets all three lines. A check with no stated consumer impact does not belong in a stakeholder document.
- Never describe a check by its technique.
- Outstanding work is stated explicitly, never omitted.
- Report against the plan's exit criteria, not against impression.
- Extract outcomes from notes. Never quote individuals.
- State results and blockers as neutral fact, never as a blame narrative.
- Where the current state includes a known governance or quality gap, state it plainly. Do not write reassurance the evidence does not support.
- Published in Confluence storage format per `confluence-formatting.md`. Status lozenges in result tables; `expand` for detail outside the reading path.
- Neutral and brief. The summary should be readable in two minutes.
