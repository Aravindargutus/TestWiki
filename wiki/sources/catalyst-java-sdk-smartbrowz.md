---
title: Catalyst Java SDK — SmartBrowz
type: source
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-smartbrowz.md]
tags: [sdk, java, smartbrowz, browser-grid, pdf, screenshot]
---

# Catalyst Java SDK — SmartBrowz

## Summary

This source covers all **7 SmartBrowz SDK pages**: Browser Grid (6: Overview, Get Instance, Get All Grids, Get Specific Grid, Get Node Details, Stop Grid) and PDF & Screenshot (1: Generate from template/HTML/URL). Two separate entry points: `ZCBrowserGrid.getInstance()` for Browser Grid, `ZCSmartBrowz.getInstance()` for PDF/Screenshot.

[Source: catalyst-java-sdk-smartbrowz.md]

## Key Takeaways

1. **Browser Grid** manages headless browser instances for automation — configure nodes, sessions, concurrency.
2. **All Browser Grid operations require Admin scope**.
3. **Grid lookup by ID or name** — consistent pattern across get/getNodes/stopGrid.
4. **PDF & Screenshot** generates visual documents from 3 input types: HTML string, URL, or predefined template.
5. **Rich PDF options**: page format, margins, landscape, scale, password protection, page ranges.
6. **Device emulation** supported via page options (e.g., "Blackberry PlayBook").
7. **Template-based generation** uses Jackson `JsonNode` for template input data.
8. **All methods return `InputStream`** for PDF/Screenshot output.

## SDK Method Reference

| Component | Method | Key Class |
|-----------|--------|-----------|
| Browser Grid | `getInstance()` | ZCBrowserGrid |
| Browser Grid | `getGrid()` (all) | ZCBrowserGrid → List<ZCGrid> |
| Browser Grid | `getGrid(id/name)` | ZCBrowserGrid |
| Browser Grid | `getGridNodes(id/name)` | ZCBrowserGrid |
| Browser Grid | `stopGrid(id/name)` | ZCBrowserGrid |
| PDF & Screenshot | `convertToPdf(details)` | ZCSmartBrowz |
| PDF & Screenshot | `generateFromTemplate(options)` | ZCSmartBrowz |

## Entities Mentioned

- [[zoho-catalyst]] — Platform
- [[smartbrowz]] — SmartBrowz service (existing, update)

## Concepts Introduced

- [[browser-grid]] — Headless browser auto-scaling management (new)
- [[pdf-screenshot]] — Visual document generation from HTML/URL/templates (new)

## Open Questions

1. What are the pricing/limits for Browser Grid nodes?
2. Can Browser Grid be used outside of Functions/AppSail?
3. What screenshot formats are supported (PNG, JPEG)?
