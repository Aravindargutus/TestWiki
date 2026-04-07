---
title: Catalyst Node.js SDK v2 — Cloud Scale Core
type: source
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [nodejs, sdk, v2, cloud-scale, authentication, data-store, nosql, stratus]
---

# Catalyst Node.js SDK v2 — Cloud Scale Core

## Summary

Covers 51 SDK pages across Authentication (11), Data Store (11), NoSQL (10), and Stratus (19). The Node.js SDK uses `app.userManagement()`, `app.datastore()`, `app.nosql()`, and `app.stratus()` to obtain component instances, then performs operations via promise-based methods.

## Key Takeaways

### Authentication (11 pages)
- **Instance**: `app.userManagement()`
- **User lifecycle**: `registerUser(signupConfig, userConfig)` → `getUserDetails(userId)` → `updateUser(userId, config)` → `deleteUser(userId)`
- **Bulk queries**: `getCurrentUser()`, `getAllUsers()`, `getAllOrganizations()`, `getAllUsersOfOrg(orgId)`
- **Security**: `resetPassword()`, `getServerToken()`, `validateUser()`, `enableUser()`/`disableUser()`
- **Dev limit**: 25 users; production: unlimited
- **Response shape**: `{ zuid, zaaid, org_id, status, email_id, first_name, last_name, role_details: { role_name, role_id }, user_type, user_id }`

### Data Store (11 pages)
- **Instance**: `app.datastore()` → `datastore.table('TableName')` or `datastore.table(tableId)`
- **CRUD**: `table.getPagedRows()`, `table.getRow(rowId)`, `table.insertRows([...])`, `table.updateRows([...])`, `table.deleteRow(rowId)`
- **Bulk**: `datastore.bulkRead({table_name})`, `datastore.bulkWrite({table_name, file_id})`, `datastore.bulkDelete({table_name, ids})`
- **Metadata**: `datastore.getAllTables()`, `table.getAllColumns()`
- **Pagination**: Token-based via `maxRows` + `nextToken` params

### NoSQL (10 pages)
- **Instance**: `app.nosql()` → `nosql.table('TableName')`
- **Item construction**: `nosql.item()` + `.put(key, value)` for String, Number, Boolean, List, Map, Set, Null, Binary
- **CRUD**: `table.insertItems([...])`, `table.updateItems([...])`, `table.fetchItems({pk, sk})`, `table.deleteItems([...])`
- **Querying**: `table.queryTable({partition_key, sort_key})`, `table.queryIndex('IndexName', {partition_key})`

### Stratus (19 pages)
- **Instance**: `app.stratus()` → `stratus.bucket('name')` → `bucket.object('key')`
- **Bucket ops**: `checkBucketAvailability()`, `listBuckets()`, `getDetails()`, `getCORS()`
- **Object ops**: `listObjects()`, `uploadObject()`, `downloadObject()`, `copyObject()`, `renameObject()`, `moveObject()`, `deleteObjects()`, `extractObject()`
- **Metadata**: `objectIns.putMeta({...})` (Admin scope required; max 2047 chars)
- **Versioning**: `objectIns.listVersions()`, `objectIns.getDetails()`

## Entities Mentioned

- [[zoho-catalyst]], [[zcatalyst-sdk-node]]

## Concepts Introduced

- [[data-store]], [[nosql]], [[stratus]], [[catalyst-authentication]], [[bulk-operations]], [[data-store-pagination]], [[versioning]]

[Source: catalyst-nodejs-sdk-cloud-scale-core.md]
