---
title: Catalyst Projects
type: concept
created: 2026-04-16
updated: 2026-04-16
sources: [catalyst-general-topics.md]
tags: [catalyst, projects, console, getting-started]
---

# Catalyst Projects

## Definition

A Catalyst project is the top-level organizational unit for building applications on the [[zoho-catalyst]] platform. Projects are not service-specific — all Catalyst services and components are accessible within a single project. Each project gets a unique Project ID and supports development and production environments. [Source: catalyst-general-topics.md]

## Key Aspects

- **Not service-specific**: Access all Catalyst services within one project
- **Platform limits**: 1 web app, 1 Android app, 1 iOS app per project. Multiple functions and AppSail solutions allowed
- **Cross-platform**: Supported within a single project
- **Shared backend**: All solutions in same project share Data Store, File Store, Functions, etc.
- **Account limit**: Max 50 projects per account (increase via `support@zohocatalyst.com`)
- **Naming**: Alphanumeric, underscores, and hyphens only — no spaces or special characters
- **Environments**: Each project has development (free sandbox) and production (billed) environments
- **Creation**: From web console (required for first project) or CLI (`catalyst init` for subsequent)

## Console URLs by Data Center

| DC | URL |
|----|-----|
| US | `console.catalyst.zoho.com/baas/index` |
| EU | `console.catalyst.zoho.eu/baas/index` |
| IN | `console.catalyst.zoho.in/baas/index` |

## Sources

- [[catalyst-general-topics]]

## Related Concepts

- [[catalyst-organizations]] — Projects belong to organizations
- [[catalyst-environments]] — Development and production per project
- [[catalyst-console]] — Web interface for project management
- [[sdk-scopes]] — Admin/User scopes apply within projects

## Evolution

_First detailed coverage from general help docs. SDK documentation assumed project existence; this source documents the creation and structure._
