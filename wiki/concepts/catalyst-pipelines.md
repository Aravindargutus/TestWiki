---
title: Catalyst Pipelines
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]
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

## Sources

- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]]

## Related Concepts

- [[appsail]] — Pipelines can deploy to AppSail
- [[serverless-functions]] — Pipelines can deploy functions
