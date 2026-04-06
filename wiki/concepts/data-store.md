---
title: Data Store
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-data-store.md, catalyst-java-sdk-overview.md]
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

## Sources

- [[catalyst-java-sdk-data-store]] — Complete SDK documentation (10 operations)
- [[catalyst-java-sdk-overview]] — Mentions Data Store as Cloud Scale component

## Related Concepts

- [[zcql]] — SQL-like query language for Data Store
- [[sdk-scopes]] — Admin/User scopes apply to Data Store
- [[instance-objects]] — `getTableInstance()` uses this pattern
- [[bulk-operations]] — Async job-based read/write at scale
- [[data-store-pagination]] — Token-based pagination for row retrieval

## Evolution

_Previously only mentioned as a Cloud Scale component. Now fully documented with all 10 SDK operations._
