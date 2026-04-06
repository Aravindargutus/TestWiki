---
title: ZCObject
type: entity
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-data-store.md, catalyst-java-sdk-overview.md]
tags: [catalyst, java, sdk, class, data-store]
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

## Appearances

- [[catalyst-java-sdk-data-store]] — Full documentation of ZCObject as Data Store entry point
- [[catalyst-java-sdk-overview]] — Referenced as part of class hierarchy (as ZCDataStore)

## Relationships

- Part of [[class-hierarchy]] under [[zcproject]]
- Returns [[zctable]] objects
- Analogous to [[zcuser]] in the authentication domain

## Notes

- The SDK overview mentions `ZCDataStore` as the Data Store class, but the actual code uses `ZCObject`. These appear to be the same or closely related — possible naming inconsistency in docs.
