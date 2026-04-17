---
title: Wiki Index
type: index
created: 2026-04-05
updated: 2026-04-16
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
- [[catalyst-nodejs-sdk-overview-serverless]] — Node.js SDK v2: Overview, upgrade, third-party integration, serverless (Functions, Circuits, AppSail) (ingested 2026-04-07)
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js SDK v2: Authentication (11), Data Store (11), NoSQL (10), Stratus (19) (ingested 2026-04-07)
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js SDK v2: File Store, Cache, Connections, Search, ZCQL, Mail, Push Notifications (23 pages) (ingested 2026-04-07)
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js SDK v2: Zia (14), SmartBrowz (9), Job Scheduling (17), Pipelines (3), QuickML (1), Connectors (1) (ingested 2026-04-07)
- [[catalyst-general-topics]] — Platform introduction, Quick Start, CLI, Projects, Organizations, Environments/Billing, DevOps, ConvoKraft, Signals, Slate (ingested 2026-04-16)

## Entities
- [[zoho-catalyst]] — Zoho's serverless BaaS platform (15 sources)
- [[zcproject]] — Base class of the Catalyst Java SDK (1 source)
- [[zcuser]] — Central class for all authentication/user operations (3 sources)
- [[zcsignupdata]] — Data holder for user registration and password reset (1 source)
- [[zcobject]] — Entry point class for Data Store operations (3 sources)
- [[zctable]] — Table class with row/column CRUD operations (1 source)
- [[zcstratus]] — Stratus component entry point class (2 sources)
- [[zcbucket]] — Bucket class for upload/download/delete/presigned URL operations (2 sources)
- [[zcobject-stratus]] — Stratus object class for versions, details, metadata (2 sources)
- [[smartbrowz]] — Browser automation service powering Browser Logic Functions (3 sources)
- [[zcfile]] — File Store entry point class (2 sources)
- [[zcfolder]] — Folder class for file upload/download/delete (1 source)
- [[zccache]] — Cache entry point class (2 sources)
- [[zcsegment]] — Cache segment class for key-value CRUD (1 source)
- [[zcml]] — Unified entry point for all Zia AI/ML services (2 sources)
- [[zcnosql]] — Entry point class for all NoSQL operations (2 sources)
- [[zcatalyst-sdk-node]] — Node.js SDK v2 npm package (`zcatalyst-sdk-node`) with ~131 pages of documentation (4 sources)
- [[catalyst-cli]] — `zcatalyst-cli` npm package for project init, test, deploy lifecycle (1 source)

## Concepts
- [[sdk-scopes]] — Admin vs User access control levels in the SDK (1 source)
- [[instance-objects]] — `getInstance()` pattern to avoid cascading API calls (1 source)
- [[class-hierarchy]] — SDK class hierarchy mirroring Catalyst project structure (1 source)
- [[third-party-sdk-integration]] — Using the SDK outside Catalyst via OAuth (1 source)
- [[catalyst-authentication]] — Full user lifecycle: registration, roles, orgs, auth types (3 sources)
- [[catalyst-organizations]] — Org ID (ZAAID) and multi-org user management (2 sources)
- [[custom-user-validation]] — Serverless function-based signup validation (1 source)
- [[third-party-authentication]] — External auth with custom server tokens (1 source)
- [[data-store]] — Catalyst's cloud-scale relational database (3 sources)
- [[data-store-pagination]] — Token-based pagination replacing deprecated getAllRows() (1 source)
- [[bulk-operations]] — Async job-based read/write/delete at scale (1 source)
- [[stratus]] — Catalyst's cloud-scale object storage service (2 sources)
- [[presigned-urls]] — Temporary authenticated URLs for unauthenticated object access (1 source)
- [[multipart-uploads]] — Splitting large objects into parts for parallel upload (1 source)
- [[transfer-manager]] — SDK utility for large file upload/download (1 source)
- [[versioning]] — Multi-version object storage in Stratus buckets (1 source)
- [[serverless-functions]] — 7 function types, runtimes, lifecycle, execution tiers (1 source)
- [[basic-io-functions]] — Simple JSON I/O with `/execute` URL pattern (1 source)
- [[advanced-io-functions]] — Full HTTP framework support: Express, Servlet, Flask (1 source)
- [[event-functions]] — Async functions triggered by Event Listeners (1 source)
- [[event-listeners]] — Event bus component listening for Catalyst/custom/Zoho events (1 source)
- [[cron-functions]] — Scheduled periodic execution via Cron jobs (1 source)
- [[cron]] — Cloud Scale job scheduler triggering URLs, cron functions, or circuits (1 source)
- [[browser-logic-functions]] — Playwright-based browser automation via SmartBrowz (1 source)
- [[integration-functions]] — Backend for Zoho services, Cliq only; excluded in EU/AU/IN/JP/SA/CA (1 source)
- [[job-functions]] — Background tasks via Job Scheduling pools (1 source)
- [[function-urls]] — URL patterns and accessibility across function types (1 source)
- [[catalyst-config]] — Universal function configuration file (1 source)
- [[runtime-support-policy]] — 4-phase language runtime deprecation lifecycle (1 source)
- [[cost-optimization]] — Credit-based pricing formula and memory tuning strategy (1 source)
- [[file-store]] — Original cloud-scale file storage, predecessor to Stratus (2 sources)
- [[zcql]] — Catalyst query language for Data Store operations (2 sources)
- [[cache]] — In-memory key-value cache organized in segments (2 sources)
- [[connections-sdk]] — OAuth token management for Zoho service integrations (2 sources)
- [[catalyst-search]] — Pattern-based search across indexed Data Store columns (2 sources)
- [[catalyst-mail]] — Email sending from Catalyst applications (2 sources)
- [[push-notifications]] — Web and mobile push notification delivery (2 sources)
- [[circuits]] — Workflow orchestration for sequential/concurrent function execution (2 sources)
- [[appsail]] — Managed platform for persistent web service deployment (2 sources)
- [[zia-services]] — AI/ML suite: vision, NLP, identity, custom ML via ZCML (2 sources)
- [[identity-scanner]] — E-KYC + Document Processing for identity verification (2 sources)
- [[text-analytics]] — NLP: Sentiment Analysis, NER, Keyword Extraction (2 sources)
- [[browser-grid]] — Headless browser auto-scaling management via SmartBrowz (2 sources)
- [[pdf-screenshot]] — Visual document generation from HTML/URL/templates (2 sources)
- [[job-scheduling-sdk]] — Job Pool + Jobs + Cron scheduling and management (2 sources)
- [[catalyst-pipelines]] — CI/CD pipeline execution for app deployment (2 sources)
- [[quickml]] — No-code ML model endpoint prediction (2 sources)
- [[connectors-external]] — External Zoho service OAuth token management (2 sources)
- [[nodejs-sdk-initialization]] — Promise-based initialization patterns for Node.js SDK v2 (1 source)
- [[dataverse]] — SmartBrowz web data extraction: Lead Enrichment, Tech Stack Finder, Similar Companies (1 source)
- [[nosql]] — Non-relational document-type database with key-value JSON storage (2 sources)
- [[security-rules]] — Default JSON-based access control for serverless functions (1 source)
- [[api-gateway]] — Advanced API management with routing, API Key auth, and throttling (1 source)
- [[catalyst-projects]] — Project structure, limits, cross-platform development (1 source)
- [[catalyst-console]] — Web console navigation, dashboards, metrics (1 source)
- [[catalyst-environments]] — Development (free) vs Production (billed) environments (1 source)
- [[catalyst-devops]] — Monitoring, APM, Logs, Metrics, GitHub Integration, Automation Testing (1 source)
- [[catalyst-convokraft]] — AI conversational bot service with Actions, Handlers, Bot Operations (1 source)
- [[catalyst-signals]] — Event Bus Service: Publishers, Targets, Events, Webhooks, Rules (1 source)
- [[catalyst-slate]] — Frontend deployment platform with Git-based automation and custom domains (1 source)
- [[web-client-hosting]] — Basic web client hosting (one app per project, console/CLI/GitHub deploy)
- [[mobile-device-management]] — Host/distribute Android + iOS apps via ManageEngine MDM (not in EU/AU/IN/JP/SA/CA)
- [[domain-mappings]] — Map custom domains to production URL (5 max per app, free Group SSL)
- [[application-alerts]] — Email alerts on Cron/Event Listener/Logs events (5 dev / 20 prod)
- [[apm]] — Function performance monitoring (Java/Node.js only, not in CA DC)
- [[catalyst-logs]] — Execution logs (Access vs Application; 7d dev / 14d prod retention)
- [[catalyst-metrics]] — Resource-usage dashboards for project consumption
- [[automation-testing]] — Automated API tests against function URLs or any third-party URL
- [[olap-database]] — Built-in read-only analytics engine synced from Data Store; ZCQL-queried
- [[audit-logs]] — Console + Application activity logs; 1yr UI / 6yr export; HIPAA-grade
- [[environment-variables]] — Per-env key/value config for Functions, AppSail, Slate (dev vs prod)
- [[collaborators]] — Project members, admins, and Super Admin; 100/org cap; console + CLI parity
- [[profiles-and-permissions]] — Project-scoped RBAC; 3 default profiles + 50 custom in dev; GDPR/HIPAA-aligned
- [[iac-settings]] — Project export/import via `project-template.json` ZIP; cross-DC/account/Git transfer
- [[collaborators]] — Project members, admins, and Super Admin; 100/org cap; console + CLI parity
- [[profiles-and-permissions]] — Project-scoped RBAC; 3 default profiles + 50 custom in dev; GDPR/HIPAA-aligned
- [[iac-settings]] — Project export/import via `project-template.json` ZIP; cross-DC/account/Git transfer

## Comparisons
_No comparisons yet._

## Analyses
_No analyses yet._
