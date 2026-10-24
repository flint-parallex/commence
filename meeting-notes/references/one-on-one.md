# One-on-One

Use for a one-on-one between the operator and one other person. The notes go to that person after the meeting, by email or on a Confluence page they can see. They are a shared recap both people can rely on later.

All rules in SKILL.md apply. This file adds the variants, the template, the section notes, extra inputs, carry-over additions, and its own final checks.

---

## Additional inputs

1. **Counterpart** — the other person's name. Optional. If missing, take it from the transcript.
2. **One-on-one type** — `partner`, `manager`, or `team-lead`. Optional.
   - If not given, infer it from the transcript using the cues in the Variants table.
   - If it's unclear, use `partner`.
3. **Delivery** — `email` or `confluence`. Optional. Defaults to `confluence`. This changes formatting only, never content (see Delivery formats).

---

## Variants

| Type | Counterpart is | Transcript cues | The notes center on |
|---|---|---|---|
| `partner` | A business, analyst, or data strategy partner the operator delivers for | Their projects, what they're seeing in their product or data, feedback on what was built for them, requirements they're planning | Their projects, their feedback, their upcoming needs |
| `manager` | The operator's manager | Priorities, plans, what's coming in the business, feedback on the operator's work | Priorities set, business context, feedback in both directions |
| `team-lead` | A lead on the delivery or Scrum team | What the team is working on or building, sprint work, process, team blockers | Team work in flight, what's being built, process improvements |

Every variant uses the shared frame below. Each variant adds its own sections in the marked slot.

---

## One-on-one handling rules

These apply on top of the neutrality rules in SKILL.md.

- **Off the record stays off.** If either person says something is off the record, between us, or not for the notes, leave it out entirely. Don't mention that anything was left out.
- **No personal matters.** Leave out health, family, personal circumstances, and compensation, unless the counterpart explicitly asked for that item to be in the notes.
- **Feedback is recorded as given.** Attribute it to whoever gave it. Capture the substance in neutral words. Don't soften it, amplify it, or add a response that wasn't given. Record feedback in both directions in the same format.
- **Both people's commitments look alike.** The operator's commitments and the counterpart's commitments appear in the same table, in the same format.

---

## Template — shared frame (use these headers verbatim, in this order)

```markdown
# One-on-One — [Operator] / [Counterpart] — [YYYY-MM-DD]

**About:** Discussed [A], [B], and [C].
**Relevant to:** [Projects, products, or teams the conversation affects]

## Key Takeaways
- [Up to 3 bullets: what changed, or what is now true, because of this conversation]

[VARIANT SECTIONS — insert the sections for this type here]

## Decisions Made
| Decision | Decided by | Context |
|---|---|---|
| [What was decided] | [Name(s)] | [One line: what it changes] |

## Open Questions
| Question | Who answers it | Needed to answer | Target date |
|---|---|---|---|
| [The open question] | [Name / not assigned] | [Input required] | [YYYY-MM-DD / Not set] |

## Action Items
| Owner | Action | Due | Status |
|---|---|---|---|
| [Operator or Counterpart] | [Verb-first action] | [YYYY-MM-DD / No date set] | [New / In progress / Done] |

## Carried Over from Previous One-on-One
| Item | Owner | Open since | Meetings open | Status this meeting |
|---|---|---|---|---|
| [Item] | [Name] | [YYYY-MM-DD] | [N] | [Done YYYY-MM-DD / In progress / Not discussed / Reassigned to Name / Closed — decided by Name] |
```

---

## Variant sections

### `partner` — business, analyst, or data strategy partner

```markdown
## Their Projects
| Project | Where it stands | What they need from us |
|---|---|---|
| [Project, in plain language] | [Current state as they described it] | [Support, data, or delivery they named / None stated] |

## What They're Seeing
- [Observation from their product, data, customers, or market, attributed: "[Name] reported…"]

## Feedback on What We've Built
| Feedback | About | Given by | Next step |
|---|---|---|---|
| [Substance of the feedback] | [Product, feature, or dataset] | [Name] | [Agreed next step / None agreed] |

## Upcoming Requirements
| Requirement | For | Needed by | Maturity |
|---|---|---|---|
| [What they will need, in plain language] | [Product, customer group, or use case] | [YYYY-MM-DD / Not set] | [Idea / Confirmed need / Needs detail] |
```

- **Their Projects** — their work, not the operator's. Include only projects they discussed.
- **What They're Seeing** — signals they reported. Never the operator's interpretation of them.
- **Upcoming Requirements** — Maturity shows how firm the need is, so nobody mistakes an idea for a commitment.

### `manager` — the operator's manager

```markdown
## Priorities
| Priority | Order | Context | Effective |
|---|---|---|---|
| [What was prioritized, in plain language] | [Rank if given / Not ranked] | [Why, as stated] | [YYYY-MM-DD / Now] |

## Plans & What's Coming in the Business
| Item | Timing | What it means for our work |
|---|---|---|
| [Plan, change, or upcoming event] | [YYYY-MM-DD / Quarter / Not stated] | [Effect on the operator's work as stated / Not discussed] |

## Feedback
| Feedback | Given by | About | Agreed next step |
|---|---|---|---|
| [Substance, in neutral words] | [Name] | [Work item, behavior, or deliverable] | [Next step / None agreed] |

## Support Requested
| Requested from | Requested by | What | Needed for | Due | Status |
|---|---|---|---|---|---|
| [Name] | [Name] | [Approval, decision, escalation, introduction, or resource] | [What it unblocks] | [YYYY-MM-DD / No date set] | [Open / Provided YYYY-MM-DD] |
```

- **Priorities** — capture each priority exactly as the manager set it, including any ordering and what it displaces. If a priority changed from the previous one-on-one, say so in Context: "Replaces [previous priority] as top priority."
- **Plans & What's Coming** — business context the manager shared. Record only what was said.
- **Feedback** — in both directions, attributed. Neutral wording, not softened.
- **Support Requested** — asks in either direction: approvals, escalations, decisions, resources.

### `team-lead` — delivery or Scrum team lead

```markdown
## Team Work in Flight
| Work item | Where it stands | Notes |
|---|---|---|
| [Ticket, feature, or workstream, with plain gloss] | [State as described] | [Risks, dependencies, or changes mentioned] |

## What's Being Built Next
| Item | Target | Notes |
|---|---|---|
| [What the team will build] | [Sprint, YYYY-MM-DD, or Not set] | [Scope or sequencing notes] |

## Process Improvements
| Proposal | Raised by | Status | Owner |
|---|---|---|---|
| [The change to how the team works] | [Name] | [Proposed / Agreed / Trialing / Dropped] | [Name / not assigned] |

## Team Blockers
| Impediment | Unblock owner | Needs |
|---|---|---|
| [What is blocked and by what] | [Name / not assigned] | [What clears it] |
```

- **Team Work in Flight** — the state of the team's work as the lead described it.
- **Process Improvements** — Status keeps proposals apart from agreed changes.

---

## Section notes (shared frame)

- **About** — one sentence, two or three topics.
- **Relevant to** — the projects, products, or teams the conversation affects. Include it in both delivery modes.
- **Open Questions** — the one-on-one name for Decisions Not Made. Same rules.
- **Action Items** — commitments from both people. Owner is always one of the two people or a named third party.
- **Carried Over** — include this only when the previous notes from one-on-ones with the same counterpart were supplied.
- **Empty sections** — in a one-on-one, leave out any variant section that wasn't discussed instead of writing "None this meeting." Shared-frame sections stay in, with "None this meeting."

---

## Delivery formats

Content is identical in both modes. Only the formatting changes.

- **`confluence`** — the template as written, with tables. Title: `One-on-One — [Operator] / [Counterpart] — [YYYY-MM-DD]`.
- **`email`** — tables don't survive many email clients, so convert each table row into one bullet with its fields in order, separated by " · ". Example: "Priya Shah · Send the issuer sample file · Due 2026-10-30."
  - Open with a subject line: `One-on-one recap — [Counterpart] — [YYYY-MM-DD]`.
  - Follow it with one opening line: "Recap of our one-on-one. Let me know if I missed or misstated anything."
  - Then the body. No sign-off beyond the operator's name.

---

## Carry-over additions

Run SKILL.md's carry-over logic, using only previous notes from one-on-ones with the same counterpart. Also:

- **`partner`** — carry Upcoming Requirements still at Idea or Needs detail, and feedback whose next step isn't done.
- **`manager`** — carry Support Requested that's still Open. Previous priorities aren't carried as items. A change in priority is recorded in Priorities (see Section notes).
- **`team-lead`** — carry Process Improvements still at Proposed or Trialing.

---

## Final checks (one-on-one)

1. Is anything flagged as off the record, or any personal matter, in the notes? Remove it.
2. Is every piece of feedback attributed and written in neutral words, in both directions?
3. Do the operator's and the counterpart's commitments appear in the same format?
4. Does the variant match the conversation? A manager conversation should not have been rendered as `partner`.
5. In email mode, are there no tables, and are the subject line and opening line present?
