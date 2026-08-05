# Initiative Summary

The container above epics. An initiative delivers a business capability that spans multiple epics, ties to one or more OKRs, and has a stakeholder set and a delivery horizon of its own.

Write an initiative when the work spans multiple epics and is coherent as a single business outcome. If it decomposes into one epic, it is an epic.

**The initiative summary is a standing reference, not a status report.** It states what is being delivered and why, and it holds while the initiative is live. Progress, timelines, and tracking belong to `delivery-communication` — do not write status into it.

## The business context gate

**Stop and flag if the inputs are not sufficient to write the business-facing sections.**

The summary, business problem, and business impact are the whole point of this document — it is read by stakeholders who never see an epic or a ticket. They must come from a business source. **Never derive them from technical content.** Given a set of epics or a description of systems to be built, a business narrative can always be constructed that reads well and is grounded in nothing a stakeholder said.

The gate is stricter here than at epic level, because an initiative summary is the most stakeholder-facing artifact this skill produces and because every epic beneath it inherits its framing.

Sufficient business context means all four are known:

1. **Who the consumer is** — a named group, not "the business"
2. **What capability is unavailable to them today**, stated without reference to implementation
3. **The consequence** of that gap
4. **What becomes possible** once it is delivered

The test: **can the business problem be stated without mentioning any table, pipeline, system, or code?** If not, the context is technical and the business sections cannot be written from it.

When context is insufficient, do not draft the sections and do not fill them with plausible language. State it:

> Insufficient business context to write the summary, business problem, and business impact. Missing: *[which of the four]*. The inputs provided describe *[the technical scope]*, which is not a business capability. Provide the requirement or BA source, or these sections should be drafted with the business owner.

The remaining sections — epics, capability dependencies, stakeholders, scope boundary — can be written from what is available. Produce those and flag the gap rather than blocking the whole document.

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
