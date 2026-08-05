# Incident Report

A factual record of a production incident, written to the standard template, from whatever sources exist — transcripts, Teams chats, alerts, Jira tickets, logs.

This artifact will be read later, by people who were not there, possibly in a context where accountability is being determined. **It must be accurate, sourced, and blameless.** Those are not tone preferences. They are what makes the record hold.

---

## Controls — read before writing

These govern every section. They exist because the source material is conversational, and conversation contains speculation, blame, and hypotheses that were later disproven.

### 1 · Attribute actions and decisions to the role that took them

**Agentless writing is prohibited.** "The job was not scheduled" obscures the record and is useless to anyone auditing it. Every action, decision, request, and decline is attributed to the role or team responsible.

> The engineering team did not schedule the job in production.
> Pipeline processing checks were requested on 12 May; the tech lead declined; no checks were implemented.
> The upgrade was applied to all pipelines except *[pipeline]*.

**Use roles, not personal names** — tech lead, engineering team, platform team, product owner. Role-level attribution is equally auditable to anyone reading the report and keeps the document about the work rather than about a person. Use a personal name only where the record genuinely requires it.

**What remains prohibited is characterization, not attribution:**

| Do not write | Write |
|---|---|
| *[Name] was careless about scheduling* | The engineering team did not schedule the job in production |
| The job was not scheduled | The engineering team did not schedule the job in production |
| The team failed to prioritize this | Automation for these tables was deprioritized on 3 June |
| This should have been caught | No detection existed for unscheduled jobs |

No adjectives about competence, effort, or attention. No motive. No judgment. Facts with actors.

### 2 · Record the decision trail

Where a preventive action was requested and not taken, that is part of the record. State it factually with dates and roles:

> Pipeline processing checks were requested on *[date]*. The request was declined. The incident on *[date]* originated in the absence of those checks.

This belongs in contributing factors and detection gaps. It is not an accusation — it is the sequence of events, which is what the report exists to establish.

Requests, declines, and deprioritizations are recorded with the date and the role that made them. Never with an interpretation of why.

### 3 · Never name an individual as incompetent, negligent, or at fault

Actions are attributed. Character is not. The distinction is between *what was done* and *what that says about someone* — the first is the record, the second is not yours to write and weakens the record if present.

Action items are assigned to teams.

### 4 · Chat speculation is not a finding

Teams chats and call transcripts contain live hypotheses. Most are wrong — that is what debugging looks like. **A hypothesis raised during an incident is not a cause unless it was confirmed.**

Where the sources contain a theory that was pursued and abandoned, it belongs in the timeline as an event ("investigated X, ruled out"), never in root cause analysis.

### 5 · Every timeline entry carries a source

Time, event, source. If a time cannot be established from a source, write `time not established` rather than estimating one. An estimated timestamp presented as fact is the most common way an incident record becomes wrong.

### 6 · Separate established from suspected

Root cause is stated as **determined** or **suspected**. If the investigation did not conclusively establish cause, say so. A confident-sounding root cause that was never confirmed is worse than an open one, because it stops anyone looking further.

### 7 · Gaps stay gaps

Detection gaps, tooling gaps, and communication gaps are recorded as they were. Do not soften them, do not explain them away, and do not add mitigating context that the sources do not contain.

### 8 · Impact is stated, not characterized

State what happened to the data and to the customer. No minimizing ("limited impact"), no amplifying ("severe outage"), no reassurance ("we have since ensured this cannot recur"). Severity is a field, not an adjective.

### 9 · No counterfactuals

Never write what should have been done, could have been caught, or would have prevented it. Prevention belongs in action items as forward work, not in analysis as retrospective judgment.

### 10 · Flag what the sources do not support

If a template section cannot be completed from the available sources, state that rather than filling it. Every incident report has holes; a report that appears complete but is partly inferred is not usable as a record.

> Response evaluation: communication cannot be assessed from the available sources — no record of stakeholder notification timing was provided.

---

## Required inputs

- All available sources: transcripts, chats, alert history, Jira tickets, logs, emails
- Severity classification
- Affected products or data products
- Jira ticket keys for the incident and any fix

If sources are thin, produce what they support and flag the rest. Never fill from inference.

---

## Template

### Customer impact

- **Symptom** — what the consumer experienced
- **Business hours affected**
- **Data loss or correctness issue** — which, and the scope
- **Products impacted**
- **SLA** — the commitment, and whether it was met

### Incident summary

- **Title**
- **When detected** — UTC
- **Severity**
- **Affected impact**
- **Jira tickets**

### Timeline of events

| Time (UTC) | Event | Source |
|---|---|---|

Chronological. One row per event. Source is the artifact the entry came from — transcript, chat, alert, ticket.

Include investigation paths that were pursued and abandoned. They belong here and nowhere else.

### Root cause analysis

- **Primary cause** — stated as determined or suspected
- **Contributing factors** — including preventive actions requested and not taken, with dates and the role that declined
- **Detection gaps** — what detection did not exist, and why it was not caught earlier

Attribute actions and decisions to roles. Do not characterize the people in them.

### Response evaluation

- **Response time**
- **Runbook** — existed, was followed, was sufficient
- **Tooling**
- **Communication**

Assess each against what the sources show. Where a source is silent, say so.

### Resolution and recovery

- **Fix applied**
- **Rollback** — whether performed, and its effect
- **Residual risk** — what remains, stated plainly

### Lessons learned

- **What worked**
- **What did not work**
- **Gaps**

Factual. Not framed as blame and not framed as reassurance.

### Action items

| Action | Team | Priority | Due |
|---|---|---|---|

Assigned to **teams**, not individuals. Each action is a specific thing to do, with an object — not "improve monitoring."

Actions addressing detection gaps route to the preventive backlog and are tracked to closure.

---

## Rules

- Attribute actions and decisions to roles. Never write agentlessly.
- Record the decision trail — requests, declines, deprioritizations, with dates and roles.
- No characterization of competence, effort, or motive.
- Every timeline entry sourced. Unestablished times are marked, never estimated.
- Determined versus suspected, stated explicitly.
- No counterfactuals, no reassurance, no characterization of severity beyond the field.
- Flag unsupported sections rather than completing them.
- Action items to teams, with a due date.
- Published in Confluence storage format per `confluence-formatting.md`.
- Neutral and brief. This document is a record, not a narrative.
