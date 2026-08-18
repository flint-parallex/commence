# Runtime Monitors

Section 6c. Production observation — what is watched, over what window, against what threshold, and how that threshold is arrived at.

Runtime monitors cannot block a merge (criterion 20). They exist to make a production problem visible to someone who is not looking for it, which means the design constraint is not sensitivity but **signal**. A monitor that fires weekly is not a monitor; it is a filter consumers learn to ignore.

## The declared fields

Every monitor carries all six. The first three are what a consumer sees in the alert.

| Field | Content |
|---|---|
| **Data product** | The Solution. Populated from section 1 |
| **Monitor name** | Short, consumer-legible. Read by someone who did not build it |
| **Monitor description** | What condition this detects and what it means for use of the data |
| **Measure and window** | What is measured, and over what evaluation period |
| **Threshold source** | Specification or observed history (criterion 22) |
| **Calibration** | For observed-history monitors: the method and the baseline window. Blank for specification-sourced |

**The alert is populated from the specification.** Product, name and description are spec fields, not fields written later at implementation. Write the name and description for the person receiving the alert at 07:00 who does not know the pipeline — not for the person who built it.

> `hd_vol_chk` — "row count monitor" — not written yet
> `Holdings volume — daily` — "Detects a drop in reported positions loaded per day. A trigger means positions may be missing for recent filings; totals by manager will understate until resolved" — written

## Measure and window

The window is a declared property of the monitor, not an implementation detail. It changes what the monitor can detect and it is chosen from the data's own density.

**Volume.** Daily where daily volume is dense enough for a day-over-day comparison to be stable. Where the product loads sparsely — low row counts, irregular arrival, seasonal concentration — a single day carries too much variance to threshold, and the measure moves to an n-day rolling total. State n. Declare the density that justified the choice, so a later reviewer can tell whether the window still fits.

**Freshness.** Whether a load or creation timestamp has advanced within n days. n comes from the observed refresh pattern, not from the SLO — the SLO is the promise to consumers, and a freshness monitor set at the SLO fires at the moment the promise breaks rather than before it. Set n inside the SLO so there is room to detect and correct (criterion 7).

**Growth.** Anchor growth on the refresh event, not the calendar. Where refresh is irregular — and it usually is — growth per calendar day mixes periods with a load and periods without, and the resulting series is dominated by cadence rather than by growth. Measure growth between successive refreshes, using the same timestamp the freshness monitor reads.

## Calibration

For observed-history monitors only. Specification-sourced monitors take their threshold from the document and are never calibrated (criterion 22).

**Method.** Set the threshold from a statistical value over a baseline window, then backtest: count the days in that window on which the threshold would have triggered. **The target is zero.**

**Declare the baseline window and its assumption.** The backtest treats the baseline period as correct. That assumption is worth stating, because it is the one that fails silently — if the window contains a real incident nobody caught, calibrating to zero triggers encodes the miss as normal, permanently.

**What zero triggers proves, and what it does not.** It proves the monitor will not be noisy. It proves nothing about whether the monitor would catch anything. A threshold wide enough never to have fired is also wide enough to miss a genuine drop.

Where a threshold has been widened to reach zero triggers, record what it can no longer detect. That sentence is the honest cost of a quiet monitor and it belongs in the monitor's description, where the consumer reading an alert — or reading no alert — can see it.

**Prefer generous thresholds.** Over-alerting is the failure mode that destroys a monitoring surface, and a consumer who has stopped opening alerts is worse off than one who never had them. Widen deliberately and record the cost; do not tighten to feel thorough.

## Timing

Observed-history thresholds cannot be set before data exists. This is not a gap in the specification.

At authoring, declare the **measure, the window, the threshold source, and the calibration method**. The number is set once real data has accumulated, and it is set as part of the monitor's post-deploy verification.

Specification-sourced monitors are complete at authoring. Their threshold is the declared rule — uniqueness holds, the key resolves, the unit is present — and there is nothing to calibrate.

## Reconciliation with data assertions

Where a correctness rule appears in both 6b and 6c, **both instances trace to the specification** (criterion 22). The 6b instance gates the build; the 6c instance detects the same rule failing later. Neither is calibrated.

The distinction to hold: a monitor covering the *same column* as an out-of-the-box check is not covered by it if its threshold comes from the specification. The oracle determines the class, not the measure.

## What good looks like

- Every monitor's name and description read correctly in an alert to someone who did not build the product.
- The evaluation window is declared and the density that justified it is stated.
- Freshness sits inside the SLO, not at it.
- Growth is anchored on refresh, not on the calendar.
- Every observed-history monitor declares its baseline window, and the assumption that the window was incident-free is explicit.
- Where a threshold was widened to reach zero backtest triggers, what it can no longer detect is recorded.
- No specification-sourced monitor carries a calibration.
