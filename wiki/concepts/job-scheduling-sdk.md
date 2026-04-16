---
title: Job Scheduling SDK
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
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

## Node.js SDK Access Pattern

```js
const jobScheduling = app.jobScheduling();

// Job Pools
const pools = await jobScheduling.getAllJobpools();
const pool = await jobScheduling.getJobpool('test');

// Jobs (4 target types: Function, Circuit, Webhook, AppSail)
const job = await jobScheduling.JOB.submitJob({
  job_name: 'test_job', jobpool_name: 'test',
  target_type: 'Function', target_name: 'fn',
  params: { arg1: 'val' },
  job_config: { number_of_retries: 2, retry_interval: 900 }
});
await jobScheduling.JOB.getJob('jobId');
await jobScheduling.JOB.deleteJob('jobId');

// Cron (3 types: OneTime, Recurring, CronExpression)
const cron = await jobScheduling.CRON.createCron({ cron_name: 'test', cron_type: 'Recurring', cron_detail: { frequency: 'hourly', every: 2 }, job_meta: meta });
await jobScheduling.CRON.getCronDetails('cronId');
await jobScheduling.CRON.getAllCronDetails();
await jobScheduling.CRON.updateCron('cronId', config);
await jobScheduling.CRON.pauseCron('cronId');
await jobScheduling.CRON.resumeCron('cronId');
await jobScheduling.CRON.runCron('cronId');
await jobScheduling.CRON.deleteCron('cronId');
```

**Key differences**: Java uses Builder pattern (`ZCJobBuilder.functionJobBuilder()`); Node.js uses plain JSON with `target_type` field. Java uses `ZCCronBuilder`; Node.js uses `cron_type` in config JSON. Node.js accesses sub-components via `jobScheduling.JOB` and `jobScheduling.CRON`. [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

## Sources

- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]]
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js Job Scheduling (17 pages)

## Related Concepts

- [[cron-functions]] — Cron Functions are triggered by Job Scheduling crons
- [[job-functions]] — Job Functions are triggered by Job Scheduling jobs
- [[circuits]] — Circuits can be triggered as job targets
- [[appsail]] — AppSail endpoints can be triggered as job targets
