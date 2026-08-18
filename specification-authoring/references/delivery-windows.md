# Delivery Windows

Section 2. When the data must be available, what bounds that, and what consumers should expect when it is not.

Cadence alone does not specify delivery. *Daily* says a load happens each day; it does not say by when, on which days, or what a consumer waiting at 07:00 should do if it has not arrived. Those are the facts engineering needs to derive a schedule and the facts a consumer needs to plan around.

## The line

**State the deadline and the constraint. Never the schedule that achieves them.**

| Crossed the line | Rewritten |
|---|---|
| The pipeline runs at 04:00 ET | Data is available by 06:00 ET |
| Runs hourly during filing season | Data is available within 2 hours of source publication |
| Triggered on upstream completion | Sources are available from 03:00 ET; delivery is required by 06:00 |
| Backfill job runs on Sundays | A missed window is recovered within one business day |

The test is the same as everywhere else: **would this change if engineering chose a different implementation?** A run time would. A deadline would not.

## The declared fields

| Field | Content |
|---|---|
| **Required delivery time** | Wall clock and timezone. The time by which consumers need the data available |
| **Calendar basis** | Business days, calendar days, or a named market or regulatory calendar. Which holidays apply |
| **Upstream availability** | The earliest sources can be present. Bounds what is achievable and is not ours to change |
| **Processing window** | The interval between upstream availability and required delivery. The number engineering builds against |
| **Downstream dependency** | What waits on this, and when it starts. A consuming process with its own deadline propagates a constraint backwards |
| **Missed-window behaviour** | What a consumer should do and expect when delivery does not occur on time |
| **Late-arriving data** | Whether records can arrive for a period already delivered, and how a consumer distinguishes a completed period from an open one |
| **Recovery expectation** | How long after a missed window delivery is restored, and whether the missed period is filled or skipped |

**The processing window is the deliverable of this section.** Upstream availability and required delivery are both facts about the world; the interval between them is what determines whether the requirement is feasible at all. Where that interval is too short for the volume, that is a finding to raise before the specification is Ready — not something to discover at build.

## Relationship to the service levels

The SLO sits tighter than the SLA, and the gap is the room to detect and correct before a promise breaks (criterion 7).

Delivery windows are where that gap becomes concrete. The required delivery time is the internal target; the published promise sits later. A freshness monitor set at the published promise fires at the moment the promise breaks, which is too late to be useful — set it inside the window, per `runtime-monitors.md`.

## Seasonality and concentration

Where load is unevenly distributed — a regulatory deadline, a month-end close, a market event — say so, and state whether the delivery requirement holds equally in peak and quiet periods.

This matters beyond scheduling. Concentration determines whether a daily volume monitor can be thresholded at all, and a specification that records the concentration gives the monitor design its justification rather than leaving it to be rediscovered. See `runtime-monitors.md`.

## Reprocessing and corrections

State how a correction reaches consumers, because the mechanism changes what a consumer can rely on.

- **Corrections arrive as new records.** History is immutable; a consumer selects. State how they identify the current version.
- **Corrections replace prior records.** History is mutable; a consumer reading yesterday may get a different answer today. State the window in which this can happen.

Where the source itself issues restatements — amendments, refiled reports, revised submissions — state whether the Solution applies them or records them. Recording without applying is a legitimate choice, and it is one consumers must know about, because it moves the decision to them.

## What good looks like

- A required delivery time with a timezone, not a cadence alone.
- The calendar named, including which holidays apply.
- Upstream availability stated as a fact about the source, not an assumption.
- The processing window computed and its feasibility assessed.
- Missed-window behaviour written for the consumer, not for the operator.
- Seasonality recorded where it exists, and its consequence for monitoring noted.
- Correction mechanism stated, and whether restatements are applied or recorded.
