---
title: Circuits
type: concept
created: 2026-04-06
updated: 2026-04-17
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-overview-serverless.md]
tags: [serverless, circuits, orchestration, workflow, state-machine]
---

# Circuits

## Definition

Circuits is Catalyst's workflow orchestration component that allows defining, organizing, and executing a sequence of tasks (typically function executions) automatically. Circuits support sequential and concurrent execution with conditions, data passing, and configurable paths. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Platform Overview

> Source: [Circuits Introduction](https://docs.catalyst.zoho.com/en/serverless/help/circuits/introduction/) and [Key Concepts](https://docs.catalyst.zoho.com/en/serverless/help/circuits/key-concepts/). The SDK material below covers execution triggers only; this section covers how circuits are **designed** and **modeled**.

A Catalyst circuit is a **JSON file** defining a state machine of tasks. Circuits can be designed two ways in the console:
1. **Visual designer** — drag-and-drop elements
2. **Code view** — edit the circuit JSON directly

Circuits can be executed from the console (manual/test), via API/SDK, or through the circuit's **Circuit URL**:
```
POST https://{project_domain}.{environment_type}.zohocatalyst.com/baas/v1/project/{project_id}/circuit/{circuit_id}/execute
```
(`environment_type` is `development` in dev; omitted in production.)

### What Can Run Inside a Circuit
- **Only [[basic-io-functions]]** — circuits use JSON in/out, which is native to Basic I/O
- **Cannot execute** Cron, Event, or Advanced I/O functions
- Nested circuits are supported (Circuit state)

### Circuit States

Every state has `Previous state` and `Next state` properties; execution traverses state-by-state, passing JSON between them.

**Flow Control states** — orchestrate logic/data:
| State | Purpose |
|---|---|
| `Pass` | Transform or pass payload through |
| `Branch` | Conditional routing (if/else logic) |
| `Parallel` | Execute multiple branches concurrently |
| `Wait` | Delay before next state |
| `Batch` | Execute a function/circuit N times in parallel (fan-out) |
| `Success` | Terminal success state |
| `Failure` | Terminal failure state |

**Functional states** — actual work:
| State | Purpose |
|---|---|
| `Function` | Invoke a Basic I/O function |
| `Circuit` | Invoke another circuit (nested) |

### Data Path Configuration
Function and Pass states support manipulating their JSON payload via:
- **Input path** — subset of incoming payload to use
- **Output path** — subset of state's result to emit
- **Result path** — where to merge the result back into the parent payload

By default, a state's output is passed as the next state's payload with no configuration.

### Availability
**Not available** in EU, AU, IN, JP, SA, or CA data centers (same as [[integration-functions]]).

### Tooling
- Console: visual designer, code view, test runner with detailed execution logs
- SDKs: Java, Node.js, Python (execute / status / abort — see below)
- REST API

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
