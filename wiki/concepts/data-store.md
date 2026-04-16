---
title: Data Store
type: concept
created: 2026-04-05
updated: 2026-04-16
sources: [catalyst-java-sdk-data-store.md, catalyst-java-sdk-overview.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, cloud-scale, database, data-store, relational]
---

# Data Store

## Definition

Catalyst Data Store is a cloud-scale relational database component. It stores data in tables with typed columns and rows, each row identified by a unique auto-generated ROWID. Accessible via the Java SDK through [[zcobject]] → [[zctable]] → row/column operations. [Source: catalyst-java-sdk-data-store.md]

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
