---
title: Cache
type: concept
created: 2026-04-06
updated: 2026-04-17
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, cache, key-value, redis, segments]
---

# Cache

## Definition

Cache is Catalyst's in-memory key-value storage component, organized into named segments. Each segment holds key-value pairs with configurable expiry times. It provides fast data access for frequently-used values without querying [[data-store]]. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Platform Overview

> Source: [Cache Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/cache/) — Introduction, Key Concepts, Architecture, Lifecycle.

### Purpose
In-memory component for **sub-millisecond** read/write of ephemeral or frequently-accessed data. Reduces load on primary stores ([[data-store]], [[nosql]]). Runs alongside or independently of main storage.

### Segments (Cache Partitions)
- A **segment** is a named cache unit that groups related cache items
- **Created only from the console** (not via SDK)
- Each segment gets an auto-generated **Segment ID**
- Every project has a **default segment** created automatically; used when Segment ID is omitted in SDK/API calls
- Segment-based organization enables categorization by purpose

### Cache Items (Key-Value Pairs)
Attributes:
| Attribute | Description |
|---|---|
| **Keyname** | Unique identifier for the cache item |
| **Value** | Stored value |
| **Expiration Time** | Validity in **hours** (max 48 hours / 2 days) |

- **Default expiry**: 2 days (48 hours)
- **Max expiry**: 2 days — cannot exceed; can only be *shorter*
- Expired items are **auto-deleted** from memory

### Lifecycle
1. User creates a segment (console) — unique ID assigned in the Redis cluster
2. Cache items created in the segment are internally bound to it
3. All cache items across projects live in a **shared dedicated Redis cluster** (isolation handled by Zoho)
4. Reads look up data by internal identifiers
5. Items remain in-memory until TTL expiry or manual delete

### Backing Technology
Powered by a **Redis cluster** managed by Catalyst (no provisioning/config required by user).

### Access Paths
- **Console**: view/update cache items per segment, see Segment ID
- **SDKs**: Java, Node.js, Python — fetch/insert/update/delete
- **REST API**: `InsertKeyValueinCacheSegment` and related endpoints

### Environments
Cache behavior and data are isolated per [[catalyst-environments]] (development vs production).

## Key Aspects

- **Segment-based organization**: [[zccache]] → [[zcsegment]] → key-value pairs
- **5 SDK operations**: Get segment instance, get/put/update/delete cache values
- **Default expiry**: 48 hours per key-value pair
- **Custom expiry**: Set in hours via `putCacheValue(key, value, expiryHours)`
- **Key collision**: If a key exists during `put`, the value is silently replaced
- **Two access patterns**: Simple string (`getCacheValue`) or rich object (`getCacheObject` with key, value, expiry)
- **Irreversible deletes**: Deleted entries cannot be restored

## SDK Entry Point

```java
ZCCache cache = ZCCache.getInstance();
ZCSegment segment = cache.getSegmentInstance(segmentId);
segment.putCacheValue("Name", "Amelia", 1L); // 1 hour expiry
String name = segment.getCacheValue("Name");
segment.deleteCacheValue("Name");
```

## Node.js SDK Access Pattern

```js
const cache = app.cache();
const segment = cache.segment();         // default segment
const segment = cache.segment(segmentId); // specific segment

await segment.put('Name', 'Amelia');
const value = await segment.getValue('Name');
await segment.update('Name', 'Updated');
await segment.delete('Name');
```

**Key difference**: Node.js `cache.segment()` without args returns the default segment. Methods use shorter names (`put`/`getValue`/`update`/`delete`). [Source: catalyst-nodejs-sdk-cloud-scale-remaining.md]

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js Cache (6 pages)

## Related Concepts

- [[data-store]] — Persistent relational storage vs Cache's ephemeral key-value storage
- [[event-functions]] — Cache changes can trigger Event Functions via Event Listeners

## Evolution

- Cache has been a stable Cloud Scale component. No major changes documented.
