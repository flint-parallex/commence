# Organization Jira Formats

The required shape of every artifact that goes into Jira. Recorded as the organization
states it — this folder is **not** where delivery judgement lives.

## How this folder is used

Two passes, in this order, and they are not merged:

1. **Content** — authored per the archetype references one level up (`user-story.md`,
   `task.md`, `spike.md`, `acceptance-criteria.md`, `ticket-titles.md`). What the ticket
   must say, and what it must not say.
2. **Shape** — rendered into the required format in this folder. Which fields, in what
   order, under what names, with which labels.

When the organization revises its conventions, only this folder changes. When delivery
discipline changes, only the archetype references change.

**A required field with no source in the specification is an open question, not an
invented value.** The never-fill rule applies at render time exactly as it applies at
authoring time.

---

## Issue types

| Type | File | When |
|---|---|---|
| Epic | `epic.md` | The container. Charter-style: business problem through resourcing |
| Story | `story.md` | Requires coding and a release. Business-facing or technical |
| Task | `task.md` | Non-development, no release required |
| Subtask | `subtask.md` | The execution ladder under a story |
| Bug | `bug.md` | A defect in delivered behaviour |
| — | `labels.md` | The label vocabulary. Strict, and it routes testing |
| — | `components.md` | Components. Required everywhere — the **team** on an epic, the **technical unit** on a ticket |

---

## The two things most likely to be got wrong

**The story/task line is the release, not the consumer.**

This organization splits on whether the work requires coding and a release. It does not
split on whether a consumer receives the outcome. Technical work on backend systems,
databases and APIs that requires a release is a **story**, in full story format, even with
no business input and no UAT. Work that ships nothing is a **task**.

This overrides the enabler framing in `../task.md`, which split on consumer-facing
outcome. That axis still matters for deciding whether a unit is real work — it does not
decide the issue type here.

**There is no spike issue type.**

A spike is a label. A spike ships nothing, so it is a **task** carrying the `spike` label,
authored per `../spike.md`. The organization's own task examples include design and impact
assessments for an epic, which is exactly what a spike produces.
