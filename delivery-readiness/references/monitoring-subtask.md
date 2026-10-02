# Monitoring Subtask

The template for the **Add observability and monitoring** subtask in the standard ladder (`org-formats/subtask.md`), and its paired validation subtask.

## When it exists

**Every build story that delivers or changes a dataset carries this subtask.** It is not proposed per-case and not pruned because the dataset is small. A dataset in production with no monitoring reads as healthy until someone notices it is not — which is the failure this subtask exists to prevent.

It is pruned only where the story delivers no dataset at all.

## The parent story

**This subtask never exists alone.** The parent story carries the acceptance criteria that close the ticket; the subtasks carry the steps. A monitoring subtask with no parent has no stated completion condition, and the MR reviewer has nothing to check it against.

Where the parent does not exist, write it first. See `org-formats/subtask.md`.

## Source-specific slates

Where a standard slate exists for the source, use it rather than designing the layers per ticket.

| Source | Slate |
|---|---|
| SEC connector, L1 onboarding | `l1-observability-sec.md` |

A slate fills in all three layers below for that source. The ticket supplies only what is genuinely per-asset.

## Three layers

They are separate because they fail separately and are configured in different places.

| Layer | What | Source |
|---|---|---|
| **1 · Pipeline observability** | Datadog. Run status, duration, row counts in and out, failure alerting | Standard. Same for every dataset |
| **2 · Out-of-the-box monitors** | Schema, volume, freshness. All three, configured, not assumed | Standard. Thresholds from observed history |
| **3 · Data quality monitors** | The rules declared in specification §6 | Per dataset. Cited by assertion ID |

**Layer 2 is a fixed set of three.** The subtask is not complete with two of them. They are cheap, they are the same everywhere, and the one nobody configured is the one that would have caught the incident.

**Layer 3 carries no query.** Each rule is cited by ID with its condition and threshold as declared. Engineering writes the check. See `acceptance-criteria.md`.

---

## Template

```
Summary: Add observability and monitoring — <dataset> — <assignee>

Component: <parent's component>
Assignee: <developer>

Description:

LAYER 1 · PIPELINE OBSERVABILITY (Datadog)

Ensure the pipeline for <dataset> emits to Datadog:
  - Run status on success and failure
  - Run duration
  - Input row count and output row count
  - Failure alert routed to <channel>

LAYER 2 · OUT-OF-THE-BOX MONITORS

Ensure all three are configured on <catalog>.<schema>.<table>. Confirm each individually;
none is assumed from the presence of the others.

  - Schema change
  - Volume
  - Freshness

Thresholds are derived from observed history per the calibration standard — not set by
hand and not copied from another dataset.

LAYER 3 · DATA QUALITY MONITORS

Configure each rule declared in the specification. Condition and threshold are as declared;
write the check against the condition.

  | Assertion ID | Name | Condition | Threshold | Type |
  |---|---|---|---|---|
  | <ID> | <name> | <condition as declared> | <mustBe: 0> | <library / sql> |

For each, record the check identifier back to the specification's binding column.

EXIT

  - All three out-of-the-box monitors configured and confirmed
  - Every declared rule configured
  - Every binding recorded in the specification
  - Datadog receiving events from at least one completed run
```

---

## The paired validation subtask

`org-formats/subtask.md` splits build validation from monitoring validation deliberately. This subtask verifies that the monitors **exist, are bound, and demonstrably ran** — not that they have caught anything.

Its acceptance criterion is an attached evidence table. **The subtask cannot close without it.**

### The evidence table

One row per monitor, over the agreed backtest window.

| Assertion ID | Monitor | Days in window | Days evaluated | Days triggered | Days not triggered | Trigger rate | Execution | Result |
|---|---|---|---|---|---|---|---|---|
| `<ID>` | `<name>` | 30 | 30 | 0 | 30 | 0.0% | PASS | PASS |
| `<ID>` | `<name>` | 30 | 27 | 0 | 27 | 0.0% | **FAIL** | **FAIL** |
| `<ID>` | `<name>` | 30 | 30 | 1 | 29 | 3.3% | PASS | **FAIL — retune** |

### Two independent pass conditions

They fail for different reasons and are never collapsed into one column.

**Execution** — `Days evaluated` equals `Days in window`. PASS only at 100%.

A check that ran on 27 of 30 days tells you nothing about the other 3. Zero triggers over an incomplete window is not a clean result, it is a partial one, and reading it as clean is how a monitor reaches production having never been shown to work. Row 2 above is the case: nothing triggered, and it still fails.

**Result** — trigger rate at or below **2%**, **and** Execution is PASS.

`Trigger rate = Days triggered ÷ Days evaluated`. Above 2%, the thresholds must be retuned before the subtask closes.

Row 3 fails on rate despite full execution. One trigger in 30 days is 3.3%.

**On a 30-day window the rate is a zero-trigger rule.** A single trigger is 3.3% and exceeds the bar; the tolerance only becomes meaningful on a longer window — at 90 days, one trigger is 1.1% and passes. Choose the window deliberately: 30 days means no trigger is tolerated, 90 days means one is. Both are defensible; picking 30 and expecting tolerance is not.

### A monitor above the trigger rate

Retuning is required, but the developer does not decide how. Do not widen the threshold to make the number pass. One of two things is true, and they need different answers:

- **The threshold is wrong** — return it to calibration, per `monitor-calibration.md`. Ground in the robust statistic, widen to cover approved history, re-derive.
- **The baseline contained a real incident nobody caught** — that is a finding about the data, not about the monitor. Report it. Widening past it encodes the incident as normal and the monitor becomes permanently unable to detect a repeat.

Deciding which is not the developer's call. Report the triggering dates and their values, and stop.

### Template

```
Summary: Validate the observability and monitoring implementation — <dataset> — <assignee>

Component: <parent's component>
Assignee: <developer>

Description:

CONFIGURATION

  - Datadog shows events for the most recent completed run
  - Schema, volume and freshness monitors each return a current evaluation
  - Every assertion ID in specification §6 resolves to a configured check
  - Every configured check resolves to an assertion ID in §6
  - Every monitor resolves in the tool with product, environment and fully qualified
    table populated
  - Every binding is recorded in the specification

BACKTEST EVIDENCE

Run every configured monitor against the agreed backtest window of <n> days and attach the
completed evidence table. Do not summarise it in a comment — attach it.

  | Assertion ID | Monitor | Days in window | Days evaluated | Days triggered |
  | Days not triggered | Execution | Result |

  Execution PASS requires Days evaluated = Days in window. 100%, not "substantially all".
  Trigger rate = Days triggered ÷ Days evaluated.
  Result PASS requires trigger rate ≤ 2% AND Execution PASS.

Where the trigger rate exceeds 2%, the thresholds must be retuned. Report the triggering
dates and the observed values with the table; do not adjust the threshold yourself.

EXIT

  - Every monitor tagged with product, environment and table
  - Evidence table attached, every row Execution PASS
  - Every row trigger rate ≤ 2%, or the exception reported with dates and values
  - Any UNBOUND assertion or UNDECLARED check reported

Report any assertion with no check as UNBOUND. Report any check with no assertion as
UNDECLARED. Report any monitor with a missing or incorrect tag as UNTAGGED.

UNBOUND and UNDECLARED go back to the specification. UNTAGGED is fixed here — it is a
configuration omission, not a specification gap.
```

**Both directions are checked.** An assertion with no check is unmonitored; a check with no assertion is enforcing something nobody declared. The second is the one that gets missed.

**The table is an attachment, not a comment.** `org-formats/subtask.md` puts test records in comments per round and templates in attachments — this is a template artifact. A table pasted into a comment cannot be diffed against the next run's.

---
## Required tagging — every observability ticket

**Not SEC-specific and not optional.** Every monitor configured under this subtask carries three tags, populated in the monitoring tool:

| Tag | Value |
|---|---|
| Product | The data product |
| Environment | `dev` / `uat` / `prod` |
| Table | Fully qualified `<catalog>.<schema>.<table>` |

An untagged monitor fires into a queue nobody can filter and never appears in product health reporting — invisible exactly when someone is looking for it. A breach in UAT read as production is a false incident; the reverse is a missed one.

**A tag stated in the ticket and absent from the tool is a tag that does not exist.** This is verified in the validation subtask, not assumed from the ticket.

## Acceptance criteria for this subtask

The three layers above are the table. The criteria reference it and never restate a threshold.

```
- Ensure Datadog receives the events listed in Layer 1 for at least one completed run
- Ensure all three Layer 2 monitors are configured, each confirmed individually
- Ensure every rule in the Layer 3 table is configured, with condition and threshold
  matching the table exactly
- Ensure every monitor is tagged with product, environment and fully qualified table,
  populated in the monitoring tool
- Ensure every binding is recorded in the specification
```

Five criteria for any number of monitors. Adding a rule to the table changes no criterion — which is the point. A criterion that names a specific threshold has to be edited every time the table is, and one of the two edits will eventually be missed.

## Rules

**Never mark this subtask complete with a partial layer 2.** Three monitors, each confirmed.

**Never write a query into the subtask.** Condition and threshold only — the same line the ticket holds everywhere else.

**Bindings are recorded as part of this subtask, not after it.** An unrecorded binding means the specification cannot tell which of its assertions are actually enforced, and coverage reporting silently overstates.

**A rule the developer cannot implement goes back to the specification.** Do not substitute a weaker check to close the subtask. An unimplementable rule is a specification defect and it is cheaper to fix there.

**The description is frozen once development begins**, per `org-formats/subtask.md`. Monitoring requirements that arrive late are a change to the specification first.

**The trigger rate is 2% of days evaluated, not of days in the window.** A monitor that ran on 27 days and triggered on 1 is 3.7%, not 3.3%. Using the window as the denominator flatters an already-failing execution row.

**Execution coverage below 100% is a failure, not a caveat.** A monitor whose logic did not run on every day of the window has not been validated, whatever its trigger count says.

**Criteria reference the table; they never restate a threshold.** Adding a monitor must not require editing a criterion.

**Never close the observability subtask before the evidence table is attached.** The table is the only evidence the monitors were ever demonstrated to run.
