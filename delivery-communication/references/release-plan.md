# Release Plan

What consumers are getting, when, and where. Grouped by product.

Jira already holds releases and their ticket composition. This exists for one reason: **Jira's version description is too short to say what a release actually delivers.** That gap is the whole value. Anything Jira already answers well does not belong here.

## The line

**Published: deliverable-level.** What a consumer receives.

**Internal: ticket-level.** Release composition, ticket keys, scope changes, what moved between releases. Real value for BA coordination and release breakdown — no value to a consumer, and meaningful exposure if published. A ticket manifest lets any reader reconstruct what slipped, what was cut, and what changed scope.

The test for any line of release content: **does a consumer need this to know what they are getting?** Ticket keys, sprint attribution, and scope-change history all fail it.

## Required inputs

- Product or data product each release belongs to
- Release name and version
- Target date
- Target environment
- What the release delivers, at deliverable level

## Structure

### By product

```
## <Product>

| Release | Date | Environment | Delivers |
|---|---|---|---|
```

**Delivers** — two or three lines describing what a consumer gets. Named tables, named capabilities, in consumer language.

> Form D issuer and offering tables, with daily refresh and full history from 2015.

Not: ticket keys, story counts, sprint numbers, or which team built it.

### Quarterly roll-up

At quarter level, one table across products: product, release, date, environment. Nothing below that.

## Rules

- Grouped by product, not by OKR or by sprint.
- Deliverable-level only. No ticket keys or counts.
- No scope-change history. A release that moved does not carry its own record of moving.
- Consumer language. If a line needs internal context to parse, rewrite it.
- Dates as stated. Do not annotate a date with why it changed.
- Published in Confluence storage format per `confluence-formatting.md`.
- Ticket-level composition is kept internally and is never published here.
