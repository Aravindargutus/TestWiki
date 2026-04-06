---
title: Third-Party SDK Integration
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-overview.md]
tags: [catalyst, sdk, oauth, integration, external]
---

# Third-Party SDK Integration

## Definition

The ability to use the Catalyst Java SDK in applications deployed outside the Catalyst environment — e.g., on Vercel, AWS EC2, or any external host. Requires OAuth2 authentication via Zoho's self-client portal. [Source: catalyst-java-sdk-overview.md]

## Key Aspects

- Enables Catalyst component access from any Java application, not just Catalyst-hosted functions
- Requires four prerequisites: **Project ID**, **ZAID**, **Environment** (dev/prod), **OAuth credentials**
- OAuth flow: Register a self-client at api-console.zoho.com → get grant token → exchange for refresh token
- Configuration uses `ZCProjectConfig.newBuilder()` with project ID, ZAID, auth, domain, and environment
- ZAID retrieval requires setting up Catalyst Authentication (even if not used for app login)

## Setup Flow

1. Create project in Catalyst Console
2. Get Project ID from Settings → General
3. Set up Authentication to retrieve ZAID (via social login providers — Google, Microsoft, LinkedIn, Facebook; Zoho login not supported for ZAID)
4. Register self-client at api-console.zoho.com
5. Configure scope, get grant token, exchange for access + refresh tokens
6. Use the SpringBoot integration code pattern

## Sources

- [[catalyst-java-sdk-overview]] — Full integration guide with code snippet

## Related Concepts

- [[sdk-scopes]] — Scope is set on the auth object via `auth.setScope()`
- [[class-hierarchy]] — Once initialized, same class hierarchy applies

## Evolution

_First documented in this source. No evolution yet._
