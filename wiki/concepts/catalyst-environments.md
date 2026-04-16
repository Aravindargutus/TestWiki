---
title: Catalyst Environments
type: concept
created: 2026-04-16
updated: 2026-04-16
sources: [catalyst-general-topics.md]
tags: [catalyst, environments, development, production, deployment, billing]
---

# Catalyst Environments

## Definition

Catalyst provides two work environments for every project: a free Development sandbox and a billed Production environment. The deployment process migrates data and configurations from development to production. [Source: catalyst-general-topics.md]

## Key Aspects

### Development Environment
- Free, permanent sandbox at no cost
- For building, configuring, testing, and hosting
- Has certain **feature limitations** per component (e.g., 25 users for authentication, 5,000 rows/table for Data Store)
- CLI (`catalyst deploy`) deploys here
- All iterative development and testing happens here

### Production Environment
- Live mode accessible to end users
- **Billed per API call** after deployment
- Must set up payment method before first production deployment
- Deploy via the web console "Deploy to Production" button

### Deployment Flow
1. Build and test in Development
2. Set up payment method (Billing settings)
3. Deploy to Production from console
4. Can switch between environments in console after deployment
5. Subsequent deployments update production from development

### Billing
- **Two pricing models**: Subscription or Pay-as-you-go
- Free-tier benefits available until hard limits exceeded
- Billing settings: payment methods, transaction reports, budgets

## Sources

- [[catalyst-general-topics]]

## Related Concepts

- [[catalyst-projects]] — Each project has both environments
- [[catalyst-console]] — Environment switching from header
- [[catalyst-cli]] — CLI works with development environment only
- [[appsail]] — Persistent web service in both environments

[Source: catalyst-general-topics.md]
