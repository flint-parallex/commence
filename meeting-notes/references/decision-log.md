# Decision Log

Load this for every meeting type.

The decision log is one table for the whole data engineering team. Each row is a product decision, or an open product decision that's waiting on someone. The Product and OKR columns make it filterable, so one log can serve every product and nobody has to search several pages.

Meeting notes stay where they are. Each log row links back to the note it came from. That note holds the full context.

Meeting notes are curated as decisions with rationale, not as minutes (`REPO-STRUCTURE.md`). The log is where those decisions are gathered.

---

## What reaches the log

Only outcomes that pass the **product-decision test** in SKILL.md Step 6. They have to change the product's direction, what gets implemented, what unblocks it, or what enhances it.

| Goes to the log | Stays in the meeting note only |
|---|---|
| "We'll source reference data from public filings instead of the paid feed." (Implementation) | "Alex will send the sample file by Thursday." (Next step: accepted on the record) |
| "We won't build the PDF export. Nobody's asked for it this year." (Direction, No-go) | "Let's move this sync to Tuesdays." (Process) |
| "We can't start the migration until the budget is approved. I'll take it to Jordan." (Unblocking, Open: owner is the speaker, review with Jordan) | "Maybe I'll look at the retry logic." (Hedge: not recorded as a commitment) |
| "Adopting automated duplicate flagging for the next release." (Enhancement) | "A dashboard could be nice someday." (Idea) |

An Open row needs a **named** decision owner or reviewer, taken from what was said. If nobody was named, the item stays in the note's Open Questions with "not assigned", and the brief asks the operator whether it belongs in the log.

**When in doubt, ask one question:** would someone on another team, months from now, need to know this to understand why the product is the way it is? If yes, it goes to the log. If not, it stays in the note.

An open question goes to the log only when it blocks the product **and** needs a decision from a named person. Ordinary open questions stay in the note's Open Questions table.

## The log table

```markdown
| ID | Date | Product | OKR | Decision | Category | Status | Decided by / Decision owner | Review with | Why it matters | Rationale & alternatives | Revisit if | Source |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| D-YYYYMMDD-NN | YYYY-MM-DD | [Product] | [OKR / Not stated] | [The choice, or the open question for Open rows] | [Direction / Implementation / Unblocking / Enhancement] | [Decided: Go / No-go / Change / Defer · Open · Superseded by D-…] | [Name / Name (agreed: Name, Name) / not assigned] | [Name / —] | [One line, business or product terms] | [Reasons given; options not chosen / Not stated] | [Condition stated / Not stated] | [Meeting name, date, link or path to the note] |
```

### Column rules

- **ID** — `D-YYYYMMDD-NN`, using the meeting date and the order of the decision within the note. IDs are append-only and never reused or renumbered.
- **Product** — from the inputs or the meeting context. If the meeting covered several products, write one row per product a decision affects, only if the decision truly differs by product. Otherwise list the products in one row. If a decision was stated as cross-product but only this product was named, write this product plus "(cross-product)". If the product isn't clear, write "Not stated" and flag it in the brief. Never guess it from a filename.
- **OKR** — a No-go gets the OKR whose outcome the meeting weighed it against, if one was stated. Otherwise, write "Not stated". If two KRs fit equally, list both, separated by " · ". Otherwise, write the most specific match, as ID and text ("KR1.2: Reduce third-party data spend by 20%"), or the Objective if no KR fits. Match only from the OKR index (`docs/okr-map.md` in the repository) or a supplied OKR list, and only when the meeting stated the goal, metric, or outcome that OKR measures. For example, a stated cost saving matches a spend KR, and a stated baseline for the KR's metric ("200 manual analyst hours a quarter") matches a KR to reduce it. Use the Objective only when the meeting stated the Objective's outcome and no KR fits. A guess from the general topic does not match. Every OKR you fill in is listed in the brief for the operator to confirm. Otherwise write "Not stated." A wrong OKR is worse than a blank, because it puts the decision in the wrong filtered view.
- **Decision** — written as the choice made: "Use…", "Will not build…". For Open rows, write the decision that's needed: "Decide whether the 2026-11-06 date holds once the batch limit is known."
- **Status**
  - **Decided** — with a type:
    - **Go:** do something.
    - **No-go:** won't do something.
    - **Change:** alters an earlier decision.
    - **Defer:** explicitly postponed to a stated date or condition.
  - **Open** — the decision is needed and hasn't been made.
  - **Superseded by D-…** — only ever set on an older row, when a later decision replaces it.
- **Decided by / Decision owner** — for Decided rows, who made the call. For Open rows, the person who will bring the decision and own its outcome, usually the one who said "I'll take it to…" or "my call".
- **Review with** — Open rows only: the person whose agreement or approval the decision needs, as named in the meeting. Example: "I'll make that call with Marcus" makes Grace the owner and Marcus the reviewer. For Decided rows, write "—".
- **Rationale & alternatives** — only what was said. If nothing was said, write "Not stated." Never supply a reason yourself.
- **Revisit if** — a stated condition or time limit ("Next quarter, as stated"). This is what makes a No-go safe to revisit later. A No-go with no condition is still logged, with "Not stated".
- **Source** — where the full context lives. In the repository, the note's path. Otherwise, the meeting name and date.

## In the meeting note

The note shows each product decision as one readable line, with the ID at the end:

```markdown
- **Will not build a general HTML table parser**: two engineers for a quarter for one partner's need (D-20261027-01)
```

- Decided product decisions go in **Decisions**.
- Open product decisions go in **Still Open**.
- The full log rows are **not** in the note. They are delivered separately (SKILL.md Step 7), along with any "Update D-… →" lines, and the note's header points to the log.
- Ordinary agreements can appear in Decisions without an ID. Never give one an ID to make it look important.

## Updating existing rows

By default, output rows to **append** and changes to **apply**, and the operator applies them. If the operator asks you to write to the log, append new rows only. Apply "Update D-… →" changes to existing rows only when the operator explicitly confirms them, and never delete or reorder rows.

- **An Open decision gets decided.** Keep the same ID. In the separately delivered rows, list it as "**Update D-… →** Status: Decided: Go; Decided by: Name; Date decided: YYYY-MM-DD; Source: this note."
- **A decision is replaced.** Append a new row with Status "Decided: Change". Also list "**Update D-… →** Status: Superseded by D-[new]."
- **A decision is revisited and kept.** Don't add a row. Record it in the topic's Outcome bullet: "Reaffirmed D-…, reason: …"
- **A decision contradicts an Active row, and nobody mentioned it.** Append the new row as stated. Flag the conflict in the operator brief. Don't resolve it yourself.

## Where the log lives

**Default: one team-wide log.** Filtering on Product and OKR gives each product its own view without separate pages.

- **In the repository**, the log is `docs/decision-log.md` (`REPO-STRUCTURE.md` → Cross-product documents), alongside the OKR and release indexes. If that row isn't in REPO-STRUCTURE.md, output the rows and say in the brief where they should go. Don't create the file (`AGENTS.md`, Before creating a file).
- If the operator keeps the log in Confluence or a spreadsheet, output the rows in a form that pastes into it. The columns stay the same.
- If the operator chooses one log per product instead, keep the same table. The Product column then stays constant, and nothing else changes.

## Final checks (decision log)

1. Does every row pass the product-decision test? Remove any row that's a next step, process agreement, or idea.
2. Does every product decision appear in the note's Decisions or Still Open section, with its ID at the end of the line, and in the delivered log rows with the same ID?
3. Does every Open row have a decision owner and a Review with person, or "not assigned"?
4. Are Rationale, OKR, and Revisit if filled only from the inputs, with "Not stated" otherwise?
