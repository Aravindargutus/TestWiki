---
title: ZCTable
type: entity
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-data-store.md]
tags: [catalyst, java, sdk, class, data-store, table]
---

# ZCTable

## Overview

`ZCTable` represents a table in the Catalyst Data Store. It provides all row and column operations — metadata retrieval, CRUD, pagination, and bulk delete. Part of the `com.zc.component.object` package. [Source: catalyst-java-sdk-data-store.md]

## Key Facts

- Obtained from [[zcobject]] via `getTable()` or `getTableInstance()`
- Tables referenced by unique numeric ID (long) or string name
- Each row has an auto-generated ROWID

## Methods

| Method | Purpose |
|--------|---------|
| `getColumn(columnId)` | Get column metadata by ID |
| `getColumn(columnName)` | Get column metadata by name |
| `getAllColumns()` | Get all columns |
| `insertRow(ZCRowObject)` | Insert single row |
| `insertRows(List<ZCRowObject>)` | Insert multiple rows |
| `getRow(rowId)` | Get row by ROWID |
| `getPagedRows(nextToken, maxRows)` | Paginated row fetch (v1.7.0+) |
| `updateRows(List<ZCRowObject>)` | Update rows (ROWID required) |
| `deleteRow(rowId)` | Delete single row |
| `deleteRows(ArrayList<Long>)` | Bulk delete up to 200 rows |

## Related Classes

| Class | Package | Purpose |
|-------|---------|---------|
| `ZCRowObject` | `com.zc.component.object` | Row data container (set/get column values) |
| `ZCColumn` | `com.zc.component.object` | Column metadata |
| `ZCRowPagedResponse` | `com.zc.component.object` | Pagination response (getRows, getNextToken, moreRecordsAvailable) |

## Appearances

- [[catalyst-java-sdk-data-store]] — Full row/column operations documentation

## Relationships

- Child of [[zcobject]] in the [[class-hierarchy]]
- Rows managed via `ZCRowObject` instances
- Bulk operations use separate `ZCDataStoreBulk` / `ZCBulkReadServices` / `ZCBulkWriteServices` classes
