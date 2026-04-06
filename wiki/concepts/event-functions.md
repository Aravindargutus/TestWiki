---
title: Event Functions
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [functions, event, async, serverless, catalyst]
---

# Event Functions

## Definition

Event Functions are asynchronous [[serverless-functions]] in [[zoho-catalyst]] triggered by Event Listeners. They cannot be invoked manually, have no Function URL, and respond to events from Catalyst components or custom URL invocations. They have a long execution time of up to 15 minutes. [Source: catalyst-serverless-functions.md]

## Key Aspects

### Trigger: Event Listeners
**Default Event Listeners** — Catalyst component events:
- **DataStore** — Row insert, update, delete
- **Cache** — Cache operations
- **Auth** — User signup, login events
- **FileStore** — File upload, delete events

**Custom Event Listeners**:
- **GitHub** — Webhook events (push, PR, etc.)
- **webclient** — Custom client-side events
- URL invocation

### Execution Limit
- **Timeout**: 15 minutes (vs 30s for HTTP-facing functions)

### Module API

**context**:
- `closeWithSuccess()` / `closeWithFailure()` — Terminates with status
- `getRemainingExecutionTimeMs()` — Remaining time in ms
- `getMaxExecutionTimeMs()` — Max time (15 min)

**event**:
- `getAction()` — Trigger action (Insert, Update, Delete)
- `getSourceEntityId()` — Triggering entity ID
- `getProjectDetails()` — Project metadata
- `getData()` — Event payload
- `getTime()` — Event timestamp
- `getSource()` — Source component name
- `getEventBusDetails()` — Event bus metadata

### Java Return Values
- `EVENT_STATUS.SUCCESS` / `EVENT_STATUS.FAILURE`

### When to Use
- Reacting to data changes (CRUD events on DataStore)
- Post-processing after file uploads
- Webhook handling (GitHub, etc.)
- Any async workflow triggered by platform events

## Sources

- [[catalyst-serverless-functions]] — Help documentation

## Related Concepts

- [[serverless-functions]] — Parent concept
- [[cron-functions]], [[job-functions]] — Other background function types
- [[data-store]] — Common event source for DataStore events
