---
title: ZCNoSQL
type: entity
created: 2026-04-07
updated: 2026-04-16
sources: [catalyst-java-sdk-nosql-security-apigateway.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, sdk, nosql, java, nodejs, class]
---

# ZCNoSQL

## Overview

`ZCNoSQL` is the entry point class for all Catalyst NoSQL operations in the Java SDK. Follows the standard [[instance-objects]] pattern with `ZCNoSQL.getInstance()`. Provides access to table metadata and table instances, from which all CRUD operations are performed. [Source: catalyst-java-sdk-nosql-security-apigateway.md]

## Key Facts

- **Entry pattern**: `ZCNoSQL.getInstance()` returns a lightweight instance
- **Table access**: `getTable(id/name)` for metadata, `getTableInstance(id/name)` for operations (no server call)
- **Metadata**: `getAllTables()` returns all table metadata
- **Item class**: `ZCNoSQLItem` — fluent builder with 15+ data types (`withString`, `withNumber`, `withList`, `withMap`, etc.)
- **Helper classes**: `ZCNoSQLAttribute`, `ZCNoSQLValue`, `ZCNoSQLCondition`, `ZCNoSQLResponseBean`
- **Bulk limits**: Insert/Update/Delete max 25 items, Fetch/Query max 100 items

## Node.js SDK Equivalent

In Node.js, NoSQL is accessed via `app.nosql()`. [Source: catalyst-nodejs-sdk-cloud-scale-core.md]

| Java SDK | Node.js SDK |
|----------|-------------|
| `ZCNoSQL.getInstance()` | `app.nosql()` |
| `getTable(name)` / `getTableInstance(name)` | `nosql.table('TableName')` |
| `getAllTables()` | `nosql.getTableDetails('TableName')` |
| `ZCNoSQLItem` builder | `nosql.item()` + `.put(key, value)` |
| `table.insertItem(items)` | `table.insertItems([items])` |
| `table.updateItem(items)` | `table.updateItems([items])` |
| `table.fetchItem(pk, sk)` | `table.fetchItems({ partition_key_value, sort_key_value })` |
| `table.queryTable(pk, sk)` | `table.queryTable({ partition_key, sort_key })` |
| `table.queryIndex(indexName, pk)` | `table.queryIndex('IndexName', { partition_key })` |
| `table.deleteItem(pk, sk)` | `table.deleteItems([{ partition_key_value, sort_key_value }])` |

**Key differences**: Java uses a fluent builder (`ZCNoSQLItem.withString().withNumber()...`), while Node.js uses `nosql.item()` + `.put(key, value)` for flexible type inference. Node.js methods accept arrays for batch operations.

## Appearances

- [[catalyst-java-sdk-nosql-security-apigateway]] — All 10 NoSQL SDK pages
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js NoSQL SDK via `app.nosql()`

## Relationships

- Parent: [[zcproject]] — NoSQL is a Cloud Scale component under the project
- Related: [[zcobject]] — Data Store counterpart (relational)
- Related: [[zctable]] — Data Store table class; NoSQL uses `ZCNoSQLTable`

## Notes

- NoSQL items use partition key (required) + optional sort key, unlike Data Store which uses ROWID
- Supports conditional operations with 14 operators and group conditions (AND/OR)
- Master-slave consistency toggle available for fetch/query operations
