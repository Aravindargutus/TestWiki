---
title: Cron Functions
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [functions, cron, scheduled, serverless, catalyst]
---

# Cron Functions

## Definition

Cron Functions are periodic [[serverless-functions]] in [[zoho-catalyst]] associated with Cron jobs. They execute on a schedule (one-time or recurring) and cannot be invoked manually. No Function URL. Max execution: 15 minutes. [Source: catalyst-serverless-functions.md]

## Key Aspects

### Schedule Types
- **One-time**: Execute once at a specific date/time
- **Recurring**: Execute at regular intervals (cron expression)

### Execution Limit
- **Timeout**: 15 minutes

### Module API

**CronRequest**:
- `getCronParam(key)` / `getAllCronParams()` — Cron parameters
- `getRemainingExecutionCount()` — Remaining scheduled executions
- `getCronDetails()` — Cron job metadata
- `getProjectDetails()` — Project metadata

**Context**:
- `getMaxExecutionTimeMs()` / `getRemainingExecutionTimeMs()` — Execution time info

### Java Return Values
- `CRON_STATUS.SUCCESS` / `CRON_STATUS.FAILURE`

### When to Use
- Periodic data cleanup or aggregation
- Scheduled report generation
- Recurring sync jobs
- One-time deferred execution

## Sources

- [[catalyst-serverless-functions]] — Help documentation

## Related Concepts

- [[serverless-functions]] — Parent concept
- [[event-functions]], [[job-functions]] — Other background function types
