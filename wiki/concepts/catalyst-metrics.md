---
title: Catalyst Metrics
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com/en/devops/help/metrics]
tags: [catalyst, devops, monitoring, metrics, usage, resources]
---

# Catalyst Metrics

## Definition

**Catalyst Metrics** is a DevOps component that surfaces **usage metrics** for project resources through graphs and charts — a project-wide resource consumption dashboard distinct from per-function performance tracking. [Source: docs.catalyst.zoho.com]

## Platform Overview

### Scope

Unlike [[apm]] (function-execution performance) and [[catalyst-logs]] (execution records), Metrics focuses on **resource utilization** at the component and project level — API call volume, storage consumption, and similar rate/quantity indicators across Catalyst components.

### Presentation

- Graph- and diagram-based dashboards
- Filterable by time range and component
- Drives capacity planning and [[cost-optimization]] decisions

### Companion Role

Metrics is one of four monitoring tools within [[catalyst-devops]]:
1. **[[application-alerts]]** — notification layer
2. **[[apm]]** — function-level deep performance
3. **[[catalyst-logs]]** — raw execution records
4. **Metrics** — aggregate resource usage

## Key Aspects

- Resource-usage focus, not function-execution focus
- Complementary to APM + Logs, not a replacement
- Feeds billing awareness (maps to consumption → cost)

## Sources

- [docs.catalyst.zoho.com/en/devops/help/metrics/introduction](https://docs.catalyst.zoho.com/en/devops/help/metrics/introduction)

## Related Concepts

- [[catalyst-devops]] — Parent DevOps umbrella
- [[apm]] — Per-function performance (different axis)
- [[catalyst-logs]] — Raw execution events (different axis)
- [[cost-optimization]] — Metrics drive cost tuning
- [[catalyst-environments]] — Dev vs Prod consumption split
