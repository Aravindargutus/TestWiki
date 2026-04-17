---
title: Automation Testing
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com/en/devops/help/automation-testing]
tags: [catalyst, devops, testing, quality, api-testing]
---

# Automation Testing

## Definition

**Catalyst Automation Testing** is a DevOps component for **automated API test execution** against function endpoints or any third-party URL. It operates as the **Quality** pillar of DevOps, complementing the Monitor tools (Alerts/APM/Logs/Metrics). [Source: docs.catalyst.zoho.com]

## Platform Overview

### Target Endpoints

- Catalyst **[[basic-io-functions]]**, **[[advanced-io-functions]]**, **[[integration-functions]]**, **[[browser-logic-functions]]** (any HTTP-addressable function URL — see [[function-urls]])
- **Any third-party URL** — not limited to the Catalyst project

### Execution

- Runs from the Catalyst console
- Produces pass/fail results with response assertions
- Surfaces problem areas for fix-in-place remediation

### Position in DevOps

Automation Testing is **DevOps → Quality**, separate from:
- **DevOps → Monitor** ([[application-alerts]], [[apm]], [[catalyst-logs]], [[catalyst-metrics]])
- **DevOps → Repositories** (GitHub Integration)

### Complementary Tools

- For deployment automation, pair with **[[catalyst-pipelines]]** (CI/CD)
- For post-deploy monitoring, pair with APM + Logs
- For end-user scenario testing with a browser, see **[[browser-logic-functions]]** (different use case — functions, not tests)

## Key Aspects

- API-level testing (not UI/E2E)
- Works against any URL, including non-Catalyst endpoints
- Runs fully inside the Catalyst console
- Quality pillar of DevOps (distinct from Monitor and Repositories pillars)

## Sources

- [docs.catalyst.zoho.com/en/devops/help/automation-testing/introduction](https://docs.catalyst.zoho.com/en/devops/help/automation-testing/introduction)

## Related Concepts

- [[catalyst-devops]] — Parent DevOps umbrella
- [[catalyst-pipelines]] — CI/CD complement for deployment flow
- [[function-urls]] — URL patterns that tests invoke
- [[api-gateway]] — API management layer for managed endpoints
