---
title: ZCQL
type: concept
created: 2026-04-06
updated: 2026-04-17
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, query-language, data-store, zcql, sql, olap]
---

# ZCQL

## Definition

ZCQL (Zoho Catalyst Query Language) is Catalyst's query language for performing SELECT, INSERT, UPDATE, and DELETE operations on [[data-store]] tables. The SDK provides a single method `executeQuery()` with optional flags for v2 query syntax and OLAP database routing. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Platform Overview

> Source: [ZCQL Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/zcql/) — Introduction, SELECT/INSERT/UPDATE/DELETE syntax, WHERE, HAVING, JOIN, GROUP BY, ORDER BY, LIMIT, Functions, V2 Syntax, ZCQL Console.

### Purpose
SQL-like DML query language for [[data-store]] — similar to MySQL / PostgreSQL. Enables retrieval, insertion, updating, and deletion without learning a proprietary syntax.

### Supported Clauses & Features
- **DML**: SELECT, INSERT, UPDATE, DELETE
- **Clauses**: WHERE, HAVING, JOIN (multiple types), GROUP BY, ORDER BY, LIMIT
- **Built-in functions**: arithmetic / numeric operations on result sets
- **Cross-table queries**: JOINs across Data Store tables

### ZCQL V2 Parser (rollout)
| Environment | Auto-migration Date |
|---|---|
| **Development** (all projects, all Orgs) | **December 1, 2024** — auto-mapped to V2 Parser |
| **Production** | **April 1, 2025** onward — auto-mapped to V2 when production is enabled (for orgs already on V2 in Development) |

To use V2 in function code, set the appropriate environment variable (see ZCQL Console docs). V2 has its own syntax and exceptions documented in *ZCQL V2 Syntax and Exceptions*.

### OLAP Routing
ZCQL SELECT queries can be routed to Catalyst's **OLAP analytical database** (read-only aggregate/analytical workloads) via an SDK boolean flag. Non-SELECT operations run only against the transactional Data Store.

### ZCQL Console
In-console **query execution window** for testing queries and viewing responses before embedding them in application code. Accessed under Data Store / ZCQL in the console.

### Access Paths
**Server**: Java, Node.js, Python SDKs + REST API (`ExecuteZCQLQuery`)
**Client**: Web, Android, iOS, Flutter SDKs (queries run with user scope)

### Benefits Summary (from docs)
- Full CRUD in Data Store via familiar SQL
- Low learning curve (MySQL-like)
- Console test-bed before deployment
- Pass queries directly in SDK/API request body
- Arithmetic on result sets via ZCQL functions

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

## Node.js SDK Access Pattern

```js
const zcql = app.zcql();
const result = await zcql.executeZCQLQuery('SELECT * FROM SampleTable WHERE Name = "Amelia"');
```

Node.js ZCQL also supports DML queries, SQL Join, GroupBy, OrderBy, built-in functions, and OLAP database. [Source: catalyst-nodejs-sdk-cloud-scale-remaining.md]

**Key difference**: Java has explicit `isV2` and `isOLAP` boolean parameters. Node.js documentation does not expose these as separate flags.

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js ZCQL (2 pages)

## Related Concepts

- [[data-store]] — ZCQL operates on Data Store tables
- [[data-store-pagination]] — Alternative to ZCQL for paginated row retrieval
- [[sdk-scopes]] — ZCQL is one of the scope-controlled components
- [[bulk-operations]] — For large-scale operations vs ZCQL for ad-hoc queries

## Evolution

- **v1**: Original query syntax
- **v2**: Enhanced query features (enabled via boolean flag)
- **OLAP**: Added analytical database routing for SELECT queries
