# Confluence Formatting

How artifacts are rendered when published to Confluence. Applies to anything posted through the Confluence MCP server.

Confluence stores pages as **storage format** — XHTML with Confluence-specific macro elements. Markdown posted directly renders poorly or not at all. Convert to storage format before publishing.

## Structural rules

**Use headings, not horizontal rules.** A heading already creates visual separation. `<hr />` between every section produces a document that reads as a list of fragments rather than a structured page.

Reserve `<hr />` for a genuine major break — typically no more than one or two on a page, and often none. If a section needs separation, it needs a heading.

**Heading levels are meaningful.** `<h2>` for major sections, `<h3>` for subsections. Do not skip levels. The table of contents macro reads them.

**Tables over nested bullets.** Anything with two or more attributes per item is a table. Nested bullet lists beyond one level are hard to scan.

## Basic elements

```xml
<h2>Section</h2>
<p>Paragraph text.</p>
<ul><li>Item</li></ul>
<ol><li>Item</li></ol>
<strong>bold</strong>
<em>italic</em>
<code>inline code</code>
```

Tables:

```xml
<table>
  <tbody>
    <tr><th>Column</th><th>Column</th></tr>
    <tr><td>Value</td><td>Value</td></tr>
  </tbody>
</table>
```

## Macros

General form:

```xml
<ac:structured-macro ac:name="NAME">
  <ac:parameter ac:name="PARAM">value</ac:parameter>
  <ac:rich-text-body><p>Content</p></ac:rich-text-body>
</ac:structured-macro>
```

### Admonitions — info, note, warning, tip

```xml
<ac:structured-macro ac:name="info">
  <ac:parameter ac:name="title">Optional title</ac:parameter>
  <ac:rich-text-body><p>Content</p></ac:rich-text-body>
</ac:structured-macro>
```

**When to use which:**

| Macro | Use for |
|---|---|
| `info` | Context a reader needs but that is not a caution. Scope notes, provenance, "this supersedes X" |
| `note` | Something easy to miss that changes how the content is read |
| `warning` | Something that will cause a problem if ignored. Missing contracts, breaking changes, gates not met |
| `tip` | Optional guidance. Use sparingly — most content is not a tip |

Do not stack admonitions. Two in a row read as noise. If most of a page is admonitions, none of it is emphasized.

### Code

```xml
<ac:structured-macro ac:name="code">
  <ac:parameter ac:name="language">sql</ac:parameter>
  <ac:parameter ac:name="title">Volume monitor</ac:parameter>
  <ac:plain-text-body><![CDATA[SELECT COUNT(*) FROM table]]></ac:plain-text-body>
</ac:structured-macro>
```

Note `plain-text-body` with CDATA, not `rich-text-body`. Always set `language` — `sql`, `python`, `yaml`, `json`, `bash`. Title it when a page carries more than one block.

### Expand

```xml
<ac:structured-macro ac:name="expand">
  <ac:parameter ac:name="title">Threshold derivation</ac:parameter>
  <ac:rich-text-body><p>Content</p></ac:rich-text-body>
</ac:structured-macro>
```

For content that must be present but not in the reading path — derivation appendices, full column lists, historical detail, long SQL. This is the macro that keeps a page short without losing content, and it is the one most often underused.

### Task lists — checklists

**Every checklist is a Confluence task list.** Never a bulleted list with `[ ]` characters, and never a plain `<ul>`. Only real tasks roll up into task reports and appear in a person's assigned work.

```xml
<ac:task-list>
  <ac:task>
    <ac:task-id>1</ac:task-id>
    <ac:task-status>incomplete</ac:task-status>
    <ac:task-body>
      <ac:link><ri:user ri:account-id="ACCOUNT_ID" /></ac:link> Publish the Definition of Ready to the delivery space
    </ac:task-body>
  </ac:task>
  <ac:task>
    <ac:task-id>2</ac:task-id>
    <ac:task-status>incomplete</ac:task-status>
    <ac:task-body>
      <ac:link><ri:user ri:account-id="ACCOUNT_ID" /></ac:link> Confirm the upstream contract version with the platform team
    </ac:task-body>
  </ac:task>
</ac:task-list>
```

**Order within the task body is load-bearing: assignee first, then the description.** The mention must lead for the task to roll up to that person. A task with the mention buried mid-sentence, or with no mention at all, does not aggregate.

- `ac:task-id` — unique within the page, sequential
- `ac:task-status` — `incomplete` or `complete`
- Assignee — `ri:account-id` on Confluence Cloud, `ri:userkey` on Data Center. Confirm which applies to the instance
- If the assignee is genuinely unknown, ask. Do not emit an unassigned task and do not guess a name

Descriptions are actions with an object: *"Publish the Definition of Ready to the delivery space"*, not *"DoR publication"*. A task that does not say what to do cannot be picked up.

### Status lozenge

```xml
<ac:structured-macro ac:name="status">
  <ac:parameter ac:name="colour">Green</ac:parameter>
  <ac:parameter ac:name="title">Ready</ac:parameter>
</ac:structured-macro>
```

Colours: `Grey`, `Red`, `Yellow`, `Green`, `Blue`. Use inside table cells for readiness, dependency, and gate status. Do not colour-code narrative text.

### Table of contents

```xml
<ac:structured-macro ac:name="toc">
  <ac:parameter ac:name="maxLevel">2</ac:parameter>
</ac:structured-macro>
```

Only on pages long enough to need it — standards, design documents, published references. Not on short pages.

### Panel

```xml
<ac:structured-macro ac:name="panel">
  <ac:parameter ac:name="title">Title</ac:parameter>
  <ac:rich-text-body><p>Content</p></ac:rich-text-body>
</ac:structured-macro>
```

For a grouped block that is not an admonition. Use sparingly — a heading is usually better.

### Links

To another Confluence page:

```xml
<ac:link>
  <ri:page ri:content-title="Definition of Ready" />
  <ac:plain-text-link-body><![CDATA[Definition of Ready]]></ac:plain-text-link-body>
</ac:link>
```

External:

```xml
<a href="https://example.com">Link text</a>
```

To a Jira issue:

```xml
<ac:structured-macro ac:name="jira">
  <ac:parameter ac:name="key">PROJ-123</ac:parameter>
</ac:structured-macro>
```

## Applying this to artifacts

**Standards and published references** — TOC at top, `<h2>` sections, `info` for scope and provenance, `expand` for detail that is not in the reading path.

**Design documents** — `code` macro with language for every SQL or config block, `warning` for unresolved decisions and open questions, `expand` for investigation detail.

**Readiness and status output** — tables with `status` lozenges. Not prose, not bullets.

**Monitoring tickets** — `code` macro per monitor with `language=sql` and a title, derivation appendix inside `expand`.

## Rules

- Convert to storage format before publishing. Never post raw Markdown.
- Headings separate sections. `<hr />` is rare and deliberate.
- Set `language` on every code macro.
- `plain-text-body` with CDATA for code; `rich-text-body` for everything else.
- Never stack admonitions.
- Use `expand` rather than deleting content that belongs on the page but not in the reading path.
- Every checklist is a task list with real checkboxes. Assignee mention first, then the description. Never bulleted pseudo-checkboxes.
- Escape `&`, `<`, and `>` in text content, or wrap in CDATA.
- Do not hard-code page URLs. Resolve targets via `confluence-map.md` and link by page title.

<ac:structured-macro ac:name="note">
  <ac:parameter ac:name="title">Verify against the instance</ac:parameter>
  <ac:rich-text-body><p>Macro availability and parameter names vary between Confluence Cloud and Data Center, and some macros depend on installed apps. Publish one test page using each macro in this file before relying on it, and correct anything that does not render.</p></ac:rich-text-body>
</ac:structured-macro>
