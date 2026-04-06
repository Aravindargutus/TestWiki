---
title: Transfer Manager
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-stratus.md]
tags: [catalyst, stratus, transfer-manager, large-files]
---

# Transfer Manager

## Definition

The Transfer Manager (`ZCTransferManager`) is a utility class in the Catalyst Java SDK that simplifies large file upload and download operations in [[stratus]] by handling chunking, part management, and reassembly. [Source: catalyst-java-sdk-stratus.md]

## Key Aspects

### Initialization
```java
ZCTransferManager transferManager = ZCTransferManager.getInstance(bucket);
```
- **Package:** `com.zc.component.stratus.transfer`
- Takes a [[zcbucket]] instance as input
- Follows the [[instance-objects]] pattern

### Download Operations

| Method | Returns | Description |
|--------|---------|-------------|
| `getIterableObject(key, partSize)` | Iterable\<InputStream\> | Stream parts lazily via iterator |
| `generatePartDownloaders(key, partSize)` | List\<ZCStratusGetObject\> | Get list of download functions to call manually |

**Pattern:** Object is split into byte ranges based on `partSize` (in MB). Each range is downloaded as a separate InputStream. Parts are written to disk sequentially via `Files.write(..., APPEND)`.

### Upload Operations

**Via ZCMultipartUpload:**
| Method | Returns | Description |
|--------|---------|-------------|
| `createMultipartInstance(key)` | ZCMultipartUpload | New multipart upload |
| `createMultipartInstance(key, uploadId)` | ZCMultipartUpload | Resume existing upload |
| `multipart.uploadPart(part, partNumber)` | Boolean | Upload one part |
| `multipart.getUploadSummary()` | ZCMultipartObjectSummary | Check progress |
| `multipart.completeUpload()` | Boolean | Finalize |

**One-call wrapper:**
| Method | Returns | Description |
|--------|---------|-------------|
| `putObjectAsParts(key, stream, partSize)` | ZCMultipartObjectSummary | Complete multipart in one call |

> Note: For objects > 2GB, use individual methods instead of `putObjectAsParts()`.

### Transfer Manager vs Direct Multipart

| Aspect | Direct (bucket methods) | Transfer Manager |
|--------|------------------------|------------------|
| Initiate | `bucket.initiateMultipartUpload()` | `tm.createMultipartInstance()` |
| Upload | `bucket.uploadPart(key, uploadId, part, num)` | `multipart.uploadPart(part, num)` |
| Summary | `bucket.getMultipartUploadSummary(key, uploadId)` | `multipart.getUploadSummary()` |
| Complete | `bucket.completeMultipartUpload(key, uploadId)` | `multipart.completeUpload()` |
| One-call | N/A | `tm.putObjectAsParts()` |
| Download | N/A | `tm.getIterableObject()` / `tm.generatePartDownloaders()` |

The Transfer Manager abstracts away the `uploadId` and `key` tracking — they're stored in the `ZCMultipartUpload` instance.

## Sources

- [[catalyst-java-sdk-stratus]] — Download and Upload Transfer Manager sections

## Related Concepts

- [[multipart-uploads]] — The underlying upload mechanism
- [[stratus]] — The object storage service
- [[zcbucket]] — The bucket class that TM wraps

## Evolution

- Transfer Manager provides the same abstraction pattern seen in AWS SDK's `TransferManager` — a higher-level API that handles chunking logic internally.
