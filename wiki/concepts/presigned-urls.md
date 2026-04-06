---
title: Presigned URLs
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-stratus.md]
tags: [catalyst, stratus, presigned-url, security, access-control]
---

# Presigned URLs

## Definition

Presigned URLs are secure, time-limited URLs that authenticated users can share with non-authenticated users, granting them temporary authorization to upload or download objects in [[stratus]]. [Source: catalyst-java-sdk-stratus.md]

## Key Aspects

### Generation
- Generated via `bucket.generatePreSignedUrl(key, action)` on [[zcbucket]]
- Requires [[sdk-scopes|Admin scope]]
- Uses the `URL_ACTION` enum: `URL_ACTION.GET` (download), `URL_ACTION.PUT` (upload)

### Time Controls
| Parameter | Description | Default | Min | Max |
|-----------|-------------|---------|-----|-----|
| expiry | URL validity in seconds | 3600 (1h) | 30s | 7 days |
| activeFrom | Timestamp after which URL becomes valid | Immediately | — | 7 days |

### Response Format
```json
{
  "signature": "https://...zohostratus.com/_signed/...",
  "expiry_in_seconds": "100",
  "active_from": "1726492859577"
}
```

### Usage Pattern
1. Server generates presigned URL using SDK (requires Admin scope)
2. URL is shared with unauthenticated client
3. Client uses standard HTTP PUT (upload) or GET (download) with the URL
4. URL expires after configured time

### External HTTP Client Example
Presigned URLs can be consumed using any HTTP client (e.g., OkHttp):
- **Upload**: PUT request with `application/octet-stream` body
- **Download**: GET request, stream response body to file

## Sources

- [[catalyst-java-sdk-stratus]] — Upload and download presigned URL sections

## Related Concepts

- [[stratus]] — The object storage service
- [[sdk-scopes]] — Admin scope required for generation
- [[third-party-authentication]] — Another form of delegated access

## Evolution

- Presigned URLs bridge the gap between server-side SDK authentication and client-side object access, enabling direct browser uploads/downloads without proxying through the server.
