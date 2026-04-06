---
title: Basic I/O Functions
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [functions, basic-io, serverless, catalyst]
---

# Basic I/O Functions

## Definition

Basic I/O Functions are the simplest [[serverless-functions]] type in [[zoho-catalyst]]. They accept JSON arguments as input and produce JSON output. Suitable for straightforward request-response operations without needing full HTTP framework capabilities. [Source: catalyst-serverless-functions.md]

## Key Aspects

### URL Pattern
```
https://{domain}.catalystserverless.com/server/{function_name}/execute
```
Note: URL ends with `/execute` — distinguishing it from [[advanced-io-functions]] which omit this suffix.

### Execution Limit
- **Timeout**: 30 seconds
- **Output**: Max 1MB
- **Log entries**: Max 1500 characters each

### Module API

**context**:
- `close()` — Terminates function execution
- `log()` — Logs messages (max 1500 chars)

**basicIO**:
- `write()` — Writes output (max 1MB)
- `setStatus()` — Sets response status code
- `getArgument(key)` — Gets a specific input argument
- `getAllArguments()` — (Java only) Gets all arguments
- `getOrigin()` — (Java only) Gets request origin

### When to Use
- Simple request → response patterns
- JSON-only I/O without custom headers
- Quick operations under 30 seconds
- When you don't need routing, middleware, or streaming

## Sources

- [[catalyst-serverless-functions]] — Help documentation

## Related Concepts

- [[advanced-io-functions]] — More capable alternative with full HTTP support
- [[function-urls]] — URL pattern differences between function types
- [[serverless-functions]] — Parent concept
