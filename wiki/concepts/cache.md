---
title: Cache
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, cache, key-value]
---

# Cache

## Definition

Cache is Catalyst's in-memory key-value storage component, organized into named segments. Each segment holds key-value pairs with configurable expiry times. It provides fast data access for frequently-used values without querying [[data-store]]. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

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

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]

## Related Concepts

- [[data-store]] — Persistent relational storage vs Cache's ephemeral key-value storage
- [[event-functions]] — Cache changes can trigger Event Functions via Event Listeners

## Evolution

- Cache has been a stable Cloud Scale component. No major changes documented.
