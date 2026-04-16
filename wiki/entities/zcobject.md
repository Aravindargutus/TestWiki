---
title: ZCObject
type: entity
created: 2026-04-05
updated: 2026-04-16
sources: [catalyst-java-sdk-data-store.md, catalyst-java-sdk-overview.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, java, nodejs, sdk, class, data-store]
---

# ZCObject

## Overview

`ZCObject` is the entry point class for all Data Store operations in the Catalyst Java SDK. It provides methods to access tables (with or without API calls) and serves as the bridge between the SDK and the Data Store component. Part of the `com.zc.component.object` package. [Source: catalyst-java-sdk-data-store.md]

## Key Facts

- Accessed via `ZCObject.getInstance()` (uses [[instance-objects]] pattern)
- Two table access modes: `getTable()` (makes API call, returns metadata) vs `getTableInstance()` (no API call, lightweight reference)
- Both modes accept table ID (long) or table name (string)
- Previously mentioned in SDK overview as `ZCDataStore` in the class hierarchy description

## Methods

| Method | Purpose | API Call? |
|--------|---------|-----------|
| `getInstance()` | Get ZCObject instance | No |
| `getTable(long tableId)` | Get table with full metadata | Yes |
| `getTable(String tableName)` | Get table by name with metadata | Yes |
| `getTableInstance(long tableId)` | Get lightweight table ref | No |
| `getTableInstance(String tableName)` | Get lightweight table ref by name | No |
| `getAllTables()` | Get all tables in project | Yes |

## Node.js SDK Equivalent

In the Node.js SDK, Data Store is accessed via `app.datastore()`. Tables are obtained by name or ID. All methods return promises. [Source: catalyst-nodejs-sdk-cloud-scale-core.md]

| Java SDK | Node.js SDK |
|----------|-------------|
| `ZCObject.getInstance()` | `app.datastore()` |
| `getTable(id/name)` (API call) | _Not available_ |
| `getTableInstance(id/name)` (no call) | `datastore.table('Name')` or `datastore.table(id)` |
| `getAllTables()` | `datastore.getAllTables()` |
| `table.insertRow(row)` | `table.insertRows([row])` |
| `table.getRow(rowId)` | `table.getRow(rowId)` |
| `table.getPagedRows()` | `table.getPagedRows({ maxRows, nextToken })` |
| `table.updateRows(rows)` | `table.updateRows([rows])` |
| `table.deleteRow(rowId)` | `table.deleteRow(rowId)` |
| `ZCDataStoreBulk.bulkRead()` | `datastore.bulkRead({ table_name })` |
| `ZCDataStoreBulk.bulkWrite()` | `datastore.bulkWrite({ table_name, file_id })` |
| `ZCDataStoreBulk.bulkDelete()` | `datastore.bulkDelete({ table_name, ids })` |

**Key difference**: Java separates `getTable()` (with API call) from `getTableInstance()` (no call). Node.js only has `datastore.table()` which creates a lightweight reference (no API call).

## Appearances

- [[catalyst-java-sdk-data-store]] — Full documentation of ZCObject as Data Store entry point
- [[catalyst-java-sdk-overview]] — Referenced as part of class hierarchy (as ZCDataStore)
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js SDK equivalent via `app.datastore()`

## Relationships

- Part of [[class-hierarchy]] under [[zcproject]]
- Returns [[zctable]] objects
- Analogous to [[zcuser]] in the authentication domain

## Notes

- The SDK overview mentions `ZCDataStore` as the Data Store class, but the actual code uses `ZCObject`. These appear to be the same or closely related — possible naming inconsistency in docs.
