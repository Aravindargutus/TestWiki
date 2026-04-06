---
title: ZCSegment
type: entity
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [sdk, java, cache, class]
---

# ZCSegment

## Overview

`ZCSegment` is the cache segment class in the [[cache]] component. All key-value CRUD operations are performed through a segment instance. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Facts

- Package: `com.zc.component.cache.ZCSegment`
- Obtained from [[zccache]] via `getSegmentInstance(long id)`
- Segments are named groupings of key-value pairs

## Methods

| Method | Description |
|--------|-------------|
| `getCacheValue(String key)` | Get value as String |
| `getCacheObject(String key)` | Get value as ZCCacheObject (includes key, value, expiry) |
| `putCacheValue(key, value)` | Insert key-value (48h default expiry) |
| `putCacheValue(key, value, expiryHours)` | Insert with custom expiry |
| `putCacheObject(ZCCacheObject)` | Insert via object with all properties |
| `deleteCacheValue(String key)` | Delete by key (irreversible) |
| `deleteCacheObject(ZCCacheObject)` | Delete via object |

## Appearances

- [[catalyst-java-sdk-cloud-scale-remaining]] — Cache SDK operations

## Relationships

- Accessed via: [[zccache]]
- Uses: `ZCCacheObject` as data holder
- Part of: [[cache]] component

## Notes

- If a key already exists during `putCacheValue`, the existing value is replaced.
- Deleted entries cannot be restored.
