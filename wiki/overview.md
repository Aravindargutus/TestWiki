---
title: Overview
type: overview
created: 2026-04-05
updated: 2025-07-10
sources: [catalyst-java-sdk-overview.md, catalyst-java-sdk-authentication.md, catalyst-java-sdk-data-store.md, catalyst-java-sdk-stratus.md, catalyst-serverless-functions.md]
---

# Overview

This wiki covers the **Zoho Catalyst** platform — a serverless Backend-as-a-Service (BaaS) by Zoho Corporation.

## Current Focus: Java SDK

Four major areas of the Catalyst Java SDK have been ingested:

### SDK Architecture
The SDK wraps Catalyst REST APIs into a Java class hierarchy rooted at `ZCProject`, supporting Java 8, 11, and 17. Key architectural concepts include:
- **Scoped access control** (Admin vs User) applied to Data Store, File Store, and ZCQL
- An **instance objects pattern** (`getInstance()`) that avoids unnecessary API calls when navigating deeply nested components
- **Third-party integration** enabling the SDK to be used in external applications (Vercel, AWS, etc.) via OAuth2

### Authentication (Cloud Scale)
The Authentication component provides **11 operations** covering the full user lifecycle through the `ZCUser` class:
- **User registration** with email invitations (max 25 in dev, unlimited in prod)
- **Organization management** — users belong to orgs (ZAAID); can be added to existing orgs
- **Password reset** via email with customizable templates
- **Three auth modes**: native/hosted, third-party (custom server tokens), and custom validation (serverless function)
- **User CRUD**: get current/all/by-ID, update details, enable/disable, delete

### Data Store (Cloud Scale)
The Data Store component provides **10 operations** for Catalyst's relational database through `ZCObject` → `ZCTable`:
- **Table/column metadata**: get by ID or name, list all
- **Row CRUD**: insert (single/multiple), get (single/paginated), update (ROWID required), delete
- **Pagination**: token-based `getPagedRows()` (default 200 rows/page, replacing deprecated `getAllRows()`)
- **Bulk operations**: Read (200K rows → CSV), Write (100K rows from Stratus CSV), Delete (200 rows/batch)
- **Dev limits**: 5,000 rows/table, 25,000 rows/project; no limits in production

### Stratus — Object Storage (Cloud Scale)
Stratus is Catalyst's object storage service, providing **17 SDK operations** across three tiers — `ZCStratus` → `ZCBucket` → `ZCObject`:
- **Bucket management**: check availability, list, get details, CORS
- **Object upload**: 5 strategies — stream, string, options, multipart (1–1000 parts, 50MB min), and Transfer Manager wrapper
- **Object download**: direct, partial (byte range), Transfer Manager (iterable/part downloaders)
- **Presigned URLs**: time-limited URLs for unauthenticated upload/download (30s–7 day expiry)
- **Versioning**: optional bucket setting; each upload gets unique versionId; rename/move blocked when enabled
- **Object management**: copy, rename/move, delete (single/multi/truncate/path), metadata, zip extract
- **Admin scope dominance**: Nearly all operations require Admin scope, unlike Data Store

The platform provides a broad surface area of components: serverless functions, cloud-scale storage (Data Store, File Store, Stratus), AI/ML (Zia), browser automation (SmartBrowz), job scheduling, pipelines, ML (QuickML), and connectors.

## Serverless Functions (Help Documentation)

The Serverless Functions documentation (first non-SDK source) reveals the core compute layer of Catalyst. There are **7 distinct function types** organized into two execution tiers:

### HTTP-Facing Functions (30-second timeout)
- **Basic I/O**: Simple JSON input/output via `/execute` URL. Modules: `context` + `basicIO`. Output limited to 1MB.
- **Advanced I/O**: Full HTTP framework support (Express.js, javax.servlet, Flask). Multiple routes per function, streaming, custom headers. URL without `/execute` suffix.
- **Browser Logic**: Playwright-based browser automation via SmartBrowz. Same URL pattern as Basic I/O. Java and Node.js only (no Python).

### Background Functions (15-minute timeout)
- **Event Functions**: Async, triggered by Event Listeners (DataStore, Cache, Auth, FileStore, GitHub, webclient). No URL.
- **Cron Functions**: Periodic execution via scheduled Cron jobs. One-time or recurring. No URL.
- **Job Functions**: Background tasks invoked by Job Scheduling pools. Supports Java 7 (unique). No URL.
- **Integration Functions**: Backend for Zoho services (Cliq only). US data center only. No URL.

### Cross-Cutting Observations
- **3 runtimes**: Java (8/11/17), Node.js (20/18/16/14/12), Python (3.9 only)
- **Python is most limited**: not supported for Browser Logic, and only version 3.9
- **`catalyst-config.json`**: Universal configuration file across all runtimes and types
- **Management tools**: Security Rules, API Gateway, Logs, APM, Circuits (not yet detailed)

## Knowledge Gaps

- File Store, ZCQL, NoSQL, Cache, Search, Mail, Push Notifications SDK operations not yet ingested
- ~~Serverless (Functions, Cron) SDK operations not yet covered~~ — Help docs ingested; SDK API reference still needed
- Zia Services, Pipelines, QuickML, Connectors not yet covered
- ~~SmartBrowz not yet covered~~ — Covered via Browser Logic Functions
- Node.js SDK and other language SDKs not yet covered
- Platform pricing, limits, and deployment mechanics unknown
- Roles and permissions deep-dive needed (how RoleId maps to scopes)
- Security Rules, API Gateway, Circuits details not yet documented
- Function memory limits and cold start characteristics unknown
- Integration Functions data center expansion plans unknown
