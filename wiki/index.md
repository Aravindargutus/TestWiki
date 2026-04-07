---
title: Wiki Index
type: index
created: 2026-04-05
updated: 2026-04-06
---

# Wiki Index

A catalog of all pages in this wiki. The LLM reads this first when answering queries.

## Sources
- [[catalyst-java-sdk-overview]] — Catalyst Java SDK documentation covering initialization, scopes, class hierarchy, and third-party integration (ingested 2026-04-05)
- [[catalyst-java-sdk-authentication]] — All 11 Authentication SDK operations: user registration, orgs, password reset, tokens, validation, CRUD (ingested 2026-04-05)
- [[catalyst-java-sdk-data-store]] — All 10 Data Store SDK operations: table/column meta, CRUD rows, pagination, bulk read/write/delete (ingested 2026-04-05)
- [[catalyst-java-sdk-stratus]] — All 17 Stratus SDK sub-pages: bucket ops, object upload/download, multipart, transfer manager, presigned URLs, versioning (ingested 2026-04-05)
- [[catalyst-serverless-functions]] — Help documentation for all 7 serverless function types: Basic I/O, Advanced I/O, Event, Cron, Browser Logic, Integration, Job (ingested 2025-07-10)
- [[catalyst-java-sdk-cloud-scale-remaining]] — 21 SDK pages: File Store, ZCQL, Cache, Connections, Search, Mail, Push Notifications, Functions, Circuits, AppSail (ingested 2026-04-06)
- [[catalyst-java-sdk-zia-services]] — 15 Zia Services SDK pages: OCR, AutoML, Face Analytics, Identity Scanner (5), Text Analytics (4), Image Moderation, Object Recognition, Barcode Scanner (ingested 2026-04-06)
- [[catalyst-java-sdk-smartbrowz]] — 7 SmartBrowz SDK pages: Browser Grid (6) + PDF & Screenshot (1) (ingested 2026-04-06)
- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]] — 20 SDK pages: Job Scheduling (15), Pipelines (3), QuickML (1), Connectors (1) (ingested 2026-04-06)
- [[catalyst-java-sdk-nosql-security-apigateway]] — 18 pages: NoSQL SDK (10), Security Rules (3), API Gateway (5) — fills remaining Java SDK gaps (ingested 2026-04-07)

## Entities
- [[zoho-catalyst]] — Zoho's serverless BaaS platform (9 sources)
- [[zcproject]] — Base class of the Catalyst Java SDK (1 source)
- [[zcuser]] — Central class for all authentication/user operations (2 sources)
- [[zcsignupdata]] — Data holder for user registration and password reset (1 source)
- [[zcobject]] — Entry point class for Data Store operations (2 sources)
- [[zctable]] — Table class with row/column CRUD operations (1 source)
- [[zcstratus]] — Stratus component entry point class (1 source)
- [[zcbucket]] — Bucket class for upload/download/delete/presigned URL operations (1 source)
- [[zcobject-stratus]] — Stratus object class for versions, details, metadata (1 source)
- [[smartbrowz]] — Browser automation service powering Browser Logic Functions (2 sources)
- [[zcfile]] — File Store entry point class (1 source)
- [[zcfolder]] — Folder class for file upload/download/delete (1 source)
- [[zccache]] — Cache entry point class (1 source)
- [[zcsegment]] — Cache segment class for key-value CRUD (1 source)
- [[zcml]] — Unified entry point for all Zia AI/ML services (1 source)
- [[zcnosql]] — Entry point class for all NoSQL operations (1 source)

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
- [[file-store]] — Original cloud-scale file storage, predecessor to Stratus (1 source)
- [[zcql]] — Catalyst query language for Data Store operations (1 source)
- [[cache]] — In-memory key-value cache organized in segments (1 source)
- [[connections-sdk]] — OAuth token management for Zoho service integrations (1 source)
- [[catalyst-search]] — Pattern-based search across indexed Data Store columns (1 source)
- [[catalyst-mail]] — Email sending from Catalyst applications (1 source)
- [[push-notifications]] — Web and mobile push notification delivery (1 source)
- [[circuits]] — Workflow orchestration for sequential/concurrent function execution (1 source)
- [[appsail]] — Managed platform for persistent web service deployment (1 source)
- [[zia-services]] — AI/ML suite: vision, NLP, identity, custom ML via ZCML (1 source)
- [[identity-scanner]] — E-KYC + Document Processing for identity verification (1 source)
- [[text-analytics]] — NLP: Sentiment Analysis, NER, Keyword Extraction (1 source)
- [[browser-grid]] — Headless browser auto-scaling management via SmartBrowz (1 source)
- [[pdf-screenshot]] — Visual document generation from HTML/URL/templates (1 source)
- [[job-scheduling-sdk]] — Job Pool + Jobs + Cron scheduling and management (1 source)
- [[catalyst-pipelines]] — CI/CD pipeline execution for app deployment (1 source)
- [[quickml]] — No-code ML model endpoint prediction (1 source)
- [[connectors-external]] — External Zoho service OAuth token management (1 source)
- [[nosql]] — Non-relational document-type database with key-value JSON storage (1 source)
- [[security-rules]] — Default JSON-based access control for serverless functions (1 source)
- [[api-gateway]] — Advanced API management with routing, API Key auth, and throttling (1 source)

## Comparisons
_No comparisons yet._

## Analyses
_No analyses yet._
