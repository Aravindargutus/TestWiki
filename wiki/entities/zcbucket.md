---
title: ZCBucket
type: entity
created: 2026-04-05
updated: 2026-04-16
sources: [catalyst-java-sdk-stratus.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, java-sdk, nodejs-sdk, stratus, class, bucket]
---

# ZCBucket

## Overview

`ZCBucket` is the central class for Stratus bucket-level operations in the Catalyst Java SDK. It handles the majority of Stratus functionality — object upload, download, listing, deletion, copying, renaming, presigned URL generation, and multipart operations. Created via `stratus.bucketInstance("name")`. [Source: catalyst-java-sdk-stratus.md]

## Key Facts

- **Package:** `com.zc.component.stratus`
- **Creation:** `stratus.bucketInstance("bucketName")` — no server call
- Hosts 25+ methods covering all bucket and object CRUD operations
- Most methods require [[sdk-scopes|Admin scope]]

## Methods

### Bucket Info
| Method | Scope | Returns |
|--------|-------|---------|
| `getDetails()` | — | ZCBucket |
| `getCors()` | Admin | List\<ZCStratusCorsResponse\> |

### Object Listing
| Method | Scope | Returns |
|--------|-------|---------|
| `listPagedObjects(options)` | Admin | ZCPagedObjectResponse |
| `listIterableObjects(options)` | Admin | Iterable\<List\<ZCObject\>\> |
| `headObject(key, versionId, throwErr)` | Admin | Boolean |

### Download
| Method | Scope | Returns |
|--------|-------|---------|
| `getObject(key)` | — | InputStream |
| `getObject(key, options)` | — | InputStream |

### Upload
| Method | Scope | Returns |
|--------|-------|---------|
| `putObject(key, stream)` | — | Boolean |
| `putObject(key, string)` | — | Boolean |
| `putObject(key, stream, options)` | — | Boolean |
| `putZipObject(key, stream, options)` | — | JSONObject (taskId) |

### Multipart Upload
| Method | Scope | Returns |
|--------|-------|---------|
| `initiateMultipartUpload(key)` | — | ZCInitiateMultipartUpload |
| `uploadPart(key, uploadId, part, partNumber)` | — | Boolean |
| `getMultipartUploadSummary(key, uploadId)` | — | ZCMultipartObjectSummary |
| `completeMultipartUpload(key, uploadId)` | — | Boolean |

### Object Management
| Method | Scope | Returns |
|--------|-------|---------|
| `unzipObject(key, destination)` | Admin | ZCStratusZipExtractResponse |
| `copyObject(key, destination)` | Admin | JSONObject |
| `renameObject(key, destination)` | Admin | JSONObject |

### Delete
| Method | Scope | Returns |
|--------|-------|---------|
| `deleteObject(key, versionId, ttl)` | Admin | JSONObject |
| `deleteObjects(request)` | Admin | JSONObject |
| `truncate()` | Admin | JSONObject |
| `deletePath(path)` | Admin | JSONObject |

### Node.js SDK Equivalent Methods

In Node.js, bucket operations are accessed via `stratus.bucket('name')`. All return promises. [Source: catalyst-nodejs-sdk-cloud-scale-core.md]

| Java SDK | Node.js SDK |
|----------|-------------|
| `getDetails()` | `bucket.getDetails()` |
| `getCors()` | `bucket.getCORS()` |
| `listPagedObjects(options)` | `bucket.listObjects()` |
| `headObject(key)` | `bucket.checkObjectAvailability('key')` |
| `getObject(key)` | `bucket.downloadObject('key')` |
| `putObject(key, stream)` | `bucket.uploadObject('key', stream, options)` |
| `putZipObject(key, stream, options)` | `bucket.extractObject('archive.zip', 'dest/')` |
| `copyObject(key, dest)` | `bucket.copyObject('src', 'dest-bucket', 'dest-key')` |
| `renameObject(key, dest)` | `bucket.renameObject('old', 'new')` |
| _N/A_ | `bucket.moveObject('src', 'dest-bucket', 'dest-key')` |
| `deleteObject(key)` | `bucket.deleteObjects(['key1', 'key2'])` |
| `bucket.getObjectInstance(key)` | `bucket.object('key')` |

**Key differences**: Node.js SDK combines delete into a single `deleteObjects()` accepting an array. Node.js adds explicit `moveObject()`. Java's multipart upload methods are not separately exposed in Node.js (handled internally by `uploadObject`).

### Presigned URLs
| Java SDK | Node.js SDK |
|----------|-------------|
| `generatePreSignedUrl(key, action)` | _Not separately documented in Node.js raw source_ |

### Factory
| Method | Scope | Returns |
|--------|-------|---------|
| `getObjectInstance(key)` | — | ZCObject |

## Appearances

- [[catalyst-java-sdk-stratus]] — Primary documentation source

## Relationships

- Created by [[zcstratus]] via `bucketInstance()`
- Parent of [[zcobject-stratus]] (creates via `getObjectInstance()`)
- Uses [[transfer-manager]] via `ZCTransferManager.getInstance(bucket)`

## Notes

- Rename and move are the same method (`renameObject`). Rename changes the name; move changes the path.
- Rename/move operations are **blocked** when [[versioning]] is enabled.
- `truncate()` deletes ALL objects — use with extreme caution.
- Without [[versioning]], `setOverwrite("true")` must be set or duplicate writes are rejected.
