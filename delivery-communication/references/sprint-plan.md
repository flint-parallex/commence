# Sprint Plan

A published, one-page statement of what the sprint delivers. Read by stakeholders, not by the delivery team.

**Outcome-level only.** No ticket counts, no ticket keys, no velocity, no capacity, no burndown, no assignments. Those invite questions that do not serve the reader and expose composition that is nobody's business outside the team.

## Required inputs

- Sprint number and dates
- Sprint goal (already authored — do not rewrite it)
- OKRs in play, and what each is delivering this sprint
- Target environment per workstream
- Known dependencies: team, what is needed, status
- Releases falling within 30 days of the sprint start, from the release plan

## Structure

### Sprint goal

One line. As authored. Do not expand or reword it.

### What success looks like

Three bullets. The themes of the sprint, outcome-framed.

Three is the count, not a maximum to approach. If there are five themes, the sprint has no focus and the plan should say three anyway — pick the three that matter.

No ticket references. No implementation detail.

### Delivering

| OKR | Delivering | Target |
|---|---|---|

One line per workstream. What a consumer gets, and the environment it lands in.

Not what is inside it. Not how many pieces. Not who is building it.

### Releases — next 30 days

| Release | Product | Date | Environment |
|---|---|---|---|

Releases falling within 30 days of the sprint start date. Sourced from `release-plan.md` — do not restate what a release delivers, and do not maintain the detail separately. Link to the release plan for that.

Include the whole window, not only releases inside the sprint. One landing eight days after the sprint ends is exactly what a reader of a sprint plan needs to see coming.

If none fall in the window, write "None in the next 30 days." Omitting the section reads as an oversight.

### Dependencies

| Team | Needed | Status |
|---|---|---|

Status: confirmed, pending, or blocked. Stated as neutral fact — "contract version pending from the platform team," never "blocked because platform hasn't delivered."

This section is why the page is worth publishing. It puts external dependencies on record, dated, before the sprint runs.

## Rules

- One page. If it does not fit, it is carrying detail it should not.
- Three themes. Not four, not seven.
- No ticket keys, counts, velocity, capacity, or assignments.
- Do not rewrite the sprint goal.
- Releases are sourced from `release-plan.md`. Name, product, date, environment only — never restate what a release delivers, and never maintain the detail in two places.
- Dependency status is neutral fact, never a blame narrative.
- Published in Confluence storage format per `confluence-formatting.md`.
- Nothing here that a reader does not need to know what the sprint delivers.
