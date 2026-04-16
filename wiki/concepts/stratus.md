---
title: Stratus
type: concept
created: 2026-04-05
updated: 2026-04-16
sources: [catalyst-java-sdk-stratus.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, stratus, object-storage, cloud-scale]
---

# Stratus

## Definition

Stratus is Catalyst's Cloud Scale object storage service. Data of any format is stored as **Objects** in containers called **Buckets**. Each bucket and object has a secure URL. Custom permissions can be applied per object. [Source: catalyst-java-sdk-stratus.md]

## Key Aspects

### Storage Model
- **Buckets** — Named containers that hold objects. Each has a Bucket URL.
- **Objects** — Individual data items (files) stored inside buckets. Each has an Object URL and a path-based key (e.g., `sam/out/sample.txt`).
- **Paths** — Objects can be organized in folder-like hierarchies within a bucket.

### SDK Class Hierarchy
```
ZCStratus (component entry point)
  └── ZCBucket (bucket operations + most object operations)
       └── ZCObject (object-level: versions, details, metadata)
```

Also: `ZCTransferManager` for large file operations.

### Operation Categories
1. **General Stratus** — Instance creation, bucket check, bucket listing
2. **Bucket Operations** — Details, CORS, object listing/check, download, upload (5 strategies), extract zip, copy, rename/move, delete (4 variants), presigned URLs
3. **Object Operations** — Instance creation, list versions, get details, put metadata

### Character Restrictions
The following characters (including space) are NOT supported in paths or object names: `"`, `<`, `>`, `#`, `\`, `|`

## Node.js SDK Access Pattern

Promise-based API via `app.stratus()` → `stratus.bucket('name')` → `bucket.object('key')`. [Source: catalyst-nodejs-sdk-cloud-scale-core.md]

```js
const stratus = app.stratus();
const bucket = stratus.bucket('my-bucket');
await bucket.uploadObject('key', fileStream, options);
const stream = await bucket.downloadObject('key');
await bucket.deleteObjects(['key1', 'key2']);

const obj = bucket.object('key');
await obj.putMeta({ tag: 'value' });
const versions = await obj.listVersions();
```

**Key differences**: Node.js provides `bucket.checkObjectAvailability()` (vs Java's `headObject()`), explicit `moveObject()` method, and array-based `deleteObjects()`. Java's multipart upload methods are not separately exposed in Node.js. Metadata rules are identical (2047 char max, destructive puts).

## Sources

- [[catalyst-java-sdk-stratus]] — Complete SDK documentation (17 sub-pages)
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js SDK Stratus (19 pages)

## Related Concepts

- [[presigned-urls]] — Temporary authenticated access to objects
- [[multipart-uploads]] — Splitting large uploads into parts
- [[transfer-manager]] — SDK utility for large file transfer
- [[versioning]] — Multiple versions per object
- [[data-store]] — Catalyst's relational database (different storage paradigm)
- [[instance-objects]] — The getInstance() pattern used throughout
- [[sdk-scopes]] — Admin scope required for most Stratus operations

## Evolution

- Stratus represents the latest storage offering in Catalyst's Cloud Scale tier
- The [[data-store-pagination]] continuation-token pattern is reused in Stratus's `listPagedObjects()` and `listPagedVersions()`
- Stratus relies more heavily on Admin scope than other components
