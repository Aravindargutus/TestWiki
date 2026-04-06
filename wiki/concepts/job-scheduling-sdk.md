---
title: Job Scheduling SDK
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]
tags: [job-scheduling, cron, jobs, job-pool]
---

# Job Scheduling SDK

## Definition

Job Scheduling is Catalyst's service for scheduling and executing jobs that trigger Functions, Webhooks, [[circuits]], and [[appsail]] endpoints. It has three sub-components: Job Pool (query pools), Jobs (submit/get/delete), and Cron (schedule with full lifecycle management). [Source: catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]

## Key Aspects

- **Entry point**: `ZCJobScheduling.getInstance()` — provides `.job` and `.cron` sub-objects
- **Job Pool** (2 ops): Get all pools, get specific by ID or name
- **Jobs** (3 ops): Submit (create), get details, delete — uses Builder pattern with 4 target types
- **Cron** (10 ops): Full lifecycle — create (one-time/recurring/expression), get (one/all), update, pause, resume, run, delete
- **Builder pattern**: `ZCJobBuilder.functionJobBuilder()` / `.circuitJobBuilder()` / `.webhookJobBuilder()` / `.appSailJobBuilder()`
- **Cron builders**: `ZCCronBuilder.zcOneTimeCronBuilder()` / `.zcEveryCronBuilder()` / `.zcExpressionCronBuilder()`
- **Job config**: Retries + retry interval (e.g., 2 retries in 15 min)
- **Dynamic vs Pre-defined**: SDK for Dynamic Crons; UI Builder for Pre-defined

## Sources

- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]]

## Related Concepts

- [[cron-functions]] — Cron Functions are triggered by Job Scheduling crons
- [[job-functions]] — Job Functions are triggered by Job Scheduling jobs
- [[circuits]] — Circuits can be triggered as job targets
- [[appsail]] — AppSail endpoints can be triggered as job targets
