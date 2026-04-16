---
title: Browser Grid
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-smartbrowz.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
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

## Node.js SDK Access Pattern

```js
const smartbrowz = app.smartbrowz();
const browserGrid = smartbrowz.browserGrid();

await browserGrid.getAllGrids();
await browserGrid.getGrid(gridId);      // or getGrid('gridName')
await browserGrid.getNodes(gridId);
await browserGrid.getNode(gridId, nodeId);
await browserGrid.stopGrid(gridId);     // or stopGrid('gridName')
```

All operations require Admin scope. Lookup by ID or name. [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

## Sources

- [[catalyst-java-sdk-smartbrowz]]
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js Browser Grid (6 pages)

## Related Concepts

- [[browser-logic-functions]] — Functions that use SmartBrowz for browser automation
- [[pdf-screenshot]] — Another SmartBrowz capability
