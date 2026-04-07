---
title: ZCNoSQL
type: entity
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-java-sdk-nosql-security-apigateway.md]
tags: [catalyst, sdk, nosql, java, class]
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

## Appearances

- [[catalyst-java-sdk-nosql-security-apigateway]] — All 10 NoSQL SDK pages

## Relationships

- Parent: [[zcproject]] — NoSQL is a Cloud Scale component under the project
- Related: [[zcobject]] — Data Store counterpart (relational)
- Related: [[zctable]] — Data Store table class; NoSQL uses `ZCNoSQLTable`

## Notes

- NoSQL items use partition key (required) + optional sort key, unlike Data Store which uses ROWID
- Supports conditional operations with 14 operators and group conditions (AND/OR)
- Master-slave consistency toggle available for fetch/query operations
