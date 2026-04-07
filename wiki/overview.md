---
title: Overview
type: overview
created: 2026-04-05
updated: 2026-04-07
sources: [catalyst-java-sdk-overview.md, catalyst-java-sdk-authentication.md, catalyst-java-sdk-data-store.md, catalyst-java-sdk-stratus.md, catalyst-serverless-functions.md, catalyst-java-sdk-cloud-scale-remaining.md, catalyst-java-sdk-zia-services.md, catalyst-java-sdk-smartbrowz.md, catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md, catalyst-nodejs-sdk-overview-serverless.md, catalyst-nodejs-sdk-cloud-scale-core.md, catalyst-nodejs-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
---

# Overview

This wiki covers the **Zoho Catalyst** platform — a serverless Backend-as-a-Service (BaaS) by Zoho Corporation.

## Current Coverage

**Two complete SDK ingests:**
- **Java SDK v1**: 124 pages across 10 sources — class-based, synchronous API
- **Node.js SDK v2**: ~131 pages across 4 sources — promise-based, async/await API

Both SDKs cover the same Catalyst components with language-appropriate patterns.

## Java SDK

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

### Remaining Cloud Scale Components

The sixth ingest completes the Cloud Scale SDK coverage with **11 additional components** (21 pages):

- **File Store**: Original file storage (5 ops via `ZCFile` → `ZCFolder`). Simple upload/download/delete with 100MB limit. Being superseded by Stratus.
- **ZCQL**: Catalyst Query Language — 1 method (`executeQuery`) with v2 and OLAP flags. Queries Data Store tables.
- **Cache**: In-memory key-value storage organized in segments (5 ops via `ZCCache` → `ZCSegment`). Default 48h expiry.
- **Connections**: Internal-only OAuth token management for Zoho services (2 ops). Functions/AppSail only.
- **Search**: Pattern-based search across indexed Data Store columns (1 op). Multi-table wildcard queries.
- **Mail**: Email sending with limits (10 to, 10 CC, 5 BCC, 5 attachments/15MB). Requires console domain config.
- **Push Notifications**: Web (50 users/call) and mobile (Android/iOS separately, requires registered app).
- **Execute Function**: JSON in/out function invocation via `ZCatalystFunction`.
- **Circuits**: Workflow orchestration — execute, monitor, abort. **Not available in EU, AU, IN, JP, SA, CA**.
- **AppSail**: Persistent web service hosting. SDK init requires `AuthHeaderProvider` interface. Maven: `java-sdk:1.15.0`.

### Zia Services (AI/ML)

The seventh ingest covers all **15 Zia Services SDK operations**, all accessed via the unified `ZCML.getInstance()` entry point:

- **Vision**: OCR (19 languages, 20MB), Face Analytics (age/gender/emotion, 3 modes), Image Moderation (safe/unsafe), Object Recognition (80 types), Barcode Scanner (multi-format)
- **Identity Scanner**: Facial Comparison (E-KYC, global), Document Processing (Aadhaar, PAN, Passbook, Cheque — **IN DC only**)
- **NLP (Text Analytics)**: Sentiment Analysis, NER, Keyword Extraction — individually or combined. 1500 char limit.
- **Custom ML**: AutoML prediction via model ID. **US DC only**.
- Privacy-first: no file storage, one-time processing, no ML training on user data.

### SmartBrowz

The eighth ingest covers **7 SmartBrowz SDK pages** with two sub-components:

- **Browser Grid** (5 ops): Manage headless browser grids — get instance, list all, get specific, get nodes, stop. All require Admin scope.
- **PDF & Screenshot** (2 methods): Generate visual documents from HTML, URLs, or templates. Rich options: format, margins, landscape, device emulation, password protection.

### Job Scheduling + Pipelines + QuickML + Connectors

The ninth ingest completes the SDK with **20 pages** across 4 remaining components:

- **Job Scheduling** (15 ops): Most feature-rich component. Job Pool (2), Jobs (3: create via builder pattern for 4 target types), Cron (10: create one-time/recurring/expression, full CRUD + pause/resume/run lifecycle).
- **Pipelines** (3 ops): CI/CD — get instance, get details, execute with branch + env vars.
- **QuickML** (1 op): Predict via published ML endpoint key. **Not available in JP, SA, CA**.
- **Connectors** (external): OAuth token management for Zoho services. Stores tokens in Cache. Different from Connections SDK (internal).

## Node.js SDK v2

**~131 pages ingested across 4 sources.** The Node.js SDK (`zcatalyst-sdk-node` v2.5.0) covers the same components as the Java SDK but with promise-based APIs:

### Key Differences from Java SDK
- **Initialization**: `catalyst.initialize(req)` returns an `app` object (vs Java's `ZCProject`)
- **API style**: Promises (`.then()` / `async/await`) vs Java's synchronous beans
- **Scopes**: Same Admin/User model, applies to Data Store, File Store, ZCQL only
- **Package**: npm (`zcatalyst-sdk-node`) vs Java SDK JAR

### Component Coverage
All 19 component instances accessible from the app object:
- **Serverless**: `app.functions()`, `app.circuit()`
- **Cloud Scale**: `app.userManagement()`, `app.datastore()`, `app.nosql()`, `app.stratus()`, `app.filestore()`, `app.cache()`, `app.connections()`, `app.search()`, `app.zcql()`, `app.email()`, `app.pushNotification()`
- **Zia**: `app.zia()` (OCR, AutoML, Face Analytics, Identity Scanner, Text Analytics, Image Moderation, Object Recognition, Barcode)
- **SmartBrowz**: `app.smartbrowz()` (Browser Grid, PDF/Screenshot, Dataverse)
- **Job Scheduling**: `app.jobScheduling()` (Job Pool, Jobs, Cron)
- **Other**: `app.pipeline()`, `app.quickML()`, `app.connection({...})`

### Unique to Node.js SDK
- **Dataverse** (SmartBrowz): Lead Enrichment, Tech Stack Finder, Similar Companies — web scraping intelligence
- **Named app caching**: `catalyst.initialize(req, { appName: 'name' })` → `catalyst.app('name')`

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

- ~~File Store, ZCQL, Cache, Search, Mail, Push Notifications SDK operations not yet ingested~~ — DONE
- ~~Serverless (Functions, Cron) SDK operations not yet covered~~ — DONE
- ~~Security Rules, API Gateway, Circuits details not yet documented~~ — Circuits SDK DONE; Security Rules and API Gateway still pending
- ~~Zia Services, Pipelines, QuickML, Connectors not yet covered~~ — ALL DONE
- ~~SmartBrowz not yet covered~~ — DONE (Browser Grid + PDF/Screenshot)
- ~~Job Scheduling SDK operations not yet covered~~ — DONE (15 pages)
- ~~NoSQL SDK pages consistently failed to load~~ — DONE (10 pages via docs-ea.catalyst.zoho.com/en/sdk/java/v1/)
- ~~Node.js SDK and other language SDKs not yet covered~~ — Node.js SDK v2 DONE (~131 pages)
- Platform pricing, limits, and deployment mechanics unknown
- Roles and permissions deep-dive needed (how RoleId maps to scopes)
- Function memory limits and cold start characteristics unknown
- Integration Functions data center expansion plans unknown
- ~~Security Rules and API Gateway details not yet documented~~ — DONE (Security Rules 3 pages, API Gateway 5 pages)
- Connectors vs Connections SDK disambiguation needs deeper analysis
