---
title: Browser Logic Functions
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [functions, browser, playwright, smartbrowz, serverless, catalyst]
---

# Browser Logic Functions

## Definition

Browser Logic Functions are [[serverless-functions]] in [[zoho-catalyst]] that enable browser automation via the [[smartbrowz]] service. They use Playwright under the hood for web scraping, PDF generation, screenshot capture, and headless browser testing. [Source: catalyst-serverless-functions.md]

## Key Aspects

### Runtimes
- **Java** and **Node.js** only — **Python is NOT supported**

### URL Pattern
Same as [[basic-io-functions]]:
```
https://{domain}.catalystserverless.com/server/{function_name}/execute
```

### Function Signature (Node.js)
```javascript
module.exports.playwright = async (request, response, page) => {
  // `page` is a Playwright page object
};
```
- Export must be `module.exports.playwright`
- The `page` parameter is a pre-configured Playwright page object
- SmartBrowz manages the browser instance lifecycle

### Capabilities
- Web scraping (extract data from websites)
- PDF generation from web pages
- Screenshot capture
- Headless browser testing
- Form filling and submission
- Navigation and interaction automation

### When to Use
- Generating PDFs or screenshots of web pages
- Scraping data from rendered (JavaScript-heavy) websites
- Automated testing of web UIs
- Any task requiring a full browser context

## Sources

- [[catalyst-serverless-functions]] — Help documentation

## Related Concepts

- [[serverless-functions]] — Parent concept
- [[smartbrowz]] — Underlying browser automation service
- [[basic-io-functions]] — Shares the same URL pattern (`/execute`)
