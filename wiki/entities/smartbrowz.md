---
title: SmartBrowz
type: entity
created: 2025-07-10
updated: 2026-04-16
sources: [catalyst-serverless-functions.md, catalyst-java-sdk-smartbrowz.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [smartbrowz, browser, playwright, catalyst]
---

# SmartBrowz

## Overview

SmartBrowz is a [[zoho-catalyst]] service that provides browser automation capabilities. It powers [[browser-logic-functions]], managing Playwright browser instances for web scraping, PDF generation, screenshot capture, and headless browser testing. [Source: catalyst-serverless-functions.md]

## Key Facts

- Uses Playwright as the underlying browser automation framework
- Manages browser instance lifecycle (developers get a pre-configured `page` object)
- Supports Java and Node.js runtimes only (no Python)
- Accessible via Browser Logic Functions with URL ending in `/execute`
- **3 sub-components**: Browser Grid, PDF & Screenshot, Dataverse

## SDK Access

| SDK | Instance | Browser Grid | PDF/Screenshot | Dataverse |
|-----|----------|-------------|----------------|----------|
| Java | `ZCSmartBrowz.getInstance()` | `getGrid()`, `getAllGrids()`, `getNode()`, `stopGrid()` | `convertToPdf()`, `generateFromTemplate()` | _Not documented_ |
| Node.js | `app.smartbrowz()` | `browserGrid().getAllGrids()`, `.getGrid()`, `.getNodes()`, `.stopGrid()` | `generatePDF(options)`, `generateScreenshot(options)` | `getEnrichedLead()`, `findTechStack()`, `getSimilarCompanies()` |

**Dataverse** is unique to the Node.js SDK documentation — it provides web data extraction: Lead Enrichment, Tech Stack Finder, and Similar Companies. [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

## Appearances

- [[catalyst-serverless-functions]] — Described as the service behind Browser Logic Functions
- [[catalyst-java-sdk-smartbrowz]] — Java SDK: Browser Grid + PDF/Screenshot (7 pages)
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js SDK: Browser Grid + PDF/Screenshot + Dataverse (9 pages)

## Relationships

- Part of [[zoho-catalyst]] platform
- Powers [[browser-logic-functions]]

## Notes

- Mentioned in the platform components list under the Zoho Catalyst entity page
- Limited to 2 runtimes (Java, Node.js) — narrowest runtime support of any Catalyst component
