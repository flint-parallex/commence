# Release Plan Template

Allocates agreed requirements to a delivery. Cites requirement IDs; never restates or
amends them.

---

## Header

Name the scope document. Then state the rule plainly:

> This document cites requirement IDs; it does not restate or amend them. If a requirement
> cannot be met as written, the scope document changes through its decision log — never by
> trimming the requirement to fit this release.

---

## 1. Allocated to this release

| ID | Requirement | Status in this release |

Status is **Full** or **Partial**. A partial allocation must point at §3 — a requirement
marked partial with no stated limitation is the most common way a release plan becomes
misleading.

## 2. Deferred from this release

| Capability | Extends | Blocked on |

Mirrors the scope document's *in scope, not yet allocated* section. State that both remain
in scope and point at it.

## 3. Known limitations

Each limitation gets three things:

- **Bounded by** — what constrains how bad it can get.
- **Mitigated by** — what reduces it in the meantime, stated honestly. A mitigation that is
  actually a workaround should say so. *Scheduling the run inside their update window is a
  mitigation for a known limitation, not a state mechanism.*
- **Resolved by** — which deferred capability closes it, and roughly when.

Distinguish **release limitations** (resolved by a later release) from **permanent
boundaries** (never resolved by this team's work). Filing them together overstates what
later releases will fix.

Known limitations read as weakness and are not. Every item is already true whether or not
it is written down; the only variable is whether it is named here or discovered in
production. Naming them is what makes the committed items credible.

## 4. Approach

From the accepted options document. Where no decision has been made yet, leave a marked
placeholder rather than an assumption.

**Parked design notes** live here — decisions surfaced during scope discussion that do not
belong in scope but must not be lost. Typical content: grain and uniqueness implications,
key handling, provenance semantics, attribute naming and controlled vocabularies, and
anything where a later reader would otherwise make a reasonable but wrong assumption.

Write each note so it survives without the conversation that produced it. *Filing reference
is decision provenance, not the attribute source of record* is a usable note. *Filing
reference — see discussion* is not.

## 5. Acceptance — what this delivers to the business

Specific, and checkable by someone who did not build it. Include baseline numbers where
available.

This is what an engineering leader reads first and the most common thing to leave empty.
Its absence stalls option generation more reliably than any other gap, because it is the
section that says what "done" means.

## 6. Open for later releases

Deferred capabilities, and any scope question the release did not settle.
