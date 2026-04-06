---
title: Wiki Index
type: index
created: 2026-04-05
updated: 2025-07-10
---

# Wiki Index

A catalog of all pages in this wiki. The LLM reads this first when answering queries.

## Sources
- [[catalyst-java-sdk-overview]] — Catalyst Java SDK documentation covering initialization, scopes, class hierarchy, and third-party integration (ingested 2026-04-05)
- [[catalyst-java-sdk-authentication]] — All 11 Authentication SDK operations: user registration, orgs, password reset, tokens, validation, CRUD (ingested 2026-04-05)
- [[catalyst-java-sdk-data-store]] — All 10 Data Store SDK operations: table/column meta, CRUD rows, pagination, bulk read/write/delete (ingested 2026-04-05)
- [[catalyst-java-sdk-stratus]] — All 17 Stratus SDK sub-pages: bucket ops, object upload/download, multipart, transfer manager, presigned URLs, versioning (ingested 2026-04-05)
- [[catalyst-serverless-functions]] — Help documentation for all 7 serverless function types: Basic I/O, Advanced I/O, Event, Cron, Browser Logic, Integration, Job (ingested 2025-07-10)

## Entities
- [[zoho-catalyst]] — Zoho's serverless BaaS platform (5 sources)
- [[zcproject]] — Base class of the Catalyst Java SDK (1 source)
- [[zcuser]] — Central class for all authentication/user operations (2 sources)
- [[zcsignupdata]] — Data holder for user registration and password reset (1 source)
- [[zcobject]] — Entry point class for Data Store operations (2 sources)
- [[zctable]] — Table class with row/column CRUD operations (1 source)
- [[zcstratus]] — Stratus component entry point class (1 source)
- [[zcbucket]] — Bucket class for upload/download/delete/presigned URL operations (1 source)
- [[zcobject-stratus]] — Stratus object class for versions, details, metadata (1 source)
- [[smartbrowz]] — Browser automation service powering Browser Logic Functions (1 source)

## Concepts
- [[sdk-scopes]] — Admin vs User access control levels in the SDK (1 source)
- [[instance-objects]] — `getInstance()` pattern to avoid cascading API calls (1 source)
- [[class-hierarchy]] — SDK class hierarchy mirroring Catalyst project structure (1 source)
- [[third-party-sdk-integration]] — Using the SDK outside Catalyst via OAuth (1 source)
- [[catalyst-authentication]] — Full user lifecycle: registration, roles, orgs, auth types (2 sources)
- [[catalyst-organizations]] — Org ID (ZAAID) and multi-org user management (1 source)
- [[custom-user-validation]] — Serverless function-based signup validation (1 source)
- [[third-party-authentication]] — External auth with custom server tokens (1 source)
- [[data-store]] — Catalyst's cloud-scale relational database (2 sources)
- [[data-store-pagination]] — Token-based pagination replacing deprecated getAllRows() (1 source)
- [[bulk-operations]] — Async job-based read/write/delete at scale (1 source)
- [[stratus]] — Catalyst's cloud-scale object storage service (1 source)
- [[presigned-urls]] — Temporary authenticated URLs for unauthenticated object access (1 source)
- [[multipart-uploads]] — Splitting large objects into parts for parallel upload (1 source)
- [[transfer-manager]] — SDK utility for large file upload/download (1 source)
- [[versioning]] — Multi-version object storage in Stratus buckets (1 source)
- [[serverless-functions]] — 7 function types, runtimes, lifecycle, execution tiers (1 source)
- [[basic-io-functions]] — Simple JSON I/O with `/execute` URL pattern (1 source)
- [[advanced-io-functions]] — Full HTTP framework support: Express, Servlet, Flask (1 source)
- [[event-functions]] — Async functions triggered by Event Listeners (1 source)
- [[cron-functions]] — Scheduled periodic execution via Cron jobs (1 source)
- [[browser-logic-functions]] — Playwright-based browser automation via SmartBrowz (1 source)
- [[integration-functions]] — Backend for Zoho services, Cliq only, US-only (1 source)
- [[job-functions]] — Background tasks via Job Scheduling pools (1 source)
- [[function-urls]] — URL patterns and accessibility across function types (1 source)
- [[catalyst-config]] — Universal function configuration file (1 source)

## Comparisons
_No comparisons yet._

## Analyses
_No analyses yet._
