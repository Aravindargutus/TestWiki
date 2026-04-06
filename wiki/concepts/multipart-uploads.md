---
title: Multipart Uploads
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-stratus.md]
tags: [catalyst, stratus, multipart, upload, large-files]
---

# Multipart Uploads

## Definition

Multipart uploads allow splitting a large object into multiple parts (1–1000) for faster, parallel upload to [[stratus]]. Each part has a minimum size of 50 MB. The process has three phases: initiate, upload parts, complete. [Source: catalyst-java-sdk-stratus.md]

## Key Aspects

### Three-Phase Process

1. **Initiate** — `bucket.initiateMultipartUpload(key)` → returns `uploadId`
2. **Upload Parts** — `bucket.uploadPart(key, uploadId, part, partNumber)` for each part (1–1000). Parts can be uploaded in any order and in parallel.
3. **Complete** — `bucket.completeMultipartUpload(key, uploadId)` → combines all parts

### Monitoring
- `bucket.getMultipartUploadSummary(key, uploadId)` returns `ZCMultipartObjectSummary`
- Summary includes: key, uploadId, status, list of parts with uploadedAt, partNumber, size

### Parallel Upload Pattern
The SDK documentation demonstrates parallel uploads using Java's `CompletableFuture` and `ExecutorService.newFixedThreadPool(4)`:
1. Calculate number of parts: `Math.ceil(fileSize / partSize)`
2. Create `CompletableFuture.runAsync()` for each part
3. Wait with `CompletableFuture.allOf().get()`
4. Call `completeMultipartUpload()`

### Constraints
- **Part count:** 1–1000
- **Part size:** Minimum 50 MB per part
- **Part ordering:** Parts don't need to be uploaded sequentially but are combined in `partNumber` order

### Alternative: Transfer Manager
The [[transfer-manager]] provides a simpler API via `ZCMultipartUpload`:
- `transferManager.createMultipartInstance(key)` — new upload
- `transferManager.createMultipartInstance(key, uploadId)` — resume existing
- `multipart.uploadPart(part, partNumber)` — simpler signature
- `multipart.completeUpload()` — finalize

Or one-call wrapper: `transferManager.putObjectAsParts(key, stream, partSize)` (not recommended for >2GB).

## Sources

- [[catalyst-java-sdk-stratus]] — Upload Object section

## Related Concepts

- [[stratus]] — The object storage service
- [[transfer-manager]] — Higher-level abstraction for multipart
- [[bulk-operations]] — Async operations in Data Store (analogous pattern)

## Evolution

- Multipart uploads in Stratus follow the same conceptual model as AWS S3 multipart uploads: initiate → upload parts → complete.
