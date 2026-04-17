---
title: Language Runtime Support Policy
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [catalyst-serverless-functions.md]
tags: [functions, runtime, deprecation, lifecycle, nodejs, java, python]
---

# Language Runtime Support Policy

## Definition

Catalyst's formal policy for deprecating and retiring language runtimes used in [[serverless-functions]]. Applies primarily to **Node.js**, though the same framework covers Java and Python versions. A runtime moves through 4 phases from end-of-life announcement to full removal. [Source: [Runtime Support Policy](https://docs.catalyst.zoho.com/en/serverless/help/functions/runtime-support/)]

## Key Aspects

### The 4 Phases

| Phase | Duration | What You Can Do |
|---|---|---|
| **1. Deprecation Announcement** | Instant, 6 months before community EOL | Plan upgrade; no impact to existing functions |
| **2. Deprecation Period** | 1 year | Create and update functions normally; must plan migration |
| **3. Retirement Period** | 3 months | **Cannot create** new functions on the deprecated runtime; existing functions still run and can be updated |
| **4. End of Support** | Indefinite | **Cannot create or update**; function invocations no longer guaranteed; no security patches applied |

Total notice period from announcement to end-of-support: **~21 months** (6 months pre-announcement + 12 months deprecation + 3 months retirement).

### Example Timeline
If a community EOL is announced for April 1, 2022:
- Oct 1, 2021 — Catalyst announces deprecation
- Oct 1, 2022 — Deprecation period ends; retirement begins
- Jan 1, 2023 — Retirement ends; end-of-support begins

### Currently Supported Runtimes

See [[serverless-functions]] for the full list. Key points:
- **Java**: 8, 11, 17
- **Node.js**: 20, 18, 16, 14, 12 (v12 is past EOL)
- **Python**: 3.9 only

### Retirement Exceptions Noted in Docs
- **Node.js v10**: Deprecation since April 2021 → retired Jan 31, 2023
- **Node.js v12**: Deprecation until April 30, 2023 → retired July 31, 2023

### After End of Support
- Existing deprecated functions may or may not continue executing
- No new security patches applied
- Catalyst still allows you to **upgrade the runtime version** of an existing function at any time from the console

## Recommendations
- Always build on the latest stable runtime
- Treat the deprecation announcement as the trigger to start planning migration
- Use [Catalyst Logs](../../raw/catalyst-general-topics.md) and APM to validate upgraded functions

## Sources

- [[catalyst-serverless-functions]] — Runtime Support Policy sub-page

## Related Concepts

- [[serverless-functions]] — Parent concept
- [[cost-optimization]] — Often considered together when picking a runtime/memory combo
