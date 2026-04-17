---
title: Cron
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [catalyst-serverless-functions.md]
tags: [cloud-scale, cron, scheduler, jobs, catalyst]
---

# Cron

## Definition

Cron is a Catalyst Cloud Scale component acting as a **job scheduler**. It runs a background daemon that continuously listens for job schedules and triggers them at configured times. Cron enables time-based workflow automation without manual effort. [Source: [Cron Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/cron/introduction/)]

> **Note**: Catalyst now offers **[[job-scheduling-sdk|Job Scheduling]]** in Early Access as a significant upgrade to Cloud Scale Cron. Access requires emailing `support@zohocatalyst.com`.

## Distinction: Cron (scheduler) vs [[cron-functions]]
- **Cron** = the scheduling component that fires on time (this page)
- **Cron Function** = one possible target type — a serverless function invoked by a cron

## Key Aspects

### Schedule Types
- **One-time**: Execute once at a specific date/time
- **Recurring**: Execute on recurring intervals (monthly on a date, daily at a time, etc.)

### Supported Target Types
A cron can automatically invoke any of:
| Target | Description |
|---|---|
| **Third-party URL** | Any external URL; can POST/modify data at the endpoint |
| **[[cron-functions|Cron Function]]** | Custom function for backup, periodic sync, etc. |
| **[[circuits|Circuit]]** | Orchestrated workflow (JSON input passed in) |

### Use Cases (from docs)
- Periodically sync Catalyst data with a third-party application
- Update a cache item's value at regular intervals
- Send monthly system-generated emails to application users
- Administrative / maintenance tasks

### Console Capabilities
- Create and configure cron jobs visually
- View cron resource statistics
- Configure **Application Alerts** — email alerts on cron failure, code exception, or timeout

### Relationship with [[job-scheduling-sdk|Job Scheduling]] (Early Access)
In the newer Job Scheduling service, crons submit jobs to a **Job Pool** which queues and executes them. A cron strictly acts as a scheduler — the Job Pool invokes the configured target (webhook / [[circuits]] / [[job-functions]] / [[appsail]] service). This expands beyond the classic Cron component's three target types.

**Cron Expressions**: Job Scheduling uses regex-like cron expressions (configurable via console Builder or SDK) for defining schedules. Builder supports Pre-Defined and Dynamic crons; Dynamic crons are recommended to be created via SDK in production.

## Sources
- [Cron Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/cron/introduction/)
- [[catalyst-serverless-functions]] — Cron Functions section
- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]] — Job Scheduling successor

## Related Concepts
- [[cron-functions]] — Function type targeted by crons
- [[circuits]] — Alternative cron target
- [[job-scheduling-sdk]] — Upgraded scheduling service (Early Access)
- [[catalyst-devops]] — Application Alerts for cron failures
