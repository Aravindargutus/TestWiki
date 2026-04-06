---
title: Browser Grid
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-smartbrowz.md]
tags: [smartbrowz, browser-grid, headless, automation]
---

# Browser Grid

## Definition

Browser Grid is [[smartbrowz]]'s auto-scaling component for configuring and managing multiple headless browsers. You configure grids with specified numbers of nodes, sessions, and concurrent browsers for automated browser tasks. [Source: catalyst-java-sdk-smartbrowz.md]

## Key Aspects

- **5 SDK operations** (excl. overview): Get instance, get all grids, get specific grid, get node details, stop grid
- **Admin scope required** for all operations
- **Grid properties**: id, name, memory (1024 default), browser versions (Chrome + Firefox), endpoint_type, max_session/nodes/concurrent_count
- **Lookup by ID or name** — consistent pattern across all operations
- **Stop is destructive**: Terminates all executions and stops the grid

## Sources

- [[catalyst-java-sdk-smartbrowz]]

## Related Concepts

- [[browser-logic-functions]] — Functions that use SmartBrowz for browser automation
- [[pdf-screenshot]] — Another SmartBrowz capability
