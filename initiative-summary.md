# Initiative Summary

The container above epics. An initiative delivers a business capability that spans multiple epics, ties to one or more OKRs, and has a stakeholder set and a delivery horizon of its own.

Write an initiative when the work spans multiple epics and is coherent as a single business outcome. If it decomposes into one epic, it is an epic.

**The initiative summary is a standing reference, not a status report.** It states what is being delivered and why, and it holds while the initiative is live. Progress, timelines, and tracking belong to `delivery-communication` — do not write status into it.

## Required inputs

Ask for any missing. Never invent stakeholder names, dates, or metrics.

- Business capability being delivered, and for whom
- OKRs this initiative serves (IP-OKR-###)
- Segment — private markets, public markets, and so on
- Epics contained, or the known shape of them
- Consumer group receiving the outcome
- Named business owner, business analyst, and upstream source owners
- Capability dependencies and their providing teams
- Delivery horizon
- Quantifiable outcomes

## Structure

### Header fields
- **OKRs:** IP-OKR-###
- **Segment:**
- **Delivery horizon:**
- **Status:** Proposed / Active / Complete

### Summary

Three to five sentences. What business capability this delivers, for whom, and why it matters now. This is the section a stakeholder reads on its own — it should stand without the rest.

### Business problem

What is unavailable, manual, or unreliable today, and the consequence of that. Present tense, factual. Not a description of the solution.

### Business impact

What becomes possible once the initiative is delivered. Bulleted, outcome-framed.

Two "why" sections only. Strategic framing belongs in the summary — do not add a third.

### Epics

| Epic | Delivers | Status |
|---|---|---|

Reference each epic. Do not restate its contents, tables, or acceptance — the epic owns those.

### Scope boundary

What this initiative does **not** include. Required.

Derive it the same way as `user-story.md`: where the stated business outcome is broader than the capabilities and inputs available to this initiative, name the residual and state where responsibility sits. Name adjacent work that could reasonably be assumed in scope, and where it routes instead.

At initiative level this is the most consequential section in the document. It is what an epic's own scope boundary inherits from, and it is where a business outcome that exceeds the delivery means gets named once rather than argued repeatedly.

### Capability dependencies

| Capability | Providing team | Status |
|---|---|---|

Work owned by another team. Do not propose tickets for their work. Status is confirmed, pending, or blocked — stated as fact.

### Key stakeholders

| Role | Name |
|---|---|
| Business owner (Data Strategy) | |
| Business analyst | |
| Consumer | |
| Upstream source owners | |
| Delivery team | Data engineering |
| Segment | |

Upstream source owners are required. They are the parties accountable for the data contracts every downstream epic depends on, and naming them at initiative level surfaces a missing contract before any epic is written.

### Success metrics

Quantifiable outcomes for the initiative as a whole, not the sum of its epics. Coverage, volume, latency, adoption, match rate — expressed as numbers where possible.

Delivery dates belong in the header. Sign-off belongs to the epics. Keep this section to measurable business outcomes.

## Rules

- Standing reference, not a status report. No progress narration.
- Two "why" sections. Strategic framing lives in the summary.
- Scope boundary is required and derived.
- Reference epics; never restate their contents.
- Name real people. If a role is unfilled, write `unassigned` — never a placeholder or a guess.
- State dependency and capability status as neutral fact. Never as a blame narrative.
- Link anything with a canonical home. Never copy it.
- Neutral and brief. A stakeholder should be able to read this in two minutes.
