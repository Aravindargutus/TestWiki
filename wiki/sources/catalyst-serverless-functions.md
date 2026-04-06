---
title: Catalyst Serverless Functions — Help Documentation
type: source
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [serverless, functions, catalyst, help-docs, compute]
---

# Catalyst Serverless Functions — Help Documentation

## Summary

This source covers the **help documentation** (not SDK API reference) for Catalyst Serverless Functions — the core compute layer of the [[zoho-catalyst]] platform. Functions are custom-built serverless code structures containing application business logic. The documentation describes **7 distinct function types**, their runtimes (Java, Node.js, Python), URL patterns, execution limits, module APIs, and use cases.

Unlike the SDK reference pages previously ingested, this is conceptual/operational documentation explaining *what* each function type does, *how* to structure code, and *when* to use each type.

[Source: catalyst-serverless-functions.md]

## Key Takeaways

1. **7 Function Types** with distinct triggers and capabilities: Basic I/O, Advanced I/O, Event, Cron, Browser Logic, Integration, Job
2. **Two execution tiers**: 30-second (HTTP-facing: Basic I/O, Advanced I/O, Browser Logic) and 15-minute (background: Event, Cron, Job)
3. **Only 3 of 7 types have URLs** — Event, Cron, Integration, and Job functions cannot be invoked via HTTP
4. **Advanced I/O is the most capable** — supports Express.js, Servlet, Flask; multiple routes per function; streaming; custom headers
5. **Browser Logic uses Playwright** under the hood for browser automation; no Python support
6. **Integration Functions are US-only** (not available in EU, AU, IN, JP, SA, CA data centers)
7. **Job Functions support Java 7** — the only function type that does, suggesting legacy compatibility
8. **Python has the most limited runtime**: only 3.9, and not supported for Browser Logic
9. **catalyst-config.json** is the universal function configuration file across all runtimes
10. **Basic I/O vs Advanced I/O URL distinction**: Basic I/O ends with `/execute`, Advanced I/O does not

## Function Type Comparison

| Feature | Basic I/O | Advanced I/O | Event | Cron | Browser Logic | Integration | Job |
|---------|-----------|-------------|-------|------|---------------|-------------|-----|
| Has URL | Yes (`/execute`) | Yes (`/`) | No | No | Yes (`/execute`) | No | No |
| Manual Invoke | Yes | Yes | No | No | Yes | No | No |
| Max Timeout | 30s | 30s | 15min | 15min | ~30s | — | 15min |
| Python | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✓ |
| Framework | None | Express/Servlet/Flask | None | None | Playwright | Cliq SDK | None |
| Trigger | HTTP | HTTP | Event Listener | Cron Job | HTTP | Zoho Service | Job Pool |

## Entities Mentioned

- [[zoho-catalyst]] — The serverless platform hosting all function types
- [[catalyst-functions]] — Umbrella concept for all 7 serverless function types
- [[smartbrowz]] — Service powering Browser Logic Functions (Playwright-based)

## Concepts Introduced

- [[serverless-functions]] — Core concept: 7 function types, runtimes, lifecycle
- [[basic-io-functions]] — Simple JSON I/O with `/execute` URL pattern
- [[advanced-io-functions]] — Full HTTP framework support (Express/Servlet/Flask)
- [[event-functions]] — Async functions triggered by Event Listeners
- [[cron-functions]] — Scheduled periodic execution
- [[browser-logic-functions]] — Playwright-based browser automation
- [[integration-functions]] — Backend for Zoho service integrations (Cliq)
- [[job-functions]] — Background tasks via Job Scheduling pools
- [[function-urls]] — URL patterns and accessibility across function types
- [[catalyst-config]] — Universal function configuration file

## Open Questions

1. **What are the exact memory limits per function type?** Documentation mentions Job Functions must have less memory than job pool, but doesn't specify concrete limits for other types.
2. **What is the exact timeout for Browser Logic Functions?** Implied 30s like Basic I/O (same URL pattern) but not explicitly stated.
3. **What are Security Rules details?** Mentioned as a management tool but not detailed in these pages.
4. **What is API Gateway configuration?** Referenced but not covered in function help docs.
5. **How do Circuits (circuit breaker) work?** Listed as a management feature but unexplained.
6. **What are the performance characteristics/cold start times** across runtimes and function types?
7. **Integration Functions**: Will more Zoho services be supported beyond Cliq? Will DC availability expand?
