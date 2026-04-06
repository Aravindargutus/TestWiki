---
title: ZCCache
type: entity
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [sdk, java, cache, class]
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

## Appearances

- [[catalyst-java-sdk-cloud-scale-remaining]] — Cache SDK operations

## Relationships

- Returns: [[zcsegment]]
- Part of: [[zoho-catalyst]] Cloud Scale components

## Notes

- Follows the [[instance-objects]] pattern.
- Default cache entry expiry: 48 hours.
