---
title: Catalyst Java SDK — Cloud Scale Remaining + Serverless + General
type: source
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [sdk, java, cloud-scale, serverless, file-store, zcql, cache, connections, search, mail, push-notifications, functions, circuits, appsail]
---

# Catalyst Java SDK — Cloud Scale Remaining + Serverless + General

## Summary

This source covers **21 SDK operation pages** across 11 components that were not in the initial 4 ingests. It completes the Cloud Scale SDK coverage and adds the Serverless SDK operations. Components: General (1), Serverless Functions (1), Circuits (1), AppSail (1), File Store (5), ZCQL (1), Connections (2), Cache (5), Search (1), Mail (1), Push Notifications (2).

[Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Takeaways

1. **File Store** is a simpler predecessor to [[stratus]] — 5 operations via `ZCFile` → `ZCFolder`. Max 100MB upload, 1GB dev storage. Being superseded by Stratus.
2. **ZCQL** has 1 SDK method (`executeQuery`) but supports v2 queries and OLAP database routing via boolean flags.
3. **Cache** uses segments (`ZCSegment`) with key-value pairs. Default expiry: 48 hours. CRUD via `putCacheValue`/`getCacheValue`/`updateCacheValue`/`deleteCacheValue`.
4. **Connections** is internal-only (Functions/AppSail) — manages OAuth tokens to Zoho services automatically via `ZCConnections`.
5. **Search** queries indexed columns across multiple tables using pattern matching (`ZCSearch`).
6. **Mail** sends emails with up to 10 recipients, 5 CC, 5 BCC, 5 attachments (15MB max) via `ZCMail`.
7. **Push Notifications** supports web (50 users/call) and mobile (Android + iOS) separately. Mobile requires registered app with appID.
8. **Execute Function** uses `ZCatalystFunction.getInstance().getFunctionInstance(id).executeFunction(json)` — simple JSON in/out.
9. **Circuits** orchestrate sequences of function executions with abort capability. Not available in EU, AU, IN, JP, SA, CA.
10. **AppSail** requires implementing `AuthHeaderProvider` interface for SDK init. Supports javax.servlet (≤v4) and jakarta.servlet (≥v5). Maven artifact: `com.zoho.catalyst:java-sdk:1.15.0`.

## SDK Method Reference

| Component | Operations | Key Classes | Scope |
|-----------|-----------|-------------|-------|
| General | getProject | ZCProject | — |
| Functions | executeFunction | ZCatalystFunction | — |
| Circuits | execute, getExecutionDetails, abortExecution | ZCCircuit, ZCCircuitDetails | Admin |
| AppSail | init (AuthHeaderProvider) | CatalystSDK | — |
| File Store | getFolderInstance, getFolder, uploadFile, downloadFile, deleteFile | ZCFile, ZCFolder | — |
| ZCQL | executeQuery(query, isV2, isOLAP) | ZCQL | Admin/User |
| Connections | getInstance, getConnectionCredentials | ZCConnections | — |
| Cache | getSegmentInstance, getCacheValue/Object, putCacheValue/Object, updateCacheValue, deleteCacheValue/Object | ZCCache, ZCSegment, ZCCacheObject | — |
| Search | executeSearchQuery | ZCSearch, ZCSearchDetails | — |
| Mail | sendMail | ZCMail, ZCMailContent | — |
| Push Notifications | notifyUser (web), sendAndroidPushNotification, sendIOSPushNotification | ZCWebNotification, ZCMobileNotification, ZCPush | — |

## Entities Mentioned

- [[zoho-catalyst]] — Platform (updated with new components)
- [[zcfile]] — File Store entry point class (new)
- [[zcfolder]] — Folder class for file operations (new)
- [[zccache]] — Cache entry point class (new)
- [[zcsegment]] — Cache segment class (new)

## Concepts Introduced

- [[file-store]] — Predecessor to Stratus, simpler file storage (new)
- [[zcql]] — Catalyst's query language for Data Store (new)
- [[cache]] — In-memory key-value cache with segments (new)
- [[connections-sdk]] — OAuth token management for Zoho service integrations (new)
- [[catalyst-search]] — Pattern-based search across indexed columns (new)
- [[catalyst-mail]] — Email sending from Catalyst apps (new)
- [[push-notifications]] — Web and mobile push notification delivery (new)
- [[circuits]] — Task orchestration / workflow automation (new)
- [[appsail]] — Managed web service deployment platform (new)

## Open Questions

1. **NoSQL SDK** — Pages would not load. Is NoSQL SDK available in Java v1?
2. **File Store vs Stratus** — When will File Store be deprecated?
3. **Cache segment limits** — Max number of segments? Max keys per segment?
4. **Connections** — Which Zoho services are available as "Default Services"?
