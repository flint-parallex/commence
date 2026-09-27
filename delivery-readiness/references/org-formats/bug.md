# Bug

A defect in delivered behaviour. Title per `../ticket-titles.md`.

---

## Required sections

### 1 · Description

Two or three sentences describing, at a high level, what the end user was trying to do.

**Environment, account and role must be defined.** A bug without these three is not
reproducible, because the same steps in a different environment or under a different role
are a different test.

### 2 · Steps to reproduce

Numbered, in Jira list formatting. One action per step.

### 3 · Actual results

What happened. As detailed as possible. Screenshot where applicable, with any error
message highlighted.

### 4 · Expected results

What should have appeared — in a view, a calculation, or a business rule. State the
expected behaviour, not merely that the actual behaviour was wrong.

### 5 · Dev notes

As on a story. Inherited or unrefined technical detail, held until owned.

### Component

Per `components.md`. Required.

---

## Notes

**Expected results need a source.** Where the expectation comes from the specification,
link the section and the assertion ID. A bug is a claim that delivered behaviour
contradicts an agreed promise, and it is much stronger when it names the promise.

Where no specification statement covers the behaviour, the expectation is someone's
judgement — say so. That is not a lesser bug, but it may be a specification gap rather
than a defect, and the two are resolved differently.

**Do not write reproduction steps from assumption.** If the steps were not executed, say
what was observed instead.
