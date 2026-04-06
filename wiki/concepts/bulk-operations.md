---
title: Bulk Operations
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-data-store.md]
tags: [catalyst, data-store, bulk, async, stratus]
---

# Bulk Operations

## Definition

Asynchronous job-based operations for reading, writing, and deleting large volumes of data in the Catalyst [[data-store]]. Bulk read and write operate via background jobs with status polling; bulk delete is synchronous but batched. [Source: catalyst-java-sdk-data-store.md]

## Key Aspects

### Bulk Read
- Max **200,000 rows** per job
- Generates a CSV file with results
- Entry: `ZCDataStoreBulk.getInstance().getBulkReadInstance()`
- Supports query details and callback details
- Poll status via `getBulkReadJobStatus(jobId)`

### Bulk Write
- Max **100,000 rows** per job
- **Requires CSV in [[stratus]]** — upload first, then reference via bucketName/objectKey/versionID
- Three modes: insert, update, upsert
- Update/upsert require a column ID as the match key
- Entry: `ZCDataStoreBulk.getInstance().getBulkWriteInstance()`
- Poll status via `getBulkWriteJobStatus(jobId)`

### Bulk Delete
- Max **200 rows** per call (synchronous, not a job)
- Pass ArrayList of ROWIDs to `deleteRows()`
- Accessed via `getTableInstance("name").deleteRows(rowIdList)`

## Key Classes

| Class | Purpose |
|-------|---------|
| `ZCDataStoreBulk` | Entry point for bulk services |
| `ZCBulkReadServices` | Bulk read job management |
| `ZCBulkWriteServices` | Bulk write job management |
| `ZCBulkReadDetails` | Read job configuration |
| `ZCBulkWriteDetails` | Write job configuration |
| `ZCBulkQueryDetails` | Query filter for bulk read |
| `ZCBulkCallbackDetails` | Callback config for bulk read |
| `ZCBucketObjectDetails` | Stratus file reference for write |
| `ZCBulkResult` | Job result with getJobId() |

## Sources

- [[catalyst-java-sdk-data-store]] — Bulk Read, Bulk Write, and Bulk Delete sections

## Related Concepts

- [[data-store]] — The database these operations act on
- [[stratus]] — Required for bulk write (CSV source)
- [[data-store-pagination]] — Alternative for moderate-scale reads
