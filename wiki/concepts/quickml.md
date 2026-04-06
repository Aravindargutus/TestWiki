---
title: QuickML
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]
tags: [quickml, ml, machine-learning, predictions]
---

# QuickML

## Definition

QuickML is Catalyst's no-code ML pipeline builder that lets you build, train, and publish ML models with authenticated endpoints. The SDK provides a single `predict()` method to send input data and receive predictions. [Source: catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]

## Key Aspects

- **1 SDK operation**: `predict(endpointKey, map)` via `ZCQuickML.getInstance()`
- **Input**: HashMap of column name → value pairs matching the training dataset
- **Endpoint key**: Unique ID from published ML model in console
- **Data center restriction**: **Not available in JP, SA, or CA**
- **Prerequisite**: ML pipeline + model endpoint must be configured and published in console first

## Sources

- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]]

## Related Concepts

- [[zia-services]] — AutoML is the Zia alternative; QuickML is the no-code pipeline approach
