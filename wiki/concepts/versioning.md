---
title: Versioning
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-stratus.md]
tags: [catalyst, stratus, versioning, objects]
---

# Versioning

## Definition

Versioning is an optional bucket-level setting in [[stratus]] that enables storing multiple versions of the same object. When enabled, each upload creates a new version with a unique `versionId` rather than overwriting the existing object. [Source: catalyst-java-sdk-stratus.md]

## Key Aspects

### Behavior With Versioning Enabled
- Each upload of the same object key creates a new version with a unique `versionId`
- All versions are stored in the bucket
- `getDetails()` returns only the latest version by default
- Use `getDetails("versionId")` or `"topVersion"` for specific versions
- `listPagedVersions()` / `listIterableVersions()` enumerate all versions
- `headObject(key, versionId, throwErr)` can check specific version existence

### Behavior Without Versioning
- Multiple writes to same key **overwrite** — only latest stored
- Must use `setOverwrite("true")` in `ZCPutObjectOptions` to allow overwrites
- Default `setOverwrite` is `"false"` — meaning duplicate writes are rejected without overwrite flag

### Versioning + Delete
- Deleting without `versionId` removes ALL versions of the object
- Deleting with specific `versionId` removes only that version

### Operations Blocked by Versioning
- `renameObject()` — Cannot rename in versioned buckets
- Move operations — Blocked (same method as rename)

### Versioning Toggle Behavior
If versioning was enabled then disabled:
- **Download**: The principal (first) object is downloaded by default. To get the latest, pass `"topVersion"` as versionId.

## Sources

- [[catalyst-java-sdk-stratus]] — Referenced across upload, download, delete, rename, and version listing pages

## Related Concepts

- [[stratus]] — The storage service
- [[zcobject-stratus]] — Object class with version methods
- [[data-store-pagination]] — Similar token-based listing pattern for version pagination
