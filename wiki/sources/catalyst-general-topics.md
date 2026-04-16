---
title: Catalyst General Topics — Getting Started, Services, DevOps, Deployment
type: source
created: 2026-04-16
updated: 2026-04-16
source_file: raw/catalyst-general-topics.md
tags: [catalyst, general, getting-started, cli, projects, organizations, environments, devops, convokraft, signals, slate, deployment, billing]
---

# Catalyst General Topics — Getting Started, Services, DevOps, Deployment

## Summary

This source covers the general Catalyst platform documentation from `docs.catalyst.zoho.com/en/` — everything outside the SDK-specific and service-specific help pages. It spans 10+ sub-pages: platform introduction, quick start guide, CLI installation, projects, console navigation, organizations, environments/deployment/billing, and overviews of four services not previously covered (DevOps, ConvoKraft, Signals, Slate).

The documentation reveals Catalyst as an **11-service platform** (not 7 or 9 as previously understood from SDK docs alone): Serverless, Cloud Scale, Zia Services, DevOps, SmartBrowz, QuickML, ConvoKraft, Job Scheduling, Slate, Pipelines, and Signals. It also establishes the complete development lifecycle: console setup → CLI init → develop → test locally → deploy dev → deploy production.

## Key Takeaways

### Platform Overview
- Catalyst supports **4 client SDKs** (Web v4, Android v2, iOS v2, Flutter v2) and **3 server-side languages** (Java, Node.js, Python)
- **Two pricing models**: Subscription or Pay-as-you-go, plus free-tier benefits
- Max **50 projects per account** (increase by request)
- One web/Android/iOS app per project, but multiple functions and AppSail solutions
- All solutions in same project share backend data (Data Store, File Store, etc.)

### CLI
- Package: `zcatalyst-cli` via npm/yarn
- Requires: Node.js v14+
- Key commands: `catalyst login`, `catalyst init`, `catalyst serve` (local test), `catalyst deploy`
- CLI works with development environment only — production deploy via console
- Supports Windows, macOS, Linux

### Organizations
- Multi-org management from single account — each org has unique Org ID
- Default org used for all login and API calls unless explicitly overridden
- Collaborator types: Owner (creator), Admin, Project Member
- Deleting an org permanently removes all its projects

### Environments
- **Development**: Free sandbox, always available, has feature limitations
- **Production**: Live mode, billed per API call
- Must set up payment method before first production deployment

### New Services Discovered
- **Catalyst DevOps**: Application Alerts, APM, Logs, Metrics, GitHub Integration, Automation Testing
- **Catalyst ConvoKraft**: AI chatbot builder with Bots, Actions, Handlers, Bot Operations — embeddable in Catalyst apps
- **Catalyst Signals**: Event Bus Service — Publishers, Targets, Events, Webhooks, Rules for event-driven architectures
- **Catalyst Slate**: Frontend deployment platform — Git-based deployments, JS framework support, custom domains, rollback, previews

## Entities Mentioned
- [[zoho-catalyst]] — the platform (enriched with service count + lifecycle details)
- [[catalyst-cli]] — the `zcatalyst-cli` npm package (NEW)

## Concepts Introduced
- [[catalyst-projects]] — project structure, limits, cross-platform development (NEW)
- [[catalyst-console]] — web console navigation, dashboards, metrics (NEW)
- [[catalyst-organizations-general]] — multi-org management (enriches existing [[catalyst-organizations]])
- [[catalyst-environments]] — development vs production environments (NEW)
- [[catalyst-devops]] — monitoring, testing, quality service (NEW)
- [[catalyst-convokraft]] — AI conversational bot service (NEW)
- [[catalyst-signals]] — event bus service (NEW)
- [[catalyst-slate]] — frontend deployment platform (NEW)
- [[catalyst-cli-concept]] — CLI tool for development lifecycle (NEW)

## Open Questions
- What are the exact free-tier limits per component in development environment?
- What are the two pricing plans' specific rates?
- What GitHub features does DevOps integration support beyond deployment?
- Does ConvoKraft support voice-based bots or only text?
- How does Signals compare to Event Functions as event-driven mechanisms?
- What JS frameworks does Slate officially support?
- What are the collaborator permission differences (Admin vs Project Member)?

[Source: catalyst-general-topics.md]
