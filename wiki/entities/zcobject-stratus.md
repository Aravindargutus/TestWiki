---
title: ZCObject (Stratus)
type: entity
created: 2026-04-05
updated: 2026-04-16
sources: [catalyst-java-sdk-stratus.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, java-sdk, nodejs-sdk, stratus, class, object]
---

# ZCObject (Stratus)

## Overview

`ZCObject` in Stratus context represents an object instance for performing object-level operations — version listing, detail retrieval, metadata management, and zip extraction status. Created via `bucket.getObjectInstance("key")`. [Source: catalyst-java-sdk-stratus.md]

**Note:** This is distinct from [[zcobject]] (the Data Store entry point class), despite sharing the same class name `ZCObject` in the `com.zc.component.stratus` package.

## Key Facts

- **Package:** `com.zc.component.stratus`
- **Creation:** `bucket.getObjectInstance("sam/out/sample.txt")`
- All methods require [[sdk-scopes|Admin scope]]
- Used for operations that target a specific object (versions, details, meta)

## Methods

| Method | Scope | Returns | Description |
|--------|-------|---------|-------------|
| `listPagedVersions(maxVersion, nextToken)` | Admin | ZCObjectVersions | Paginated version listing |
| `listIterableVersions(maxVersion)` | Admin | Iterable\<List\<ZCVersionDetail\>\> | Iterable version listing |
| `getDetails()` | Admin | ZCObject | Get latest version details |
| `getDetails(versionId)` | Admin | ZCObject | Get specific version details |
| `putMeta(metaMap)` | Admin | JSONObject | Add/replace object metadata |
| `getUnzipStatus(key, taskId)` | Admin | JSONObject | Check zip extraction status |

## Node.js SDK Equivalent

In Node.js, object-level ops are accessed via `bucket.object('key')`. [Source: catalyst-nodejs-sdk-cloud-scale-core.md]

| Java SDK | Node.js SDK |
|----------|-------------|
| `bucket.getObjectInstance(key)` | `bucket.object('key')` |
| `listPagedVersions(max, token)` | `objectIns.listVersions()` |
| `getDetails()` / `getDetails(versionId)` | `objectIns.getDetails()` |
| `putMeta(metaMap)` | `objectIns.putMeta({ key: value })` |

**Same metadata rules apply**: Max 2047 chars, alphanumeric + underscores + whitespace + hyphens. `putMeta()` is destructive (replaces all metadata).

## Appearances

- [[catalyst-java-sdk-stratus]] — Primary documentation source
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js SDK object operations

## Relationships

- Created by [[zcbucket]] via `getObjectInstance()`
- Uses [[versioning]] for version-specific operations

## Notes

- `getDetails()` without versionId returns **only the latest version** when versioning is enabled.
- Use `"topVersion"` as versionId to explicitly get the latest version.
- `putMeta()` is destructive: passing new meta without existing meta **deletes existing metadata**. Always include all metadata in each call.
- Metadata limited to 2047 characters total, alphanumeric + underscores + whitespace + hyphens only.
