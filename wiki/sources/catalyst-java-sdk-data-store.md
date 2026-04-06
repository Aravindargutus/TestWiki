---
title: Catalyst Java SDK — Data Store
type: source
created: 2026-04-05
updated: 2026-04-05
source_file: raw/catalyst-java-sdk-data-store.md
tags: [catalyst, java, sdk, data-store, cloud-scale, database, crud, bulk]
---

# Catalyst Java SDK — Data Store

## Summary

The Data Store SDK provides complete CRUD operations and bulk data operations for Catalyst's cloud-scale relational database. The SDK models tables, columns, and rows as `ZCTable`, `ZCColumn`, and `ZCRowObject` classes respectively, accessed through the `ZCObject` entry point. Operations range from simple single-row CRUD to bulk jobs handling up to 200,000 reads or 100,000 writes. [Source: catalyst-java-sdk-data-store.md]

## Key Takeaways

- **Entry point**: `ZCObject.getInstance()` — from which you get table instances [Source: catalyst-java-sdk-data-store.md]
- **Two ways to reference tables**: by table ID (long) or by table name (string) [Source: catalyst-java-sdk-data-store.md]
- **`getTable()` vs `getTableInstance()`**: `getTable()` makes an API call for metadata; `getTableInstance()` returns a lightweight dummy (no API call) — the [[instance-objects]] pattern [Source: catalyst-java-sdk-data-store.md]
- **ROWID**: Auto-generated unique identifier for each row; required for update/delete operations [Source: catalyst-java-sdk-data-store.md]
- **Pagination**: `getPagedRows(nextToken, maxRows)` replaces deprecated `getAllRows()`. Default page size: 200 rows. Available from SDK v1.7.0+ [Source: catalyst-java-sdk-data-store.md]
- **Dev limits**: 5,000 rows/table, 25,000 rows/project. No limits in production [Source: catalyst-java-sdk-data-store.md]
- **Bulk operations**: Read (max 200K rows, CSV output), Write (max 100K rows, CSV from [[stratus]]), Delete (max 200 rows per op) [Source: catalyst-java-sdk-data-store.md]
- **Bulk write requires Stratus**: CSV file must be uploaded to [[stratus]] first, referenced by bucketName/objectKey/versionID [Source: catalyst-java-sdk-data-store.md]

## SDK Method Reference

### ZCObject Methods (Entry Point)

| Method | Purpose | API Call? |
|--------|---------|-----------|
| `getTable(tableId)` | Get table with metadata | Yes |
| `getTable(tableName)` | Get table with metadata by name | Yes |
| `getTableInstance(tableId)` | Get lightweight table reference | No |
| `getTableInstance(tableName)` | Get lightweight table reference by name | No |
| `getAllTables()` | Get all tables in project | Yes |

### ZCTable Methods (Row Operations)

| Method | Purpose |
|--------|---------|
| `getColumn(columnId)` | Get column metadata by ID |
| `getColumn(columnName)` | Get column metadata by name |
| `getAllColumns()` | Get all columns in table |
| `insertRow(row)` | Insert single row |
| `insertRows(rows)` | Insert multiple rows |
| `getRow(rowId)` | Get single row by ROWID |
| `getPagedRows(nextToken, maxRows)` | Get rows with pagination |
| `updateRows(rows)` | Update one or more rows (ROWID required) |
| `deleteRow(rowId)` | Delete single row |
| `deleteRows(rowIdList)` | Delete up to 200 rows |

### Bulk Operations (via ZCDataStoreBulk)

| Method | Purpose | Limit |
|--------|---------|-------|
| `createBulkReadJob(...)` | Bulk read → CSV output | 200,000 rows |
| `getBulkReadJobStatus(jobId)` | Check read job status | — |
| `createBulkWriteJob(details)` | Bulk write from CSV | 100,000 rows |
| `createInsertBulkWriteJob(tableId, obj)` | Bulk insert | 100,000 rows |
| `createUpdateBulkWriteJob(tableId, obj, colId)` | Bulk update | 100,000 rows |
| `createUpsertBulkWriteJob(tableId, obj, colId)` | Bulk upsert | 100,000 rows |
| `getBulkWriteJobStatus(jobId)` | Check write job status | — |

## Entities Mentioned

- [[zcobject]] — Entry point class for Data Store operations
- [[zctable]] — Table class with row/column operations
- [[zoho-catalyst]] — Platform providing the Data Store
- [[stratus]] — Required for bulk write (CSV upload)

## Concepts Introduced

- [[data-store]] — Catalyst's cloud-scale relational database
- [[data-store-pagination]] — Token-based pagination replacing deprecated getAllRows()
- [[bulk-operations]] — Async job-based read/write/delete at scale

## Open Questions

- What column data types are supported (text, number, boolean, file, etc.)?
- How does ZCQL relate to Data Store — can you query across tables?
- What happens when bulk read/write jobs fail partway through?
- Are there column-level constraints (unique, required, default value)?
