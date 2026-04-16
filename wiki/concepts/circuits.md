---
title: Circuits
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-overview-serverless.md]
tags: [serverless, circuits, orchestration, workflow]
---

# Circuits

## Definition

Circuits is Catalyst's workflow orchestration component that allows defining, organizing, and executing a sequence of tasks (typically function executions) automatically. Circuits support sequential and concurrent execution with conditions, data passing, and configurable paths. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Aspects

- **3 SDK operations**: Execute circuit, get execution details, abort execution
- **Execution model**: Submit with name + JSON input → get execution ID → poll status or abort
- **Status tracking**: `ZCCircuitExecutionStatus` enum (includes SUCCESS)
- **Abort capability**: Can cancel a running circuit execution
- **Data center restriction**: **Not available in EU, AU, IN, JP, SA, or CA** data centers (same restriction as [[integration-functions]])

## SDK Entry Point

```java
ZCCircuitDetails circuit = ZCCircuit.getInstance().getCircuitInstance(circuitId);
ZCCircuitExecutionDetails exec = circuit.execute("Case 1", inputJson);
String execId = exec.getExecutionId();

// Check status:
ZCCircuitExecutionDetails details = circuit.getExecutionDetails(execId);

// Abort:
circuit.abortExecution(execId);
```

## Node.js SDK Access Pattern

```js
const circuit = app.circuit();

// Execute
const result = await circuit.execute('195000000041001', 'sampleName', { name: 'Aaron Jones' });

// Check status
const status = await circuit.status('195000000041001', '195000000043002');

// Abort
await circuit.abort('195000000041001', '195000000043002');
```

**Same restriction**: Not available in EU, AU, IN, JP, SA, or CA data centers. [Source: catalyst-nodejs-sdk-overview-serverless.md]

**Key difference**: Java uses `ZCCircuit.getInstance().getCircuitInstance(id)` then `.execute()` / `.getExecutionDetails()` / `.abortExecution()`. Node.js uses a flatter API: `app.circuit()` then `.execute()` / `.status()` / `.abort()` with both circuit ID and execution ID as parameters.

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]
- [[catalyst-nodejs-sdk-overview-serverless]] — Node.js Circuits (2 pages)

## Related Concepts

- [[serverless-functions]] — Circuits orchestrate function executions
- [[integration-functions]] — Shares the same data center restrictions
- [[cron-functions]] — Alternative for scheduled sequential tasks (but no orchestration logic)

## Evolution

- Circuits was mentioned in [[serverless-functions]] overview as a management tool alongside Security Rules, API Gateway, Logs, and APM. This source provides the SDK operations.
