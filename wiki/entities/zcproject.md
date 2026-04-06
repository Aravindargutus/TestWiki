---
title: ZCProject
type: entity
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-overview.md]
tags: [catalyst, java, sdk, class]
---

# ZCProject

## Overview

`ZCProject` is the fundamental base class of the Catalyst Java SDK. It serves as the entry point for all SDK operations. [Source: catalyst-java-sdk-overview.md]

## Key Facts

- Base class of the entire SDK package
- Has methods to initialize project configurations and associate components
- Initialization: `ZCProject.initProject()` (auto-initialized in Catalyst functions)
- Supports scoped initialization: `ZCProject.initProject("admin", ZCUserScope.ADMIN)`
- For third-party apps: `ZCProject.initProject(config, "")` using `ZCProjectConfig`

## Key Methods

| Method | Purpose |
|--------|---------|
| `initProject()` | Initialize with defaults (Admin scope) |
| `initProject(name, scope)` | Initialize with specific scope |
| `initProject(config, "")` | Initialize for third-party app with full config |

## Appearances

- [[catalyst-java-sdk-overview]] — Documented as the SDK entry point

## Relationships

- Parent of all component classes in the [[class-hierarchy]]
- Works with [[sdk-scopes]] (Admin/User)
- Configured via `ZCProjectConfig` for [[third-party-sdk-integration]]

## Notes

- In Catalyst-hosted Java functions, initialization is automatic — calling `initProject()` explicitly is optional.
