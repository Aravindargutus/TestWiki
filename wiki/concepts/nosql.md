---
title: NoSQL
type: concept
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-java-sdk-nosql-security-apigateway.md]
tags: [catalyst, nosql, cloud-scale, database, storage]
---

# NoSQL

## Definition

Catalyst Cloud Scale NoSQL is a fully managed, non-relational database that supports document-type data storage in key-value pair based Custom JSON format. It offers schema flexibility, horizontal scalability, and master-slave replication. Accessed via `ZCNoSQL.getInstance()` → `ZCNoSQLTable`. [Source: catalyst-java-sdk-nosql-security-apigateway.md]

## Key Aspects

- **Data model**: Document-type, key-value pairs in Custom JSON format. Partition key required, sort key optional.
- **Supported data types**: String, Number, Int, BigInteger, Short, Float, Double, Long, Binary, Boolean, Null, StringSet, NumberSet, BinarySet, ByteBufferSet, List, Map, JSON
- **CRUD operations**: 10 SDK methods — metadata (2), instance (1), construct (1), item ops (1), insert (1), update (1), fetch (1), query table (1), query index (1), delete (1)
- **Bulk limits**: Insert/Update/Delete max 25 items; Fetch/Query max 100 items per operation
- **Conditional operations**: All mutating operations support conditions with 14 operators (equals, contains, begins_with, between, in, greater_than, etc.) and group operators (AND, OR)
- **Consistency**: `withConsistency(true)` reads from master; `false` reads from slave (cheaper, slight delay)
- **Pagination**: Query returns `getStartKey()` for next page; pass via `withStartKey()`
- **Indexing**: Supports secondary indexes queryable via `queryIndex()` — allows alternate queries without main table primary keys

## Data Store vs NoSQL

| Aspect | Data Store | NoSQL |
|--------|-----------|-------|
| Architecture | Relational | Non-relational |
| Data structure | Structured rows/columns | Semi-structured/unstructured JSON |
| Schema | Fixed, designed in advance | Flexible, items can vary |
| Query language | ZCQL (SQL-like) | Key-based conditions |
| Priority | ACID compliance | Horizontal scalability |
| Best for | Read-heavy, consistent schema | Write-heavy, flexible schema |
| Identifier | ROWID | Partition key + optional sort key |

## Sources

- [[catalyst-java-sdk-nosql-security-apigateway]] — All 10 NoSQL SDK pages

## Related Concepts

- [[data-store]] — Relational counterpart
- [[zcql]] — Query language for Data Store (not applicable to NoSQL)
- [[cache]] — Another Cloud Scale storage option (in-memory, ephemeral)

## Evolution

_Previously flagged as "failed to load" — resolved by using the `docs-ea.catalyst.zoho.com` domain with explicit `/v1/` path. NoSQL is fully available in Java SDK v1._
