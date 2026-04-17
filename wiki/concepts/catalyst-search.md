---
title: Catalyst Search
type: concept
created: 2026-04-06
updated: 2026-04-17
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, search, data-store, search-integration]
---

# Catalyst Search

## Definition

Catalyst Search provides pattern-based search across indexed columns in [[data-store]] tables. Unlike [[zcql]] which queries specific tables with SQL-like syntax, Search queries across multiple tables simultaneously using wildcard patterns. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Platform Overview

> Source: [Search Integration Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/search-integration/).

### Purpose
Search Integration (aka Catalyst Search) is a Cloud Scale component that searches within **search-indexed columns** of Data Store tables in a single query. It offloads the memory-intensive work of implementing full-text search over high-volume datasets.

### Why Not Use [[zcql]]?
ZCQL joins can combine tables but can only filter by related columns. Search Integration provides **powerful cross-table, cross-column search** against index-enabled columns — not possible via ZCQL.

### Index Source
Columns are marked **search-indexable from the Data Store console** via the *Search Index* column constraint (see [[data-store]] Platform Overview). Indexing/un-indexing is toggled per column at any time.

### Console Support
- Ready-made **code templates** for Java, Node.js, and Python available in the console
- Toggle indexing per Data Store column

### Access Paths
- **Server-side SDKs**: Java, Node.js, Python
- **Client-side Web SDK** (v4)
- **REST API**: `ExecuteSearchQuery`

## Key Aspects

- **1 SDK operation**: `executeSearchQuery(ZCSearchDetails)`
- **Pattern matching**: Supports wildcards (e.g., `"Sa*"`)
- **Multi-table**: Search across columns in multiple tables in a single call
- **Indexed columns only**: Columns must be marked as search-indexed in the console
- **Returns row objects**: `ArrayList<ZCRowObject>` — same as Data Store and ZCQL

## SDK Entry Point

```java
ZCSearchDetails search = ZCSearchDetails.getInstance();
search.setSearch("Sa*");
HashMap<String, List> map = new HashMap();
List cols = new ArrayList();
cols.add("SearchIndexedColumn");
map.put("SampleTable", cols);
search.setSearchTableColumns(map);
ArrayList<ZCRowObject> rows = ZCSearch.getInstance().executeSearchQuery(search);
```

## Node.js SDK Access Pattern

```js
const search = app.search();
const result = await search.executeSearchQuery({
  search: 'santh*',
  search_table_columns: {
    SampleTable: ['SearchIndexedColumn'],
    Users: ['SearchTest']
  }
});
```

Returns JSON with table names as keys and matching row arrays as values. [Source: catalyst-nodejs-sdk-cloud-scale-remaining.md]

**Key difference**: Java uses `ZCSearchDetails` object with setters; Node.js uses a plain JSON config object.

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js Search (2 pages)

## Related Concepts

- [[data-store]] — Search operates on Data Store tables
- [[zcql]] — SQL-like queries vs Search's pattern matching
- [[data-store-pagination]] — Different approach to retrieving large result sets
