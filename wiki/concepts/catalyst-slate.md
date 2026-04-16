---
title: Catalyst Slate
type: concept
created: 2026-04-16
updated: 2026-04-16
sources: [catalyst-general-topics.md]
tags: [catalyst, slate, frontend, deployment, javascript, git]
---

# Catalyst Slate

## Definition

Catalyst Slate is a frontend deployment platform for launching scalable web applications. It provides native support for JavaScript frameworks, Git-based automation, custom domain mapping, and deployment management features like rollback and previews. [Source: catalyst-general-topics.md]

## Key Aspects

### Deploy Options (5 methods)
1. **Slate Starter Templates** — Quick deployment with ready-to-use templates
2. **Private Repository** — Deploy from a private Git repo
3. **Public Repository** — Deploy from a public Git repo
4. **Direct Upload** — Upload build zip files
5. **CLI** — `catalyst` CLI commands for Slate deployment

### Configuration
- **Cache** — Control caching settings per deployment
- **Memory** — Allocate memory for Next.js framework apps
- **Environment Variables** — Secure management of deployment env vars
- **Custom Domains** — Map deployments to custom domain names

### Management
- **Slate Access URL** — Generated URL for collaboration
- **Deployment Previews** — Instant preview of every deployment
- **Rollback Deployment** — Revert to previous deployment version
- **Fix Failed Deployment** — Apply latest commit to fix issues
- **Monitor Deployment** — Track logs and deployment history
- **Delete Deployment** — Remove when no longer needed

### Supported Frameworks
- Next.js (with memory allocation support)
- React, Vue, and other JavaScript frameworks

## Sources

- [[catalyst-general-topics]]

## Related Concepts

- [[appsail]] — Server-side persistent hosting (vs Slate for frontend)
- [[catalyst-pipelines]] — CI/CD for backend deployment (complements Slate for frontend)
- [[catalyst-environments]] — Slate deployments exist within project environments

[Source: catalyst-general-topics.md]
