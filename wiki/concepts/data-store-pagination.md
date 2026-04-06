---
title: Data Store Pagination
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-data-store.md]
tags: [catalyst, data-store, pagination, sdk]
---

# Data Store Pagination

## Definition

Token-based pagination for retrieving rows from the Catalyst [[data-store]] in batches. Introduced in SDK v1.7.0 as a replacement for the deprecated `getAllRows()` method. [Source: catalyst-java-sdk-data-store.md]

## Key Aspects

- Uses `ZCTable.getPagedRows(nextToken, maxRows)` method
- Returns `ZCRowPagedResponse` with: `getRows()`, `getNextToken()`, `moreRecordsAvailable()`
- `maxRows` parameter is optional — defaults to 200 rows per page
- First call uses `nextToken = null`; subsequent calls use the token from the previous response
- Loop continues while `moreRecordsAvailable()` returns true
- **`getAllRows()` is deprecated** and will be removed in future SDK versions

## Pattern

```java
String nextToken = null;
ZCRowPagedResponse pagedResp;
Long maxRows = 100;
do {
    pagedResp = ZCObject.getInstance().getTable("TableName").getPagedRows(nextToken, maxRows);
    for (ZCRowObject row : pagedResp.getRows()) { /* process */ }
    if (pagedResp.moreRecordsAvailable()) {
        nextToken = pagedResp.getNextToken();
    }
} while (pagedResp.moreRecordsAvailable());
```

## Sources

- [[catalyst-java-sdk-data-store]] — Get Rows section with full pagination example

## Related Concepts

- [[data-store]] — The database component this pagination applies to
- [[bulk-operations]] — Alternative for very large data retrieval (200K rows via bulk read)
