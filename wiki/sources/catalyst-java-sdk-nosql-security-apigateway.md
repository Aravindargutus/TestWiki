---
title: Catalyst Java SDK — NoSQL, Security Rules & API Gateway
type: source
created: 2026-04-07
updated: 2026-04-07
source_file: raw/catalyst-java-sdk-nosql-security-apigateway.md
tags: [catalyst, java, sdk, nosql, security-rules, api-gateway, cloud-scale]
---

# Catalyst Java SDK — NoSQL, Security Rules & API Gateway

## Summary

This source covers the three remaining gaps in the Catalyst Java SDK wiki: **NoSQL** (10 SDK pages), **Security Rules** (3 help pages), and **API Gateway** (5 help pages). NoSQL is a fully managed non-relational database for document-type data, accessed via `ZCNoSQL.getInstance()`. Security Rules is the default access control mechanism for serverless functions. API Gateway is an optional, paid enhancement to Security Rules that adds routing, API Key auth, and throttling. [Source: catalyst-java-sdk-nosql-security-apigateway.md]

## Key Takeaways

- **NoSQL has 10 SDK pages** covering full CRUD: get metadata, create table instance, construct items, item operations, insert (25 max), update (25 max), fetch (100 max), query table, query index, delete (25 max) [Source: catalyst-java-sdk-nosql-security-apigateway.md]
- **Builder pattern** throughout: `ZCNoSQLItem` uses fluent `withString()`, `withNumber()`, `withList()`, etc. for 15+ data types [Source: catalyst-java-sdk-nosql-security-apigateway.md]
- **Conditional operations**: Insert, update, and delete support conditions via `ZCNoSQLCondition` with 14 operators, functions, and group conditions (AND/OR) [Source: catalyst-java-sdk-nosql-security-apigateway.md]
- **Master-slave consistency**: Fetch and query support `withConsistency(true/false)` — true reads from master, false from slave (cheaper, slight delay) [Source: catalyst-java-sdk-nosql-security-apigateway.md]
- **Pagination**: Query operations return `getStartKey()` for next page, pass via `withStartKey()` [Source: catalyst-java-sdk-nosql-security-apigateway.md]
- **Security Rules** is the default access control — auto-created JSON for HTTP methods and auth. Disabled when API Gateway enabled. [Source: catalyst-java-sdk-nosql-security-apigateway.md]
- **API Gateway** is optional & paid — adds API Key auth, custom routing with regex, throttling (general + IP-based with sliding window), and web client support. Hard limit: 1000 APIs/project in dev. [Source: catalyst-java-sdk-nosql-security-apigateway.md]

## SDK Method Reference — NoSQL

| Operation | Method | Key Details |
|-----------|--------|-------------|
| Get table metadata (single) | `ZCNoSQL.getInstance().getTable(id/name)` | By table ID or name |
| Get all table metadata | `ZCNoSQL.getInstance().getAllTables()` | Returns List<ZCNoSQLTable> |
| Create table instance | `ZCNoSQL.getInstance().getTableInstance(id/name)` | No server call (dummy) |
| Construct item | `new ZCNoSQLItem()` / `ZCNoSQLItem.fromJSON(json)` | Builder pattern |
| Insert items | `table.insert(item)` / `table.getInsertHelper(item).insert()` | Max 25 bulk |
| Update items | `table.update(item, updateOp)` / `table.getUpdateHelper(...)` | Max 25 bulk |
| Fetch items | `table.fetch(key)` / `table.getFetchHelper(key).fetch()` | Max 100 bulk |
| Query table | `table.query(partitionKey, forwardScan, limit)` | Max 100 + pagination |
| Query index | `table.queryIndex(indexID, partitionKey, forwardScan, limit)` | Max 100 + pagination |
| Delete items | `table.delete(key)` / `table.getDeleteHelper(key).delete()` | Max 25 bulk |

## Entities Mentioned

- [[zcnosql]] — Entry point class for NoSQL operations
- [[zoho-catalyst]] — The platform providing these features

## Concepts Introduced

- [[nosql]] — Non-relational document-type database service
- [[security-rules]] — Default access control for serverless functions
- [[api-gateway]] — Advanced API management with routing, auth, and throttling

## Open Questions

- What are the production limits for NoSQL (table count, item size, throughput)?
- How does NoSQL TTL attribute work in practice?
- What are the API Gateway pricing details?
- Can Security Rules and API Gateway coexist or is it strictly either/or?
