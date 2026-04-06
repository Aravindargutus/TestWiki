---
title: PDF & Screenshot
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-smartbrowz.md]
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

## Sources

- [[catalyst-java-sdk-smartbrowz]]

## Related Concepts

- [[browser-grid]] — Another SmartBrowz capability (browser management)
- [[browser-logic-functions]] — SmartBrowz context for function execution
