---
title: Catalyst Java SDK — Stratus
type: source
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-stratus.md]
tags: [catalyst, java-sdk, stratus, object-storage, cloud-scale]
---

# Catalyst Java SDK — Stratus

## Summary

The Stratus section of the Catalyst Java SDK provides comprehensive object storage capabilities through 17 sub-pages covering bucket and object lifecycle management. Stratus organizes data as **Objects** stored in **Buckets**, each with secure URLs and customizable permissions. The SDK exposes three main classes — [[zcstratus]], [[zcbucket]], and [[zcobject-stratus]] — plus a [[transfer-manager]] utility for large files. [Source: catalyst-java-sdk-stratus.md]

## Key Takeaways

1. **Three-tier class hierarchy**: `ZCStratus` → `ZCBucket` → `ZCObject` mirrors the storage model. Most operations happen at the bucket level.
2. **Instance pattern continues**: `getInstance()` and `bucketInstance()` create local references without server calls, consistent with [[instance-objects]].
3. **Admin scope dominance**: Nearly every Stratus operation requires [[sdk-scopes|Admin scope]] — far more than Data Store or Authentication.
4. **Multiple upload strategies**: Stream, string, options, zip-extract, multipart (manual or Transfer Manager), and [[presigned-urls]]. Choice depends on object size and auth context.
5. **[[multipart-uploads]]** support 1–1000 parts with 50MB minimum per part. Parallel upload via `CompletableFuture` demonstrated.
6. **[[transfer-manager]]** wraps both upload and download of large objects. `putObjectAsParts()` is a one-call wrapper; individual methods recommended for >2GB.
7. **[[presigned-urls]]** enable unauthenticated access with configurable expiry (30s–7 days) and active-from time. Available for both GET and PUT.
8. **[[versioning]]**: With versioning enabled, each upload gets a unique `versionId`. Without it, `setOverwrite("true")` is needed. Rename/move operations are blocked when versioning is enabled.
9. **Scheduled deletes**: The `ttl` parameter allows scheduling deletions (minimum 60 seconds).
10. **Pagination pattern**: Same continuation-token pattern as [[data-store-pagination]] — used in `listPagedObjects()` and `listPagedVersions()`.

## Method Reference

### General Stratus Operations (ZCStratus)

| Method | Scope | Returns | Description |
|--------|-------|---------|-------------|
| `ZCStratus.getInstance()` | — | ZCStratus | Create component instance (no server call) |
| `headBucket(name, throwErr)` | — | Boolean | Check bucket existence/permissions |
| `listBuckets()` | Admin | List\<ZCBucket\> | Get all buckets in the project |

### Bucket Operations (ZCBucket)

| Method | Scope | Returns | Description |
|--------|-------|---------|-------------|
| `stratus.bucketInstance(name)` | — | ZCBucket | Create bucket reference (no server call) |
| `getDetails()` | — | ZCBucket | Get bucket details |
| `getCors()` | Admin | List\<ZCStratusCorsResponse\> | Get CORS config |
| `listPagedObjects(options)` | Admin | ZCPagedObjectResponse | Paginated object listing |
| `listIterableObjects(options)` | Admin | Iterable\<List\<ZCObject\>\> | Iterable object listing |
| `headObject(key, versionId, throwErr)` | Admin | Boolean | Check object existence |
| `getObject(key)` | — | InputStream | Download object |
| `getObject(key, options)` | — | InputStream | Download with version/range options |
| `putObject(key, stream)` | — | Boolean | Upload as stream |
| `putObject(key, string)` | — | Boolean | Upload as string |
| `putObject(key, stream, options)` | — | Boolean | Upload with TTL/overwrite/meta |
| `putZipObject(key, stream, options)` | — | JSONObject | Upload and extract zip (async, returns taskId) |
| `initiateMultipartUpload(key)` | — | ZCInitiateMultipartUpload | Start multipart upload |
| `uploadPart(key, uploadId, part, partNumber)` | — | Boolean | Upload one part |
| `getMultipartUploadSummary(key, uploadId)` | — | ZCMultipartObjectSummary | Get upload progress |
| `completeMultipartUpload(key, uploadId)` | — | Boolean | Finalize multipart |
| `unzipObject(key, destination)` | Admin | ZCStratusZipExtractResponse | Extract existing zip (async) |
| `copyObject(key, destination)` | Admin | JSONObject | Copy object within bucket |
| `renameObject(key, destination)` | Admin | JSONObject | Rename or move object |
| `deleteObject(key, versionId, ttl)` | Admin | JSONObject | Delete single object |
| `deleteObjects(request)` | Admin | JSONObject | Delete multiple objects |
| `truncate()` | Admin | JSONObject | Delete all objects in bucket |
| `deletePath(path)` | Admin | JSONObject | Delete all objects in path |
| `generatePreSignedUrl(key, action)` | Admin | JSONObject | Generate presigned URL |
| `generatePreSignedUrl(key, action, expiry, activeFrom)` | Admin | JSONObject | Presigned URL with time options |
| `getObjectInstance(key)` | — | ZCObject | Create object reference |

### Object Operations (ZCObject)

| Method | Scope | Returns | Description |
|--------|-------|---------|-------------|
| `listPagedVersions(maxVersion, nextToken)` | Admin | ZCObjectVersions | Paginated version listing |
| `listIterableVersions(maxVersion)` | Admin | Iterable\<List\<ZCVersionDetail\>\> | Iterable version listing |
| `getDetails()` | Admin | ZCObject | Get latest version details |
| `getDetails(versionId)` | Admin | ZCObject | Get specific version details |
| `putMeta(metaMap)` | Admin | JSONObject | Add/replace object metadata |
| `getUnzipStatus(key, taskId)` | Admin | JSONObject | Check zip extraction status |

### Transfer Manager (ZCTransferManager)

| Method | Returns | Description |
|--------|---------|-------------|
| `ZCTransferManager.getInstance(bucket)` | ZCTransferManager | Create TM instance |
| `getIterableObject(key, partSize)` | Iterable\<InputStream\> | Download as iterable streams |
| `generatePartDownloaders(key, partSize)` | List\<ZCStratusGetObject\> | Generate download part functions |
| `createMultipartInstance(key)` | ZCMultipartUpload | Create multipart upload |
| `createMultipartInstance(key, uploadId)` | ZCMultipartUpload | Resume multipart upload |
| `putObjectAsParts(key, stream, partSize)` | ZCMultipartObjectSummary | One-call multipart wrapper |

### Transfer Manager Multipart (ZCMultipartUpload)

| Method | Returns | Description |
|--------|---------|-------------|
| `uploadPart(part, partNumber)` | Boolean | Upload one part |
| `getUploadSummary()` | ZCMultipartObjectSummary | Get upload progress |
| `completeUpload()` | Boolean | Finalize upload |

## Entities Mentioned

- [[zoho-catalyst]] — The platform
- [[zcstratus]] — Stratus component entry point class
- [[zcbucket]] — Bucket class for most operations
- [[zcobject-stratus]] — Object class for versions, details, metadata

## Concepts Introduced

- [[stratus]] — Object storage service (Buckets + Objects)
- [[presigned-urls]] — Temporary authenticated URLs for unauthenticated access
- [[multipart-uploads]] — Splitting large objects into parts for upload
- [[transfer-manager]] — SDK utility for large file upload/download
- [[versioning]] — Multiple versions of same object with unique versionIds

## Open Questions

1. What is the maximum object size for non-multipart uploads?
2. How do bucket permissions (custom permissions) map to SDK scopes?
3. Is there a way to create buckets via the SDK, or only via console?
4. How does the `cachedUrl` on objects work — is there a CDN layer?
5. What is the relationship between Stratus and File Store — when to use which?
