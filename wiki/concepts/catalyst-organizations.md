---
title: Catalyst Organizations (ZAAID / Org ID)
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-authentication.md]
tags: [catalyst, organizations, authentication, users]
---

# Catalyst Organizations

## Definition

In Catalyst, every user belongs to an **Organization** identified by an **Org ID** (also called **ZAAID** — Zoho Accounts Association ID). This ID is auto-generated when a user signs up, is added via the API, or is added through the console. Organizations group users together within a Catalyst application. [Source: catalyst-java-sdk-authentication.md]

## Key Aspects

- Org ID is a unique numeric identifier (e.g., `"35712181"`, `10062701096`)
- Created automatically on first user signup
- New users can be added to existing orgs using `ZCUser.getInstance().addUser()` with `setOrgId()`
- All org IDs retrievable via `ZCUser.getInstance().getAllOrgs()`
- All users in an org retrievable via `ZCUser.getInstance().getAllUser(orgId)`
- Users can be updated to different orgs via `updateUser()` with `setZAAID()`

## Sources

- [[catalyst-java-sdk-authentication]] — Documents org operations (get all orgs, add user to org, get users in org)

## Related Concepts

- [[catalyst-authentication]] — Orgs are part of the authentication system
- [[sdk-scopes]] — Scopes may interact with org-level access

## Evolution

_First documented from authentication SDK source. The concept also appeared briefly in the SDK overview's third-party integration (ZAID field in ZCProjectConfig)._
