---
title: Stratus
type: concept
created: 2026-04-05
updated: 2026-04-17
sources: [catalyst-java-sdk-stratus.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, stratus, object-storage, cloud-scale, cors, permissions]
---

# Stratus

## Definition

Stratus is Catalyst's Cloud Scale object storage service. Data of any format is stored as **Objects** in containers called **Buckets**. Each bucket and object has a secure URL. Custom permissions can be applied per object. [Source: catalyst-java-sdk-stratus.md]

## Platform Overview

> Source: [Stratus Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/stratus/) — Buckets, Stratus Configurations (CORS, Caching), Permissions. The SDK material below covers programmatic access; this section covers platform-level features accessed via the console.

### Bucket URL (per Data Center)

Each bucket gets a globally unique URL auto-generated at creation. Access is gated by the bucket's permission template.

| DC | Development | Production |
|---|---|---|
| US | `https://<bucket>-development.zohostratus.com` | `https://<bucket>.zohostratus.com` |
| EU | `https://<bucket>-development.zohostratus.eu` | `https://<bucket>.zohostratus.eu` |
| IN | `https://<bucket>-development.zohostratus.in` | `https://<bucket>.zohostratus.in` |
| AU | `https://<bucket>-development.zohostratus.com.au` | `https://<bucket>.zohostratus.com.au` |
| CA | `https://<bucket>-development.zohostratus.ca` | `https://<bucket>.zohostratus.ca` |

### Permission Templates (set at bucket creation)
- **Authenticated** — Only users authenticated on the app's client portal can access objects
- **Public** — Any end-user on the internet can access objects with no auth

Both templates are editable; individual object permissions can also be overridden. Permissions apply to **project users only**, not collaborators. For authenticated access to cached URLs, see [[presigned-urls]] / signed URL flow (must be re-signed every hour via SDK or API).

### Bucket CORS

Whitelist domains that need cross-origin access to objects (e.g., [[appsail]] or third-party frontends). Configured per bucket in the console's **Configurations** tab.

- Per-domain request method: `GET`, `POST`, `PUT`, `DELETE`, `HEAD`
- Configurations migrate from development to production environments
- Requires **Write** permission for Stratus component (Profiles & Permissions)

### Other Bucket Configurations
- **General settings** (including Caching) — configured per bucket
- **Caching URL** — CDN-backed URL for faster delivery (requires URL signing for Authenticated buckets)
- Configurations migrate dev → prod

### Console Capabilities
- Create / delete buckets
- Upload / download / rename / move / copy objects
- Browse object versions (see [[versioning]])
- Generate presigned URLs (see [[presigned-urls]])
- Configure CORS, caching, permissions

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
