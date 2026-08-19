# Jira Formatting

How an authored artifact is rendered and posted to Jira.

**This file governs transport and rendering, never content.** The artifact arrives written. Nothing here changes what it says.

`<angle brackets>` mark conventions specific to this organization that must be filled in. Where one is unfilled, ask rather than guessing — a wrong field mapping fails loudly, a wrong default fails quietly.

## The rule

**Rendering must not change content.**

If an artifact cannot be rendered without altering what it says — a table too wide, a code block Jira mangles, a heading level with no equivalent — that goes back to the authoring skill. It is not fixed in transit.

An artifact whose posted version differs from the reviewed version, with no record of who changed it, is the failure this rule exists to prevent.

## Format

Jira Cloud stores rich text as **ADF** (Atlassian Document Format), not markdown or wiki markup. The API accepts ADF; the editor accepts pasted markdown and converts it, inconsistently.

| Authored as | Renders as | Note |
|---|---|---|
| `# H1` … `### H3` | Heading levels | Jira supports h1–h6. Do not skip levels |
| Table | Table | **Jira tables do not scroll.** Wide tables truncate on the board view — see below |
| Fenced code block | Code block | Specify the language. An unlabelled block renders as plain text |
| Inline code | Monospace | Survives conversion reliably |
| Nested list | Nested list | Two levels render cleanly; three is unreliable |
| Bold, italic | As written | |
| Footnote | **Nothing** | Not supported. Inline the content or drop it |
| HTML | **Escaped** | Never embed HTML in a Jira field |
| Markdown link | Link | Bare URLs auto-link; explicit link text is preferred |

**Wide tables are the most common rendering failure.** Acceptance criteria tables and assertion pair tables both run wide. Where a table exceeds `<max columns>` columns, render it as a list of records rather than truncating. This is a rendering decision, not a content change — the same information, differently laid out.

## Fields

| Field | Source in the artifact | Required |
|---|---|---|
| Summary | The ticket title | Yes |
| Description | Overview, scope, data model reference | Yes |
| Issue type | Story / Task / Spike, from the work set classification | Yes |
| Epic link | The epic named in the ticket header | Yes for stories and tasks |
| Acceptance criteria | `<field name — custom field or part of description>` | Yes |
| Labels | `<convention>` | `<yes/no>` |
| Components | `<convention>` | `<yes/no>` |
| OKR reference | `<field name>` | `<yes/no>` |
| Story points | **Never populated here.** Set in grooming | No |
| Assignee | **Never populated here.** | No |

**Never populate estimation or assignment.** Both are decided by the team, and a pre-filled value reads as a decision already taken.

## Link types

| Relationship in the work set | Jira link type |
|---|---|
| `is blocked by` | Blocks / is blocked by |
| `relates to` | Relates |
| Spike blocking a build ticket | Blocks / is blocked by |
| Subtask of a story | Parent / subtask, not a link |

Create links after all tickets in a set exist. A link to a ticket that has not been created yet fails silently in some clients.

## Project conventions

| Convention | Value |
|---|---|
| Project key | `<key>` |
| Board | `<board>` |
| Default issue type for an enabler | `<Task / Story>` |
| Workflow state on creation | `<state>` |
| Required fields beyond the above | `<list>` |
| Naming convention for summaries | `<convention, if any>` |

## Pre-post check

Mechanical only. **This check does not improve wording.** If the artifact reads badly, that goes back to the authoring skill.

1. Every required field is populated. Name any that are not — do not supply a default.
2. Every table is within the column limit, or has been rendered as records.
3. Every code block declares a language.
4. Every link resolves. Specification references point at a commit, not a branch.
5. Issue type matches the work set classification.
6. No estimation, no assignee.
7. Nesting is two levels or fewer.

Report the result before posting. Where anything fails, say which item and stop.

## Posting

**Confirm before creating.** State what will be created — count, issue types, epic — and wait.

**Create tickets before links.** Then create links in a second pass.

**Report what was created**, with keys, so the work set file can record them. A created ticket whose key is not written back to the repository breaks the trace from specification to Jira.

**Never edit an existing ticket without being asked.** A ticket in flight may have been changed by the team, and overwriting that is destroying someone's work.
