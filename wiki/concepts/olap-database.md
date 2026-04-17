---
title: OLAP Database
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com/en/cloud-scale/help/data-store/olap-database]
tags: [catalyst, data-store, olap, analytics, business-intelligence, zcql, read-only]
---

# OLAP Database

## Definition

Catalyst's **OLAP (Online Analytical Processing) database** is a built-in **secondary, read-optimized** database that sits alongside the primary [[data-store]] transactional (OLTP) engine. It exists specifically to execute **analytical ZCQL queries** on multidimensional data at scale without impacting transactional workloads. [Source: docs.catalyst.zoho.com]

## Platform Overview

### Architecture

- **Primary Data Store** = OLTP engine → full CRUD, transaction-optimized
- **OLAP Database** = secondary engine → **read-only**, analytics-optimized
- Data is **auto-synchronized** one-way from the primary Data Store to OLAP after a **single one-time enable** action
- Queried through the same [[zcql]] SDK/API surface

### Capability Matrix

| Aspect | Primary Data Store (OLTP) | OLAP Database |
|---|---|---|
| Workload type | Transactional | Analytical / ad-hoc |
| Operations | Full CRUD | **Read only** (SELECT) |
| Optimization | Balanced CRUD | Read-heavy, multidimensional |
| ZCQL built-in functions | Supported | Supported (advanced analytics focus) |
| Direct writes | Yes | **Not allowed** |

### Query Surface

- Uses [[zcql]] with its built-in function library for aggregations, arithmetic, and analytical transformations
- Access via platform SDKs (Java / Node.js / Python) and REST APIs — same as Data Store
- Suited for SELECT queries returning aggregated / complex result sets

### Use Cases

- Aggregated report generation
- Real-time dashboards and analytics
- Financial analysis (budgeting, forecasting)
- Sales and marketing performance analysis
- Inventory and logistics management
- Data mining and trend analysis

### Enablement

- One-time console action enables sync
- After enable, OLAP mirrors the primary store and accepts read queries directly

## Key Aspects

- Secondary read-only database — not a replacement for Data Store
- ZCQL-only access; built-in functions are the primary analytics surface
- Designed for BI and dashboards, not OLTP workloads
- Auto-sync from primary Data Store handled by Catalyst

## Sources

- [docs.catalyst.zoho.com/en/cloud-scale/help/data-store/olap-database/introduction](https://docs.catalyst.zoho.com/en/cloud-scale/help/data-store/olap-database/introduction/)

## Related Concepts

- [[data-store]] — Primary OLTP source database
- [[zcql]] — Query language used for both engines
- [[bulk-operations]] — Mass data loads into Data Store (syncs to OLAP)
- [[nosql]] — Alternative storage for non-relational workloads (different category)
