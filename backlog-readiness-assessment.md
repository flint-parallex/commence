# Backlog Readiness Assessment

Applies `definition-of-ready.md` across a range of the backlog and produces two outputs: a private assessment for the Product Owner, and a grooming communication for the delivery team.

The backlog is rank-ordered. Assess from the top down to a stated depth — "the top 15," "everything above the sprint line." Never assess the whole backlog by default; depth is always stated.

## Required inputs

- The backlog range: from the top, down to a stated ticket or count
- The ticket set — title, type, and current content for each

If a ticket's content is unavailable, list it as **Not assessed** with the reason. Never infer readiness from a title.

---

## Output 1 · Private assessment (Product Owner)

Not shared. This is the working list for what needs fixing before grooming.

```
BACKLOG READINESS — <range>, <date>

| Rank | Ticket | Type | DoR | Unmet conditions | Owner |
|------|--------|------|-----|------------------|-------|
```

- **DoR** — Ready / Partial / Not ready, per `definition-of-ready.md`
- **Unmet conditions** — name each specific condition, not a category. "Upstream contract not versioned," not "upstream issues"
- **Owner** — the party who can resolve it. May be the PO, a BA, a capability team, or engineering

Then:

```
SUMMARY
Ready: n · Partial: n · Not ready: n · Not assessed: n

MOST FREQUENT UNMET CONDITIONS
<condition> — n tickets
```

The frequency list is the improvement instrument. Conditions that fire repeatedly indicate either a systemic upstream gap or a condition worded too tightly. Conditions that never fire are candidates for removal at the next DoR revision. This is the data behind H2 Objective 1's readiness reporting.

---

## Output 2 · Grooming communication (delivery team)

Shared with the team ahead of grooming. Purpose: they arrive knowing what will be reviewed and what is available to pick up.

**Include only tickets assessed Ready or Partial.** Not-ready tickets are the PO's work to fix, not the team's to review. Listing them wastes grooming time and shifts the readiness burden onto engineering.

```
BACKLOG GROOMING — <date>

We'll be reviewing <n> tickets from the top of the backlog.

COMPOSITION
<n> net new dataset builds
<n> empirical monitoring tickets
<n> spikes
<n> KTLO
<n> other

SPIKES — open for pickup
<ticket> — <the question it answers>

KTLO — available
<ticket> — <one line>

FOR REVIEW
| Ticket | Type | Notes |
```

**Composition first.** A team seeing "4 builds, 6 monitors, 2 spikes" understands the shape of the session before reading a single ticket.

**Spikes are listed separately with their question**, not their title. The question is what tells a developer whether they want to pick it up. This is the section that generates volunteers rather than assignments.

**Notes column** carries what is still open on a Partial ticket — stated as fact, never as blame. "Upstream contract pending from the platform team," not "blocked because platform hasn't delivered."

---

## Rules

- Depth is always stated. Never assess the full backlog by default.
- Assess against the published DoR. Do not apply additional judgment about what should count as ready.
- Name specific unmet conditions, never categories.
- Name the owning party for every unmet condition. Neutral fact, never a blame narrative.
- Not-ready tickets appear in the private assessment only.
- Never infer readiness from a ticket title. Unavailable content means Not assessed.
- The readiness call is the Product Owner's. This assessment is an input, never the gate.
- Neutral and brief in both outputs.
