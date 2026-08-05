# Incident Report

A factual record of a production incident, written to the standard template, from whatever sources exist — transcripts, Teams chats, alerts, Jira tickets, logs.

This artifact will be read later, by people who were not there, possibly in a context where accountability is being determined. **It must be accurate, sourced, and blameless.** Those are not tone preferences. They are what makes the record hold.

---

## Controls — read before writing

These govern every section. They exist because the source material is conversational, and conversation contains speculation, blame, and hypotheses that were later disproven.

### 1 · Never name an individual as a cause

Causes are systems, processes, gaps, and decisions. Not people.

> The deployment did not include the schema change.

Never: *"X forgot to include the schema change."*

Individuals appear only where a role is needed for an action item owner, and action items are assigned to **teams**, not people.

### 2 · Chat speculation is not a finding

Teams chats and call transcripts contain live hypotheses. Most are wrong — that is what debugging looks like. **A hypothesis raised during an incident is not a cause unless it was confirmed.**

Where the sources contain a theory that was pursued and abandoned, it belongs in the timeline as an event ("investigated X, ruled out"), never in root cause analysis.

### 3 · Every timeline entry carries a source

Time, event, source. If a time cannot be established from a source, write `time not established` rather than estimating one. An estimated timestamp presented as fact is the most common way an incident record becomes wrong.

### 4 · Separate established from suspected

Root cause is stated as **determined** or **suspected**. If the investigation did not conclusively establish cause, say so. A confident-sounding root cause that was never confirmed is worse than an open one, because it stops anyone looking further.

### 5 · Gaps stay gaps

Detection gaps, tooling gaps, and communication gaps are recorded as they were. Do not soften them, do not explain them away, and do not add mitigating context that the sources do not contain.

### 6 · Impact is stated, not characterized

State what happened to the data and to the customer. No minimizing ("limited impact"), no amplifying ("severe outage"), no reassurance ("we have since ensured this cannot recur"). Severity is a field, not an adjective.

### 7 · No counterfactuals

Never write what should have been done, could have been caught, or would have prevented it. Prevention belongs in action items as forward work, not in analysis as retrospective judgment.

### 8 · Flag what the sources do not support

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
- **Contributing factors**
- **Detection gaps** — why it was not caught earlier

Systems and processes only. No individuals.

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

- Blameless. Systems and processes, never people.
- Every timeline entry sourced. Unestablished times are marked, never estimated.
- Determined versus suspected, stated explicitly.
- No counterfactuals, no reassurance, no characterization of severity beyond the field.
- Flag unsupported sections rather than completing them.
- Action items to teams, with a due date.
- Published in Confluence storage format per `confluence-formatting.md`.
- Neutral and brief. This document is a record, not a narrative.
