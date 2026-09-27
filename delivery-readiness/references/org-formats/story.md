# Story

Work that requires coding and a release. Business-facing or technical — both use this
format.

Content per `../user-story.md` and `../acceptance-criteria.md`. Title per
`../ticket-titles.md`.

---

## Required sections

### 1 · User story

`As a <role>, I want <what> so that <why>.`

The who, the what, and the why. All three. `../user-story.md` forbids the engineer as the
user — that rule holds here, including on technical stories. A technical story's user is
the consumer of the system behaviour, not the person building it.

### 2 · Acceptance criteria

Define any prerequisite or condition needed for story context, then state the criteria.

Use **ensure** and **verify** as the verbs. Every criterion must be testable.

Explicitly define, where the story touches them:

| | |
|---|---|
| **Fields** | With types, and whether required and visible |
| **Buttons** | With expected results |
| **Dropdowns** | With the reference tables behind them |
| **Notifications and messages** | For success, warning, and error paths separately |

Attach mockups, images and supporting visuals where they make a criterion clearer than
prose does.

### 3 · Dev notes

Tickets are sometimes inherited carrying information that is disjointed, overly technical,
or incomplete. That information is **kept here** until it is refined — not deleted, and
not promoted into acceptance criteria before someone has decided it belongs there.

Dev notes are a holding area with a known exit. Content that stays there indefinitely is
content nobody has owned.

---

## Component and labels

**Component** per `components.md`. Required on every story.

**Labels** per `labels.md`. Every story carries its testing label — `uat` or `dev test` — and that
choice routes who tests it. Add `data quality` where the story's outcome is a data quality
control.

---

## Technical stories

Backend systems, database analysis, APIs — work that requires no business input and no
UAT. If it requires coding and a release, it is a story in this format, with the same
three sections.

It is labelled `dev test`, not `uat`. That label is the difference, not the format.

Work in the same territory that ships nothing is a **task**. See `task.md`.
