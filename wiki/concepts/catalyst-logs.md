---
title: Catalyst Logs
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com/en/devops/help/logs]
tags: [catalyst, devops, monitoring, logs, access-logs, application-logs]
---

# Catalyst Logs

## Definition

**Catalyst Logs** is a DevOps component that records the execution history of **all [[serverless-functions]]** in a project. Logs capture both successful and failed invocations — including internally-invoked calls — to surface bugs and runtime errors. [Source: docs.catalyst.zoho.com]

## Platform Overview

### Runtime Support

Native logging for **Java, Node.js, and Python** across **all 6 function types**: Basic I/O, Advanced I/O, Event, Cron, Integration, Browser Logic. (Contrast: [[apm]] only supports Java/Node.js and 5 types.)

### Two Log Categories

- **Access Logs** — external-access-layer entries for functions invoked manually from outside the app
  - Applies only to: Basic I/O, Advanced I/O, Integration, Browser Logic
- **Application Logs** — internal-application-layer entries for both user-invoked AND internally-invoked calls
  - Applies to **all** function types

### Push to Logs

Code can emit custom entries via a single-line statement inside the function body. Supports **severity levels**:
- Standard: `Info`, `Error`, `Severe`, `Warning`
- Node.js only: `Uncaught Exception`, `Unhandled Rejection`

### Retention

- **Development environment**: 7 days
- **Production environment**: 14 days

### Console Features

- Filter by function, log type, severity, keyword
- **Managing Alerts in Logs** — integrates with [[application-alerts]] for keyword-based automated searches
- Access Logs and Application Logs are viewed as separate tabs per function

### Logs vs APM

Logs give the raw record; **[[apm]]** layers on response times, invocation counts, component call traces, and top-N slowest reports on top of the execution history.

## Key Aspects

- Supports Java, Node.js, and Python (unlike APM)
- 7/14 day retention split dev/prod
- Access Logs: only user-invoked paths (4 function types)
- Application Logs: all function types, all invocation paths
- Integrates with Application Alerts for keyword-based email notification

## Sources

- [docs.catalyst.zoho.com/en/devops/help/logs/introduction](https://docs.catalyst.zoho.com/en/devops/help/logs/introduction)

## Related Concepts

- [[catalyst-devops]] — Parent DevOps umbrella
- [[apm]] — Enhanced performance layer over Logs
- [[application-alerts]] — Consumes log searches
- [[serverless-functions]] — Logs target
- [[basic-io-functions]] / [[advanced-io-functions]] / [[integration-functions]] / [[browser-logic-functions]] — Functions with both Access + Application logs
- [[cron-functions]] / [[event-functions]] — Application Logs only
