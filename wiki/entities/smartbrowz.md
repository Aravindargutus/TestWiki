---
title: SmartBrowz
type: entity
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
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

## Appearances

- [[catalyst-serverless-functions]] — Described as the service behind Browser Logic Functions

## Relationships

- Part of [[zoho-catalyst]] platform
- Powers [[browser-logic-functions]]

## Notes

- Mentioned in the platform components list under the Zoho Catalyst entity page
- Limited to 2 runtimes (Java, Node.js) — narrowest runtime support of any Catalyst component
