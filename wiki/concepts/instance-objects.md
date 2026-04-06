---
title: Instance Objects Pattern
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-overview.md]
tags: [catalyst, sdk, design-pattern, java]
---

# Instance Objects

## Definition

A design pattern in the Catalyst Java SDK where every component class provides a `getInstance()` method that returns a lightweight dummy object. This avoids traversing the full [[class-hierarchy]] (which would trigger API calls at each level) when you only need to access methods of a deeply nested component. [Source: catalyst-java-sdk-overview.md]

## Key Aspects

- `getInstance()` returns a dummy object with no properties filled in (no API call made)
- Used solely to access non-static methods of the class
- Avoids cascading API calls when navigating from top-level to lower-level components
- After getting the instance, call other methods on it to retrieve actual data

## Sources

- [[catalyst-java-sdk-overview]] — Explains the pattern and its rationale

## Related Concepts

- [[class-hierarchy]] — The hierarchy that this pattern shortcuts
- [[sdk-scopes]] — Scope is set at project level, then instance objects operate within it

## Evolution

_First documented in this source. No evolution yet._
