---
title: Advanced I/O Functions
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [functions, advanced-io, serverless, catalyst, express, flask, servlet]
---

# Advanced I/O Functions

## Definition

Advanced I/O Functions provide native HTTP request/response handling in [[zoho-catalyst]], supporting full web frameworks. They allow multiple API endpoints within a single function, streaming responses, custom headers, view rendering, and file downloads. The most capable HTTP-facing [[serverless-functions]] type. [Source: catalyst-serverless-functions.md]

## Key Aspects

### URL Pattern
```
https://{domain}.catalystserverless.com/server/{function_name}/
```
Note: URL does NOT end with `/execute` — unlike [[basic-io-functions]].

### Execution Limit
- **Timeout**: 30 seconds

### Framework Support
| Runtime | Framework |
|---------|-----------|
| Node.js | Express.js (recommended) or blank template |
| Java | javax.servlet (HttpServlet) |
| Python | Flask |

### Capabilities vs Basic I/O
- Multiple API routes in one function
- Custom HTTP headers
- Streaming responses
- File downloads
- View rendering (HTML templates)
- Full middleware support
- Path parameters and query strings

### Node.js Templates
1. **Express.js template** — Pre-configured with Express routing
2. **Blank template** — Minimal setup for custom frameworks

### When to Use
- REST APIs with multiple endpoints
- Custom HTTP headers or status codes
- Streaming or large response bodies
- HTML rendering or file serving
- Any scenario needing full web framework capabilities

## Sources

- [[catalyst-serverless-functions]] — Help documentation

## Related Concepts

- [[basic-io-functions]] — Simpler alternative for JSON-only I/O
- [[function-urls]] — URL pattern differences
- [[serverless-functions]] — Parent concept
