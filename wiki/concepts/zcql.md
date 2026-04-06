---
title: ZCQL
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, query-language, data-store, zcql]
---

# ZCQL

## Definition

ZCQL (Zoho Catalyst Query Language) is Catalyst's query language for performing SELECT, INSERT, UPDATE, and DELETE operations on [[data-store]] tables. The SDK provides a single method `executeQuery()` with optional flags for v2 query syntax and OLAP database routing. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Aspects

- **Single SDK method**: `ZCQL.getInstance().executeQuery(query, isV2, isOLAP)`
- **Supports SQL-like syntax**: SELECT, INSERT, UPDATE, DELETE on Data Store tables
- **v2 queries**: Boolean flag enables newer query syntax features
- **OLAP routing**: Boolean flag routes queries to the OLAP analytical database (SELECT only)
- **Returns**: `ArrayList<ZCRowObject>` — same row objects as Data Store operations
- **Scope**: Supports both Admin and User scopes via `getInstance(project)`

## SDK Entry Point

```java
ZCQL zcql = ZCQL.getInstance();
ArrayList<ZCRowObject> rows = zcql.executeQuery("SELECT * FROM empDetails LIMIT 10", true, false);
```

With custom scope:
```java
ZCProject userProject = ZCProject.getProject("user");
ZCQL.getInstance(userProject).executeQuery("SELECT * FROM test");
```

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]

## Related Concepts

- [[data-store]] — ZCQL operates on Data Store tables
- [[data-store-pagination]] — Alternative to ZCQL for paginated row retrieval
- [[sdk-scopes]] — ZCQL is one of the scope-controlled components
- [[bulk-operations]] — For large-scale operations vs ZCQL for ad-hoc queries

## Evolution

- **v1**: Original query syntax
- **v2**: Enhanced query features (enabled via boolean flag)
- **OLAP**: Added analytical database routing for SELECT queries
