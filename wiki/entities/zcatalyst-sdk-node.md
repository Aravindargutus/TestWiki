---
title: zcatalyst-sdk-node
type: entity
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-nodejs-sdk-overview-serverless.md, catalyst-nodejs-sdk-cloud-scale-core.md, catalyst-nodejs-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [nodejs, sdk, npm, v2, entry-point]
---

# zcatalyst-sdk-node

## Overview

The npm package for the Catalyst Node.js SDK v2. Current version: **2.5.0**. All versions earlier than 2.5.0 (including betas) are deprecated. This is the Node.js counterpart to the [[zcproject|Java SDK's ZCProject]] class.

## Key Facts

| Property | Value |
|---|---|
| Package name | `zcatalyst-sdk-node` |
| SDK version | v2 |
| Install | `npm install zcatalyst-sdk-node` |
| Import | `const catalyst = require('zcatalyst-sdk-node')` |
| Current version | 2.5.0 |
| GitHub | [catalystbyzoho](https://github.com/catalystbyzoho) |

## Component Instance Methods

All accessed via `const app = catalyst.initialize(req)`:

| Component | Method | Category |
|---|---|---|
| User Management | `app.userManagement()` | Cloud Scale |
| Data Store | `app.datastore()` | Cloud Scale |
| NoSQL | `app.nosql()` | Cloud Scale |
| Stratus | `app.stratus()` | Cloud Scale |
| File Store | `app.filestore()` | Cloud Scale |
| Cache | `app.cache()` | Cloud Scale |
| Connections | `app.connections()` | Cloud Scale |
| Search | `app.search()` | Cloud Scale |
| ZCQL | `app.zcql()` | Cloud Scale |
| Email | `app.email()` | Cloud Scale |
| Push Notifications | `app.pushNotification()` | Cloud Scale |
| Functions | `app.functions()` | Serverless |
| Circuits | `app.circuit()` | Serverless |
| Zia | `app.zia()` | Zia Services |
| SmartBrowz | `app.smartbrowz()` | SmartBrowz |
| Job Scheduling | `app.jobScheduling()` | Job Scheduling |
| Pipelines | `app.pipeline()` | Pipelines |
| QuickML | `app.quickML()` | QuickML |
| Connectors | `app.connection({...})` | Connectors |

## Initialization Patterns

- **Serverless functions**: `catalyst.initialize(req)` (Advanced I/O), `catalyst.initialize(context)` (Basic I/O, Event, Cron)
- **Express.js**: Same as Advanced I/O within route handlers
- **Third-party apps**: `catalyst.credential.refreshToken(creds)` + `catalyst.initializeApp(req)` with project_id, project_key, environment
- **Scopes**: `catalyst.initialize(req, { scope: 'admin' | 'user' })`
- **Named apps**: `catalyst.initialize(req, { appName: 'name' })` → `catalyst.app('name')`

## Appearances

- [[catalyst-nodejs-sdk-overview-serverless]] — Overview, initialization, scopes, third-party integration, serverless operations
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Authentication, Data Store, NoSQL, Stratus SDK methods
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — File Store, Cache, Connections, Search, ZCQL, Mail, Push Notifications
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Zia Services, SmartBrowz, Job Scheduling, Pipelines, QuickML, Connectors

## Relationships

- [[zoho-catalyst]] — The platform this SDK connects to
- [[zcproject]] — Java SDK equivalent (v1, class-based vs promise-based)

## Notes

- Promise-based API (`.then()` / `async/await`) vs Java SDK's synchronous bean pattern
- Scopes (Admin/User) only affect Data Store, File Store, and ZCQL operations
- ~131 documented SDK pages across 9 major sections

[Source: catalyst-nodejs-sdk-overview-serverless.md]
