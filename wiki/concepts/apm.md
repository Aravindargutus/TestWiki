---
title: Application Performance Monitoring (APM)
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com/en/devops/help/apm]
tags: [catalyst, devops, monitoring, apm, performance, functions]
---

# Application Performance Monitoring (APM)

## Definition

**Catalyst APM** is a DevOps component that provides **deep performance insights** into [[serverless-functions]] executions — invocations, errors, response times, slowest calls, and component-usage drill-downs. It is a performance-tier enhancement over [[catalyst-logs]]. [Source: docs.catalyst.zoho.com]

## Platform Overview

### Supported Function Types & Runtimes

- Monitors **5 function types**: Basic I/O, Advanced I/O, Cron, Event, Browser Logic
- Supports **Java** and **Node.js** only
- **Python is NOT supported** — Python function executions are not tracked by APM

### Regional Restriction

- **Not available in the CA (Canada) data center** (`accounts.zohocloud.ca`)

### Analytics Delivered

- Per-function graphs: **invocation count**, **error count**, **response time** over a selected period
- **Aggregated response-time report** across all functions
- **Top 100 slowest function calls** for the selected period
- **Invocation list** with per-call metadata and response details
- **Component-usage drill-down**: which components each function calls, slowest-performing components, visual execution traces

### Enablement

- Must be **explicitly enabled** per project from the console
- Can be disabled/re-enabled anytime
- Once enabled, data flows to Catalyst servers and is accessible from the APM console

### APM vs Logs

- **[[catalyst-logs]]**: execution records, errors, push-to-log statements — basic visibility
- **APM**: full-performance view with time-series, traces, and component-call analysis — a superset for performance tuning

## Key Aspects

- Java + Node.js only; Python excluded
- CA DC exclusion (same restriction class as [[integration-functions]])
- Project-level opt-in toggle, not always-on
- Enhances (does not replace) Catalyst Logs

## Sources

- [docs.catalyst.zoho.com/en/devops/help/apm/introduction](https://docs.catalyst.zoho.com/en/devops/help/apm/introduction)

## Related Concepts

- [[catalyst-devops]] — Parent DevOps umbrella
- [[catalyst-logs]] — Basic logging, complementary
- [[application-alerts]] — Alerting layer
- [[serverless-functions]] — Monitored subject
- [[catalyst-environments]] — Available in both envs (subject to CA DC exclusion)
