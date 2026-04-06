---
title: Serverless Functions
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [serverless, functions, compute, catalyst]
---

# Serverless Functions

## Definition

Serverless Functions are the core compute units of the [[zoho-catalyst]] platform — custom-built code structures that contain application business logic and execute without managing infrastructure. Catalyst supports **7 distinct function types** across **3 runtimes** (Java, Node.js, Python). [Source: catalyst-serverless-functions.md]

## Key Aspects

### Function Types
1. **[[basic-io-functions]]** — Simple JSON input/output, `/execute` URL
2. **[[advanced-io-functions]]** — Full HTTP framework support (Express/Servlet/Flask)
3. **[[event-functions]]** — Async, triggered by Event Listeners (DataStore, Cache, Auth, FileStore, GitHub)
4. **[[cron-functions]]** — Periodic execution via Cron job schedules
5. **[[browser-logic-functions]]** — Playwright-based browser automation (SmartBrowz)
6. **[[integration-functions]]** — Backend for Zoho services (Cliq only, US-only)
7. **[[job-functions]]** — Background tasks via Job Scheduling pools

### Runtime Support
- **Java**: 8, 11, 17 (Job Functions also support Java 7)
- **Node.js**: 20, 18, 16, 14, 12 (12 is EOL)
- **Python**: 3.9 only (not supported for Browser Logic)

### Execution Time Tiers
- **30 seconds**: HTTP-facing functions (Basic I/O, Advanced I/O, Browser Logic)
- **15 minutes**: Background functions (Event, Cron, Job)

### URL-Accessible vs Non-URL
- **Has URL**: Basic I/O (`/execute`), Advanced I/O (`/`), Browser Logic (`/execute`)
- **No URL**: Event, Cron, Integration, Job — triggered by internal platform mechanisms

### Lifecycle
Initialize → Code → Test (local) → Deploy → Delete

### Configuration
All functions use `catalyst-config.json` as their configuration file, regardless of runtime. See [[catalyst-config]].

### Management Tools
- Security Rules, API Gateway, Logs, APM, Circuits (circuit breaker)

## Sources

- [[catalyst-serverless-functions]] — Comprehensive help documentation covering all 7 types

## Related Concepts

- [[basic-io-functions]], [[advanced-io-functions]] — The two HTTP-facing request/response patterns
- [[event-functions]], [[cron-functions]], [[job-functions]] — Background/async execution patterns
- [[browser-logic-functions]] — Specialized browser automation via [[smartbrowz]]
- [[integration-functions]] — Zoho service backend pattern
- [[function-urls]] — URL patterns across function types
- [[catalyst-config]] — Shared configuration file
- [[sdk-scopes]] — Access control for SDK operations (related but documented separately)

## Evolution

- **Initial ingest (2025-07-10)**: Full documentation of all 7 function types from help pages. This is the first non-SDK source ingested — operational/conceptual rather than API reference.
