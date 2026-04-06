---
title: ZCStratus
type: entity
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-stratus.md]
tags: [catalyst, java-sdk, stratus, class]
---

# ZCStratus

## Overview

`ZCStratus` is the entry-point class for Catalyst's Stratus object storage component in the Java SDK. It provides general Stratus operations (bucket listing, availability checks) and serves as the factory for [[zcbucket]] instances. [Source: catalyst-java-sdk-stratus.md]

## Key Facts

- **Package:** `com.zc.component.stratus`
- **Initialization:** `ZCStratus.getInstance()` — no server call
- Follows the [[instance-objects]] pattern used across the SDK
- Sits at the top of the Stratus class hierarchy: `ZCStratus` → [[zcbucket]] → [[zcobject-stratus]]

## Methods

| Method | Scope | Returns | Description |
|--------|-------|---------|-------------|
| `getInstance()` | — | ZCStratus | Static factory, no API call |
| `headBucket(name, throwErr)` | — | Boolean | Check bucket existence and permissions |
| `listBuckets()` | Admin | List\<ZCBucket\> | Return all buckets in the organization |
| `bucketInstance(name)` | — | ZCBucket | Create a bucket reference (no API call) |

## Appearances

- [[catalyst-java-sdk-stratus]] — Primary documentation source

## Relationships

- Part of [[zoho-catalyst]] platform
- Parent of [[zcbucket]] (creates via `bucketInstance()`)
- Analogous to [[zcobject]] (Data Store entry point) in pattern

## Notes

- Unlike Data Store's [[zcobject]] which has `getTable()` (API call) vs `getTableInstance()` (no call), ZCStratus only has `bucketInstance()` which never makes API calls.
- `listBuckets()` returns all buckets in the **organization**, not just the project.
