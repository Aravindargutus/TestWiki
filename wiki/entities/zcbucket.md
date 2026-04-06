---
title: ZCBucket
type: entity
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-stratus.md]
tags: [catalyst, java-sdk, stratus, class, bucket]
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

### Presigned URLs
| Method | Scope | Returns |
|--------|-------|---------|
| `generatePreSignedUrl(key, action)` | Admin | JSONObject |
| `generatePreSignedUrl(key, action, expiry, activeFrom)` | Admin | JSONObject |

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
