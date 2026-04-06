---
title: Catalyst Java SDK — Job Scheduling + Pipelines + QuickML + Connectors
type: source
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]
tags: [sdk, java, job-scheduling, pipelines, quickml, connectors, cron]
---

# Catalyst Java SDK — Job Scheduling + Pipelines + QuickML + Connectors

## Summary

Covers **20 SDK pages** across 4 components: Job Scheduling (15 pages: Overview, Init, Job Pool 2, Jobs 3, Cron 10), Pipelines (3), QuickML (1), Connectors (1). Completes the entire Catalyst Java SDK v1 documentation.

[Source: catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]

## Key Takeaways

1. **Job Scheduling** is the most feature-rich component with 15 pages. Three sub-components: Job Pool (query), Jobs (create/get/delete), Cron (full CRUD + lifecycle).
2. **Builder pattern**: Jobs use `ZCJobBuilder` (function/circuit/webhook/appsail variants) and Crons use `ZCCronBuilder` (one-time/every/expression variants).
3. **4 job targets**: Functions, Webhooks, Circuits, AppSail — all share the same builder pattern.
4. **3 cron types**: One-time (UNIX timestamp), Recurring (every N hours/mins/secs, daily, monthly, yearly), Expression (UNIX cron expressions).
5. **Cron lifecycle**: Create → Get → Update → Pause → Resume → Run → Delete. All accept ID or name.
6. **Dynamic vs Pre-defined Crons**: SDK is for Dynamic Crons; UI Builder for Pre-defined. `getCron()` (all) only returns Pre-defined.
7. **Pipelines** = CI/CD: Get instance, get details, execute (with branch + env vars). Returns run history.
8. **QuickML** = No-code ML predictions via published endpoint keys. **Not available in JP, SA, CA**.
9. **Connectors** (external) manage OAuth tokens for Zoho service APIs. Stores tokens in [[cache]]. Different from [[connections-sdk]] (internal).
10. **Connectors are Zoho-only** — cannot connect to third-party services. Each connector name must be unique per user.

## SDK Method Reference

| Component | Operations | Key Classes |
|-----------|-----------|-------------|
| Job Scheduling | getInstance, getJobpool (all/specific) | ZCJobScheduling, ZCJobpoolDetails |
| Jobs | submitJob, getJob, deleteJob | ZCJobBuilder, ZCJobMetaDetail, ZCJobDetails |
| Cron | createCron, getCron (one/all), updateCron, pauseCron, resumeCron, runCron, deleteCron | ZCCronBuilder, ZCCronDetails |
| Pipelines | getInstance, getPipelineDetails, runPipeline | ZCPipeline, ZCPipelineDetails, ZCPipelineRunHistory |
| QuickML | predict(endpointKey, map) | ZCQuickML, ZCQuickMLDetail |
| Connectors | getInstance(json), getConnector(name), getAccessToken | ZCConnection, ZCConnector |

## Entities Mentioned

- [[zoho-catalyst]] — Platform (update)

## Concepts Introduced

- [[job-scheduling-sdk]] — Job Pool + Jobs + Cron management (new)
- [[catalyst-pipelines]] — CI/CD pipeline execution (new)
- [[quickml]] — No-code ML model endpoint execution (new)
- [[connectors-external]] — External Zoho service OAuth management (new)

## Open Questions

1. What job pool types exist beyond "functions_jobpool"?
2. Cron expression format — standard UNIX 5-field or custom?
3. How many concurrent pipeline executions are allowed?
4. Connectors vs Connections SDK — when to use which?
