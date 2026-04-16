---
title: QuickML
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
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

## Node.js SDK Access Pattern

```js
const quickml = app.quickML();
const result = await quickml.predict('{endpoint_key}', { col1: 'val1', col2: 'val2' });
// Returns: { status: 'success', result: [...] }
```

Same restriction: Not available in JP, SA, CA. [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

**Key difference**: Java uses `ZCQuickML.getInstance().predict(key, hashMap)` with HashMap; Node.js uses a plain JSON object.

## Sources

- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]]
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js QuickML (1 page)

## Related Concepts

- [[zia-services]] — AutoML is the Zia alternative; QuickML is the no-code pipeline approach
