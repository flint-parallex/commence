# Catalog DDL

Emitting the specification's semantics into the warehouse, so they exist where queries and agents can reach them.

A specification that lives only as a document describes a product nobody's tooling can see. The same content — column meaning, grain, keys, relationships — is what a catalog carries and what a natural-language query interface reads to answer correctly. Author once; emit to both.

## What the specification owns

| Specification field | Catalog target |
|---|---|
| Section 4 · Asset description | Table comment |
| Section 4 · Column `Meaning` | Column comment |
| Section 4 · Keys and uniqueness | Primary key constraint |
| Section 4 · Relationships | Foreign key constraints |
| Section 4 · Grain | Table comment, stated explicitly |
| Section 7 · Classification | Classification tags |
| Section 7 · Catalog trust status | Certification status |
| Section 8 · Ownership, domain | Catalog properties |

## Rules

**The specification is the source.** DDL is emitted from it, never authored alongside it. Two hand-maintained copies of a column meaning diverge, and the catalog copy is the one consumers read.

**Comments carry meaning, not names.** A comment restating the column name adds nothing and displaces the field that would have helped. If the comment could be derived from the identifier, it is not written yet.

> `cusip` — "The CUSIP" — not written yet
> `cusip` — "Nine-character CUSIP as reported. The primary means of resolving a position to an instrument, and filer-supplied rather than validated by EDGAR" — written

**State the grain in the table comment.** It is the single most useful sentence a consumer or an agent can read about a table, and it is absent from almost every catalog.

**Declare keys as constraints, not as documentation.** A primary key constraint is what tells a query planner and a query-generating agent that a grain holds. Written only in prose, it prevents nothing and informs nothing.

**Declare foreign keys even where they are not enforced.** The relationship is what stops a fan-out being discovered at analysis time. Its enforcement is engineering's decision; its declaration is the specification's.

**Never emit a constraint the specification does not declare.** A constraint inferred from observed data is sourcing, not verification. See `verification.md`.

## Emission

Emit as DDL statements the operator reviews and applies. Do not apply directly.

```sql
COMMENT ON TABLE <catalog>.<schema>.<table> IS
  '<asset description>. Grain: <one row is exactly what>.';

COMMENT ON COLUMN <catalog>.<schema>.<table>.<column> IS
  '<meaning from section 4>';

ALTER TABLE <catalog>.<schema>.<table>
  ADD CONSTRAINT <name> PRIMARY KEY (<declared key>);

ALTER TABLE <catalog>.<schema>.<table>
  ADD CONSTRAINT <name> FOREIGN KEY (<column>)
  REFERENCES <catalog>.<schema>.<parent> (<column>);
```

Where a constraint cannot be added because the data does not currently satisfy it, that is a verification finding — report it per `verification.md`. Do not weaken the constraint to make it apply.

## Downstream

Catalog content authored this way is also the configuration a natural-language query interface reads: table and column descriptions, declared keys, and relationships are what let it resolve a question to the right grain and avoid an unbounded join.

This is a consequence, not a separate deliverable. Specify the product properly and the query layer is configured; specify it loosely and no amount of tuning at the query layer recovers the meaning.

Do not author catalog content *for* the query interface. Author it for the specification, and let it serve both — content shaped to make one tool answer well is content that has stopped describing the product.
