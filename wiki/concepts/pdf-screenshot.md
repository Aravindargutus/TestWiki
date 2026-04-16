---
title: PDF & Screenshot
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-smartbrowz.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [smartbrowz, pdf, screenshot, document-generation]
---

# PDF & Screenshot

## Definition

PDF & Screenshot is a [[smartbrowz]] component for programmatically generating visual documents (PDFs, screenshots) from HTML content, URLs, or predefined templates. [Source: catalyst-java-sdk-smartbrowz.md]

## Key Aspects

- **3 input types**: HTML string, URL, predefined template (with template ID + JSON data)
- **2 SDK methods**: `convertToPdf(details)`, `generateFromTemplate(options)`
- **Rich options**: Page format (A4 etc.), landscape, scale, margins, password, page ranges, header/footer
- **Device emulation**: Set device name for viewport simulation
- **JavaScript control**: Enable/disable JS rendering
- **Returns InputStream**: Caller handles the output stream
- **Template system**: Templates created in console; data passed as Jackson JsonNode

## Node.js SDK Access Pattern

```js
const smartbrowz = app.smartbrowz();
const pdfResult = await smartbrowz.generatePDF(options);
const screenshotResult = await smartbrowz.generateScreenshot(options);
```

Node.js SDK provides separate `generatePDF()` and `generateScreenshot()` methods (vs Java's `convertToPdf()` and `generateFromTemplate()`). [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

## Sources

- [[catalyst-java-sdk-smartbrowz]]
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js PDF & Screenshot

## Related Concepts

- [[browser-grid]] — Another SmartBrowz capability (browser management)
- [[browser-logic-functions]] — SmartBrowz context for function execution
