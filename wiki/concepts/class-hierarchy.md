---
title: Class Hierarchy
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-overview.md]
tags: [catalyst, sdk, architecture, java]
---

# Class Hierarchy

## Definition

The Catalyst Java SDK models all platform components as Java classes organized in a hierarchy that mirrors the project structure in the Catalyst console. [[zcproject]] sits at the root, with component classes branching beneath it. [Source: catalyst-java-sdk-overview.md]

## Key Aspects

- **ZCProject** is the root/base class
- Each class has methods to fetch its own properties and its immediate child entities (via API calls)
- Example chain: `ZCProject` → `ZCDataStore` → `ZCTable` → row operations
- Traversing the full hierarchy triggers API calls at each level — use [[instance-objects]] to shortcut this
- Class members and methods define component behavior

## Sources

- [[catalyst-java-sdk-overview]] — Describes the hierarchy and its relationship to the project structure

## Related Concepts

- [[instance-objects]] — The pattern to avoid full hierarchy traversal
- [[sdk-scopes]] — Applied at the ZCProject root level

## Evolution

_First documented in this source. No evolution yet._
