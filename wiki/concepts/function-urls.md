---
title: Function URLs
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [functions, urls, endpoints, serverless, catalyst]
---

# Function URLs

## Definition

Function URLs are the HTTP endpoints used to invoke [[serverless-functions]] in [[zoho-catalyst]]. Only 3 of 7 function types are URL-accessible. The URL pattern differs between Basic I/O and Advanced I/O functions. [Source: catalyst-serverless-functions.md]

## Key Aspects

### URL Patterns
| Function Type | URL | Suffix |
|---------------|-----|--------|
| [[basic-io-functions]] | `https://{domain}.catalystserverless.com/server/{function_name}/execute` | `/execute` |
| [[advanced-io-functions]] | `https://{domain}.catalystserverless.com/server/{function_name}/` | `/` (no suffix) |
| [[browser-logic-functions]] | `https://{domain}.catalystserverless.com/server/{function_name}/execute` | `/execute` |

### Non-URL Function Types
These 4 types have **no URL** and cannot be invoked via HTTP:
- [[event-functions]] — Triggered by Event Listeners
- [[cron-functions]] — Triggered by Cron jobs
- [[integration-functions]] — Invoked by Zoho services (Cliq)
- [[job-functions]] — Invoked by Job Scheduling

### Important Distinction
The `/execute` suffix is critical: Basic I/O requires it, Advanced I/O does not. Using the wrong pattern will result in errors.

## Sources

- [[catalyst-serverless-functions]] — Help documentation

## Related Concepts

- [[serverless-functions]] — Parent concept
- [[basic-io-functions]], [[advanced-io-functions]] — The two URL patterns
