---
title: Data Store
type: concept
created: 2026-04-05
updated: 2026-04-17
sources: [catalyst-java-sdk-data-store.md, catalyst-java-sdk-overview.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, cloud-scale, database, data-store, relational]
---

# Data Store

## Definition

Catalyst Data Store is a cloud-scale relational database component. It stores data in tables with typed columns and rows, each row identified by a unique auto-generated ROWID. Accessible via the Java SDK through [[zcobject]] → [[zctable]] → row/column operations. [Source: catalyst-java-sdk-data-store.md]

## Platform Overview

> Source: [Data Store Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/data-store/) — Introduction, Tables, Columns, Records, Bulk Operations, Scopes and Permissions. SDK material below covers programmatic access; this section covers console-level schema management.

### Tables
- Created **from the console** (Cloud Scale → Data Store → *Create a new Table*); not creatable via SDK
- **Naming**: alphanumeric + underscores only; no whitespace, special characters, or leading digit
- Auto-assigned unique **Table ID** (used by SDK/APIs)
- Operations: Create, Rename (Edit), **Truncate** (wipe rows, retain schema; requires typing `TRUNCATE` to confirm), Delete
- Truncate runs via background scheduler — other operations allowed while it runs

### Default Columns (auto-created per table)
| Column | Type | Notes |
|---|---|---|
| **ROWID** | BigInt | Primary key, auto-generated, immutable |
| **CREATORID** | BigInt | Catalyst account user ID, immutable |
| **CREATEDTIME** | DateTime | Record creation timestamp |
| **MODIFIEDTIME** | DateTime | Last update timestamp |

### Supported Column Data Types
| Type | Description |
|---|---|
| **Text** | Long-form text, up to **10,000 chars** (supports multiline/rich text) |
| **Var Char** | Variable-length, up to **255 chars** (requires Max Length) |
| **Date** | `YYYY-MM-DD` |
| **DateTime** | `YYYY-MM-DD HH:MM:SS` |
| **Int** | 4-byte integer, up to 10 digits |
| **BigInt** | 8-byte integer, up to 19 digits |
| **Double** | Floating point, up to 17 digits incl. decimal |
| **Boolean** | `true` / `false` |
| **Foreign Key** | References primary key of another table |
| **Encrypted Text** | Encrypted long-form, up to 10,000 chars |

### Column Constraints & Validators
- **Default Value** — used when column left empty (available for all types *except Text*)
- **Search Index** — enables column for [[catalyst-search]] full-text search (all types except Text)
- **IsUnique** — prevents duplicate values; **not editable after creation**
- **IsMandatory** — rejects rows with empty value
- **PII / ePHI Validator** — marks sensitive data (HIPAA/GDPR); logs all row-level activity to [[catalyst-devops|Audit Logs / Application Logs]]
- **Max Length** (Var Char only) — new value must always be ≥ previous value

### Foreign Key Extras
- **Parent Table** — the referenced table
- **On Delete** behavior:
  - **Null** — child FK value set to null when parent row deleted
  - **Cascade** — child row deleted entirely when parent row deleted

### Column Editing Rules
- **Cannot edit after creation**: Column ID, Data Type, IsUnique
- All other characteristics (Name, Default Value, Mandatory, Search Index, Max Length) are editable

### Limits
| | Development | Production |
|---|---|---|
| Rows per table | 5,000 | No upper limit |
| Rows per project | 25,000 | No upper limit |
| Columns per table | 100 (increase via support@zohocatalyst.com) | No upper limit |

### Console Capabilities
- Schema View (list columns with characteristics)
- Records View (browse/edit rows)
- Bulk Operations (import/export)
- Scopes & Permissions per table (Admin / User scope + role-based permissions)
- Search by table name / column name
- Metrics: Table Count and Row Count history

## Key Aspects

- **Relational model**: Tables → Columns → Rows (with ROWID as primary key)
- **Tables and columns** must be created from the Catalyst console (not via SDK)
- **SDK operations**: Full CRUD — insert, get (single/paginated), update, delete
- **Pagination** via `getPagedRows()` with token-based iteration (default 200 rows/page). `getAllRows()` is **deprecated** since v1.7.0
- **Bulk operations**: Read (200K max, CSV output), Write (100K max, CSV from [[stratus]]), Delete (200 max per op)
- **Scopes apply**: [[sdk-scopes]] (Admin/User) control access to Data Store operations
- **Dev limits**: 5,000 rows/table, 25,000 rows/project. No limits in production
- Queryable via [[zcql]] in addition to SDK row operations

## SDK Class Hierarchy

```
ZCObject.getInstance()
  ├── getTable(id/name) → ZCTable (with API call)
  ├── getTableInstance(id/name) → ZCTable (no API call)
  └── getAllTables() → List<ZCTable>
       └── ZCTable
           ├── getColumn() / getAllColumns() → ZCColumn
           ├── insertRow() / insertRows()
           ├── getRow() / getPagedRows() → ZCRowObject / ZCRowPagedResponse
           ├── updateRows()
           ├── deleteRow() / deleteRows()
           └── (bulk via ZCDataStoreBulk)
```

## Node.js SDK Access Pattern

Node.js uses `app.datastore()` → `datastore.table('Name')`. All operations return promises. [Source: catalyst-nodejs-sdk-cloud-scale-core.md]

```js
const datastore = app.datastore();
const table = datastore.table('SampleTable');

// CRUD
await table.insertRows([{ Name: 'Amelia', Age: 25 }]);
const rows = await table.getPagedRows({ maxRows: 200, nextToken: 'token' });
const row = await table.getRow(rowId);
await table.updateRows([{ ROWID: id, Name: 'Updated' }]);
await table.deleteRow(rowId);

// Bulk
await datastore.bulkRead({ table_name: 'SampleTable' });
await datastore.bulkWrite({ table_name: 'SampleTable', file_id: fileId });
await datastore.bulkDelete({ table_name: 'SampleTable', ids: [id1, id2] });
```

**Key difference from Java**: No separate `getTable()` vs `getTableInstance()` distinction — `datastore.table()` always creates a lightweight reference (no API call). Bulk operations are on the `datastore` object in Node.js (vs `ZCDataStoreBulk` in Java).

## Sources

- [[catalyst-java-sdk-data-store]] — Complete SDK documentation (10 operations)
- [[catalyst-java-sdk-overview]] — Mentions Data Store as Cloud Scale component
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js SDK Data Store (11 pages)

## Related Concepts

- [[zcql]] — SQL-like query language for Data Store
- [[sdk-scopes]] — Admin/User scopes apply to Data Store
- [[instance-objects]] — `getTableInstance()` uses this pattern
- [[bulk-operations]] — Async job-based read/write at scale
- [[data-store-pagination]] — Token-based pagination for row retrieval

## Evolution

_Fully documented across both Java and Node.js SDKs. Node.js SDK simplifies the access pattern (no separate instance vs full-object methods) while maintaining the same 10+ operations._
