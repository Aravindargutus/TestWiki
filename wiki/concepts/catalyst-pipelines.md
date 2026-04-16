---
title: Catalyst Pipelines
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [pipelines, ci-cd, deployment]
---

# Catalyst Pipelines

## Definition

Catalyst Pipelines implements CI/CD automation for building, testing, and deploying web/mobile applications. Pipelines are created in the console and can be queried and executed via the SDK. [Source: catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]

## Key Aspects

- **3 SDK operations**: Get instance, get pipeline details, execute pipeline
- **Entry point**: `ZCPipeline.getInstance()`
- **Execute with**: Pipeline ID + branch name + optional env vars (JSONObject)
- **Returns run history**: history_id, pipeline_id, event_time, history_status
- **Pipeline statuses**: Active (from details), Queued (from execution)

## Node.js SDK Access Pattern

```js
const pipeline = app.pipeline();
const details = await pipeline.getPipelineDetails('pipelineId');
const result = await pipeline.runPipeline('pipelineId', 'main', { EVENT: 'push', URL: 'https://example.com' });
```

Returns: `{ history_id, pipeline_id, event_time, event_details, history_status: 'Queued' }`. [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

**Key difference**: Java uses `ZCPipeline.getInstance()` + `getPipelineInstance(id)` two-step; Node.js directly calls `app.pipeline()` methods.

## Sources

- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]]
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js Pipelines (3 pages)

## Related Concepts

- [[appsail]] — Pipelines can deploy to AppSail
- [[serverless-functions]] — Pipelines can deploy functions
