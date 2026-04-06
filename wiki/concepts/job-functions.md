---
title: Job Functions
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [functions, job, scheduling, background, serverless, catalyst]
---

# Job Functions

## Definition

Job Functions are [[serverless-functions]] in [[zoho-catalyst]] invoked by the Job Scheduling service (Function Job Pool). They are non-HTTPS functions without a URL, designed for long-running background tasks managed by job pools. Max execution: 15 minutes. [Source: catalyst-serverless-functions.md]

## Key Aspects

### Runtimes
- **Java**: 7, 11, 17 (note: Java 7 is unique to Job Functions)
- **Node.js**: 18
- **Python**: 3.9

### Module API

**JobRequest**:
- `getProjectDetails()` — Project metadata
- `getJobDetails()` / `getJobMetaDetails()` — Job info
- `getJobpoolDetails()` — Job pool configuration
- `getJobCapacityAttributes()` — Memory/CPU capacity info
- `getAllJobParams()` / `getJobParam(key)` — Job parameters

**Context**:
- `getRemainingExecutionTimeMs()` / `getMaxExecutionTimeMs()` — Execution time (15 min)

### Java Return Values
- `JOB_STATUS.SUCCESS` / `JOB_STATUS.FAILURE`

### Memory Constraint
Function memory must be **less than** the associated job pool memory.

### When to Use
- Long-running batch processing
- Background data transformations
- Tasks triggered by job scheduling system
- Operations requiring controlled resource allocation via pools

## Sources

- [[catalyst-serverless-functions]] — Help documentation

## Related Concepts

- [[serverless-functions]] — Parent concept
- [[event-functions]], [[cron-functions]] — Other background function types
