---
title: NoSQL
type: concept
created: 2026-04-07
updated: 2026-04-17
sources: [catalyst-java-sdk-nosql-security-apigateway.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, nosql, cloud-scale, database, storage, partition-key, indexing]
---

# NoSQL

## Definition

Catalyst Cloud Scale NoSQL is a fully managed, non-relational database that supports document-type data storage in key-value pair based Custom JSON format. It offers schema flexibility, horizontal scalability, and master-slave replication. Accessed via `ZCNoSQL.getInstance()` → `ZCNoSQLTable`. [Source: catalyst-java-sdk-nosql-security-apigateway.md]

## Platform Overview

> Source: [NoSQL Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/nosql/) — Introduction, Components, Create and Manage Tables, Indexing, Working with Data, Query Search, Third-Party Migration.

### Basic Components (Terminology)
| Term | Relational Equivalent | Notes |
|---|---|---|
| **Table** | Table | Fundamental storage unit |
| **Item** | Row / Record | Collection of attributes; **max 400 KB per item** |
| **Attribute** | Field / Column | Each holds a typed value |
| **Data** | Table contents | Stored in **Catalyst Custom JSON** format |

### Table Keys
| Key | Purpose |
|---|---|
| **Partition Key** | Determines the logical partition (shard) an item is stored in via hash function. **Required.** |
| **Sort Key** | Optional. Sorts items within a partition; enables range queries |
| **Simple Primary Key** | Partition key alone (must be unique per item) |
| **Composite Primary Key** | Partition key + sort key together (partition key values may repeat; sort key must be unique within a partition) |
| **Additional Sort Keys** | Up to **5 per table**; pair with the partition key as alternative composite keys (similar to local indexing) |

### Time to Live (TTL)
- Configurable attribute holding **expiration timestamp in Unix epoch format** (seconds)
- A built-in Catalyst scheduler runs **every 24 hours** and permanently deletes expired items
- TTL removal cascades to any indexes
- TTL attribute can be updated or removed per-item

### Indexing (Secondary Indexes)
Secondary indexes allow querying by attributes other than the main partition/sort key.

- **Global by design** — an index spans all partitions of the base table
- **Max 20 indexes per table**
- Each index has its own partition key + sort key (can differ from main table)
- **Auto-maintained** on insert/update/delete of base table
- Items missing the index's partition key attribute are skipped from indexing

**Three Index Types** (attribute projection modes):
| Type | Projected Attributes |
|---|---|
| **All Attributes** | Every attribute of the table |
| **Only Keys** | Partition key + sort key + additional sort keys |
| **Specific Attributes** | User-selected subset |

### Catalyst Custom JSON Format
Attribute values are wrapped with a data type marker. Examples: `{"S": "Honolulu"}` (String), `{"N": 4960}` (Number), `{"L": [...]}` (List), `{"M": {...}}` (Map). Per-SDK construction syntax varies; canonical form is the Custom JSON used by the REST API.

### Console Capabilities
- Create/configure/delete tables
- Define partition/sort/additional sort keys
- Configure TTL attribute
- Create & manage indexes (20 max)
- Add / edit / query data
- **Third-party migration** from external NoSQL databases
- Query Search UI

### Access Paths
Java SDK, Node.js SDK, Python SDK, REST API.

### Data Store vs NoSQL (Official Guidance)
| Choose [[data-store]] if... | Choose NoSQL if... |
|---|---|
| Relational architecture with related data | Independent, unrelated data points |
| Structured tabular data | Semi/unstructured, disparate JSON data |
| Uniform, known schema | Flexible, evolving schema |
| SQL querying needed | Primarily JSON + multi-type values |
| ACID > horizontal scaling | Horizontal scaling > ACID |
| Read-heavy | Write-heavy, distributed clusters beneficial |

## Key Aspects

- **Data model**: Document-type, key-value pairs in Custom JSON format. Partition key required, sort key optional.
- **Supported data types**: String, Number, Int, BigInteger, Short, Float, Double, Long, Binary, Boolean, Null, StringSet, NumberSet, BinarySet, ByteBufferSet, List, Map, JSON
- **CRUD operations**: 10 SDK methods — metadata (2), instance (1), construct (1), item ops (1), insert (1), update (1), fetch (1), query table (1), query index (1), delete (1)
- **Bulk limits**: Insert/Update/Delete max 25 items; Fetch/Query max 100 items per operation
- **Conditional operations**: All mutating operations support conditions with 14 operators (equals, contains, begins_with, between, in, greater_than, etc.) and group operators (AND, OR)
- **Consistency**: `withConsistency(true)` reads from master; `false` reads from slave (cheaper, slight delay)
- **Pagination**: Query returns `getStartKey()` for next page; pass via `withStartKey()`
- **Indexing**: Supports secondary indexes queryable via `queryIndex()` — allows alternate queries without main table primary keys

## Data Store vs NoSQL

| Aspect | Data Store | NoSQL |
|--------|-----------|-------|
| Architecture | Relational | Non-relational |
| Data structure | Structured rows/columns | Semi-structured/unstructured JSON |
| Schema | Fixed, designed in advance | Flexible, items can vary |
| Query language | ZCQL (SQL-like) | Key-based conditions |
| Priority | ACID compliance | Horizontal scalability |
| Best for | Read-heavy, consistent schema | Write-heavy, flexible schema |
| Identifier | ROWID | Partition key + optional sort key |

## Node.js SDK Access Pattern

Promise-based API via `app.nosql()`. [Source: catalyst-nodejs-sdk-cloud-scale-core.md]

```js
const nosql = app.nosql();
const table = nosql.table('MyTable');

// Build items
const item = nosql.item();
item.put('pk', 'partition_value');
item.put('sk', 'sort_value');
item.put('name', 'John');
item.put('age', 30);

// CRUD
await table.insertItems([item]);
await table.updateItems([updatedItem]);
const result = await table.fetchItems({ partition_key_value: 'pk', sort_key_value: 'sk' });
await table.deleteItems([{ partition_key_value: 'pk', sort_key_value: 'sk' }]);

// Query
const rows = await table.queryTable({ partition_key: { value: 'pk' }, sort_key: { value: 'sk', condition: 'BEGINS_WITH' } });
const indexed = await table.queryIndex('IndexName', { partition_key: { value: 'pk' } });
```

**Key differences**: Java uses a fluent builder with typed methods (`withString`, `withNumber`, etc.), while Node.js `item.put()` infers types dynamically. Java supports 14 conditional operators with group conditions; Node.js uses JSON condition objects.

## Sources

- [[catalyst-java-sdk-nosql-security-apigateway]] — All 10 NoSQL SDK pages
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js NoSQL SDK (10 pages)

## Related Concepts

- [[data-store]] — Relational counterpart
- [[zcql]] — Query language for Data Store (not applicable to NoSQL)
- [[cache]] — Another Cloud Scale storage option (in-memory, ephemeral)

## Evolution

_Initially flagged as "failed to load" in Java SDK. Resolved via `docs-ea.catalyst.zoho.com`. Node.js SDK v2 provides matching 10-page coverage with simplified item construction via `nosql.item().put()`._
