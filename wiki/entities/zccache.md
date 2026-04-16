---
title: ZCCache
type: entity
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-cloud-scale-remaining.md]
tags: [sdk, java, nodejs, cache, class]
---

# ZCCache

## Overview

`ZCCache` is the entry point class for [[cache]] operations in the Catalyst Java SDK. It provides access to cache segments and direct cache value updates. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Facts

- Package: `com.zc.component.cache.ZCCache`
- Singleton via `ZCCache.getInstance()`
- Returns [[zcsegment]] instances by ID
- Also supports direct `updateCacheValue()` and `deleteCacheValue()` at cache level

## Methods

| Method | Description |
|--------|-------------|
| `getInstance()` | Get ZCCache singleton |
| `getSegmentInstance(long id)` | Get a segment instance by ID |
| `updateCacheValue(key, value)` | Update a cache key's value |
| `updateCacheValue(key, value, expiryHours)` | Update value and expiry |

## Node.js SDK Equivalent

In Node.js, Cache is accessed via `app.cache()` → `cache.segment(segmentId)`. [Source: catalyst-nodejs-sdk-cloud-scale-remaining.md]

| Java SDK | Node.js SDK |
|----------|-------------|
| `ZCCache.getInstance()` | `app.cache()` |
| `cache.getSegmentInstance(id)` | `cache.segment()` or `cache.segment(segmentId)` |
| `segment.putCacheValue(key, value)` | `segment.put(key, value)` |
| `segment.getCacheValue(key)` | `segment.getValue(key)` |
| `segment.updateCacheValue(key, value)` | `segment.update(key, newValue)` |
| `segment.deleteCacheValue(key)` | `segment.delete(key)` |

**Key difference**: Node.js `cache.segment()` without args returns the default segment. Java requires `getSegmentInstance(id)`. Node.js methods are shorter (`put`/`getValue`/`update`/`delete` vs `putCacheValue`/`getCacheValue` etc.).

## Appearances

- [[catalyst-java-sdk-cloud-scale-remaining]] — Cache SDK operations
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js Cache SDK operations

## Relationships

- Returns: [[zcsegment]]
- Part of: [[zoho-catalyst]] Cloud Scale components

## Notes

- Follows the [[instance-objects]] pattern.
- Default cache entry expiry: 48 hours.
