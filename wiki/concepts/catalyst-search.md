---
title: Catalyst Search
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, search, data-store]
---

# Catalyst Search

## Definition

Catalyst Search provides pattern-based search across indexed columns in [[data-store]] tables. Unlike [[zcql]] which queries specific tables with SQL-like syntax, Search queries across multiple tables simultaneously using wildcard patterns. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

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

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]

## Related Concepts

- [[data-store]] — Search operates on Data Store tables
- [[zcql]] — SQL-like queries vs Search's pattern matching
- [[data-store-pagination]] — Different approach to retrieving large result sets
