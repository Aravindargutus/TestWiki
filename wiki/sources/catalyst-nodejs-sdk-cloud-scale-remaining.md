---
title: Catalyst Node.js SDK v2 — Cloud Scale Remaining
type: source
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-nodejs-sdk-cloud-scale-remaining.md]
tags: [nodejs, sdk, v2, cloud-scale, file-store, cache, connections, search, zcql, mail, push-notifications]
---

# Catalyst Node.js SDK v2 — Cloud Scale Remaining

## Summary

Covers 23 SDK pages for 7 Cloud Scale components: File Store (6), Cache (6), Connections (2), Search (2), ZCQL (2), Mail (2), Push Notifications (3). Each follows the pattern: get component instance → perform operations → handle promise results.

## Key Takeaways

### File Store (6 pages)
- **Instance**: `app.filestore()` → `filestore.folder(folderId)`
- **Ops**: `folder.uploadFile({code, name})`, `folder.downloadFile(fileId)`, `folder.deleteFile(fileId)`, `folder.getDetails()`
- **Note**: Stratus is the recommended upgrade path (Early Access)

### Cache (6 pages)
- **Instance**: `app.cache()` → `cache.segment()` or `cache.segment(segmentId)`
- **CRUD**: `segment.put(key, value)`, `segment.getValue(key)`, `segment.update(key, newValue)`, `segment.delete(key)`

### Connections (2 pages)
- **Instance**: `app.connections()` → `connections.getConnector('ConnectorName')`
- **Credential**: `await credential.getAccessToken()`
- **Restriction**: Only within Functions and AppSail, not third-party apps

### Search (2 pages)
- **Instance**: `app.search()`
- **Query**: `search.executeSearchQuery({ search: 'pattern*', search_table_columns: { TableName: ['Column'] } })`
- Returns matched rows grouped by table name

### ZCQL (2 pages)
- **Instance**: `app.zcql()`
- **Query**: `zcql.executeZCQLQuery('SELECT * FROM Table WHERE ...')`
- Supports DML queries, SQL Join, GroupBy, OrderBy, built-in functions, OLAP database

### Mail (2 pages)
- **Instance**: `app.email()`
- **Send**: `email.sendMail({ from_email, to_email: [...], subject, content })`

### Push Notifications (3 pages)
- **Web**: `app.pushNotification().sendWebNotification({ message, recipients: [...] })`
- **Mobile**: `app.pushNotification().mobile("appId")` → `.sendAndroidNotification(obj, recipient)` / `.sendIOSNotification(obj, recipient)`
- Prerequisites: Catalyst Authentication enabled, app registration, user opt-in

## Entities Mentioned

- [[zoho-catalyst]], [[zcatalyst-sdk-node]]

## Concepts Introduced

- [[file-store]], [[cache]], [[connections-sdk]], [[catalyst-search]], [[zcql]], [[catalyst-mail]], [[push-notifications]]

[Source: catalyst-nodejs-sdk-cloud-scale-remaining.md]
