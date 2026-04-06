---
title: Catalyst Java SDK Overview
type: source
created: 2026-04-05
updated: 2026-04-05
source_file: raw/catalyst-java-sdk-overview.md
tags: [catalyst, java, sdk, zoho, serverless, baas]
---

# Catalyst Java SDK Overview

## Summary

The Catalyst Java SDK is a wrapper around the Catalyst REST APIs that enables building microservices, web, and mobile applications in Java. It supports Java 8, 11, and 17. The SDK models all Catalyst components as Java classes following the project hierarchy, with `ZCProject` as the fundamental base class. Projects can be initialized with Admin or User scopes to control access to Data Store, File Store, and ZCQL operations. The SDK can be used both within Catalyst-hosted functions (auto-initialized) and in third-party applications via OAuth authentication. [Source: catalyst-java-sdk-overview.md]

## Key Takeaways

- SDK acts as a REST API wrapper — no raw HTTP calls needed [Source: catalyst-java-sdk-overview.md]
- Supports Java 8, 11, and 17 [Source: catalyst-java-sdk-overview.md]
- Two scope levels: **Admin** (full access) and **User** (restricted) — scopes only apply to [[data-store]], [[file-store]], and [[zcql]] [Source: catalyst-java-sdk-overview.md]
- `getInstance()` pattern avoids unnecessary API calls by returning dummy objects for direct method access [Source: catalyst-java-sdk-overview.md]
- Exception handling via `ZCException`, `ZCServerException`, `ZCClientException` [Source: catalyst-java-sdk-overview.md]
- Third-party integration requires: Project ID, ZAID, OAuth credentials (Client ID, Client Secret, Refresh Token) [Source: catalyst-java-sdk-overview.md]
- SDK upgrade via console download (ZIP → `lib/` folder) or Maven [Source: catalyst-java-sdk-overview.md]

## Entities Mentioned

- [[zoho-catalyst]] — The serverless platform (BaaS) by Zoho
- [[zcproject]] — Base class of the SDK
- [[zcql]] — Catalyst query language for Data Store
- [[data-store]] — Catalyst's cloud-scale database component
- [[file-store]] — Catalyst's cloud-scale file storage component
- [[stratus]] — Catalyst's cloud-scale object storage (S3-like)

## Concepts Introduced

- [[sdk-scopes]] — Admin vs User scope for access control
- [[instance-objects]] — `getInstance()` pattern to avoid cascading API calls
- [[class-hierarchy]] — SDK mirrors the Catalyst project hierarchy
- [[third-party-sdk-integration]] — Using the SDK outside Catalyst's environment via OAuth

## Open Questions

- What are the full API operations available per SDK category (General, Serverless, Cloud Scale, etc.)?
- How do scopes interact with Catalyst Authentication roles in practice?
- What are the Maven coordinates for the SDK dependency?
- What version of the SDK is current, and what changed in recent releases?
