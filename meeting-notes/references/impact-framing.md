# Impact Framing

**The voice is owned by `delivery-communication/SKILL.md`**: Voice, Audience shaping, and The line this skill holds. Read those first. This file covers only how that voice applies to meeting notes. It doesn't restate it.

It applies to the Quick Recap, the Decisions and Still Open lines, Takeaways, Key Takeaways, Why it matters lines, and the Why it matters column of each decision-log row. Everything else in the notes stays in plain ledger style.

---

## The shape of an impact line

**[What changed] → [so what, for whom].**

This is delivery-communication's "outcome-level" rule applied to a meeting. Say what the meeting changed for the people who consume the product, not what the meeting did.

| Activity-focused (avoid) | Impact-focused (write) |
|---|---|
| Discussed the vendor feed versus SEC filings. | Issuer names will come from SEC Form D filings, so the November release no longer depends on renewing the vendor contract. |
| Agreed not to build the export feature. | Will not build CSV export this quarter. No requests since July, and the sprint goes to matching accuracy. |
| Talked about the timeline. | The 2026-11-06 release date is at risk until the matching service's batch limit is confirmed (stated by Sam Ortiz). |

## Rules for meetings

These rules exist because the source is a conversation.

1. **Lead with the outcome.** Never start an impact line with "Discussed," "Reviewed," "Talked about," or "Went over."
2. **Ground every line in the meeting.** Use only the meeting's own numbers, dates, and reasons, or the direct consequence of what was decided ("removes," "unblocks," "delays," "frees"). Don't chain consequences that nobody in the meeting stated. This is delivery-communication's "No forecast without basis" rule, applied to a transcript.
3. **Attribute assessments.** If an impact is one person's judgment rather than something agreed, attribute it: "(stated by Name)."
4. **Frame a No-go as a choice.** State what it frees up or what it avoids, if the meeting said so.
5. **Keep it short.** Quick Recap key topics: one fragment each. Decisions lines: under about 30 words. Key Takeaways: one sentence each. Why it matters: one line.

## Translating a technical meeting

Engineering meetings describe problems as causes: keys, duplicates, filters, version chains. Readers need the effect. Translate up, and keep the technical detail one layer down.

1. **Effect first, cause one layer down.** The Quick Recap, the bold text in Decisions and Still Open, and every Takeaway name the effect on the product or its consumers. The technical cause goes in the Discussion Summary, under What was found or in a "Technical:" fragment.

   | Technical (Discussion Summary) | Product (Quick Recap, Decisions) |
   |---|---|
   | Merge keyed on a per-load surrogate; reprocess inserts rows | Every reprocess multiplies the holdings data: 40M → 310M rows in six weeks |
   | Custodian security_id reissued after corporate actions | One security appears under several IDs, so positions are double-counted |
   | valid_to never closed; chains fork on ID change | Point-in-time history before September can't be trusted for affected securities |
   | Null market_value filter at bronze | Private holdings and pending settlements never reach the product |

2. **Use only links the meeting made.** If someone connected the cause to a consumer effect ("that's why PMs see 2x positions"), use that effect. If nobody did, state the direct product effect ("row count multiplies", "job runtime 7 hours versus 90 minutes"), and don't reach further.
3. **Say what a partial fix leaves.** When a decision fixes one cause of a problem that has several, add "Covers:" and "Doesn't cover:" fragments.
   - Covers: new duplicates from reprocessing.
   - Doesn't cover: duplicates from unstable security IDs, which wait for the ID standard (D-…).

   Never let an interim fix read as if it resolves the consumer problem.
4. **Turn capability gaps into a need with an owner.** "We don't have a data architect" plus "someone has to own the key standard" becomes "**Need: an owner for the cross-product primary key standard**", with nested lines for who will ask (Grace Liu), who decides (Priya), and what waits on it.
5. **Technical precision still belongs somewhere.** Exact keys, columns, and checks go in Technical bullets, in the topic detail, and in the log's Rationale column. Engineering builds from those. They never go in the bold lines.

## Choosing the lens

Use delivery-communication's Audience shaping table to choose what the impact line emphasizes. Meeting notes are the record for attendees and the team. When leadership needs the outcome, produce a separate artifact through delivery-communication (SKILL.md Step 7). Don't add a leadership section to the notes.

## Fallback if `delivery-communication` isn't available

- **Concise, outcome-level, neutral, unannotated.**
- State a moved date as a date, without the history behind it.
- No reassurance and no characterization.
- Use plain business words: customer, release, date, cost, risk, scope.
