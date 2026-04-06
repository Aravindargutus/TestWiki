---
title: SDK Scopes (Admin vs User)
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-overview.md]
tags: [catalyst, sdk, access-control, scopes]
---

# SDK Scopes

## Definition

SDK Scopes in the Catalyst Java SDK control the level of access a project initialization has over Catalyst components. Two levels exist: **Admin** (unrestricted) and **User** (restricted). [Source: catalyst-java-sdk-overview.md]

## Key Aspects

- **Admin scope**: Full CRUD access to all components (Read, Write, Delete, etc.)
- **User scope**: Restricted access — e.g., read-only on Data Store
- Scopes only apply to: [[data-store]], [[file-store]], and [[zcql]]
- Default is Admin when no scope is specified
- User scope permissions are tied to the role assigned during Catalyst Authentication signup
- Permissions configurable in Scopes & Permissions section of Data Store and File Store

## Usage

```java
// Admin scope
ZCProject adminProject = ZCProject.initProject("admin", ZCUserScope.ADMIN);

// User scope
ZCProject userProject = ZCProject.initProject("user", ZCUserScope.USER);
```

## Sources

- [[catalyst-java-sdk-overview]] — Defines the two scope levels and their application

## Related Concepts

- [[class-hierarchy]] — Scopes are set at the ZCProject level
- [[third-party-sdk-integration]] — External apps use `auth.setScope(ZCUserScope.ADMIN)`

## Evolution

_First documented in this source. No evolution yet._
