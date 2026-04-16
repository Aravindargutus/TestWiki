---
title: Catalyst Organizations (ZAAID / Org ID)
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-authentication.md, catalyst-general-topics.md]
tags: [catalyst, organizations, authentication, users, management]
---

# Catalyst Organizations

## Definition

In Catalyst, every user belongs to an **Organization** identified by an **Org ID** (also called **ZAAID** — Zoho Accounts Association ID). This ID is auto-generated when a user signs up, is added via the API, or is added through the console. Organizations group users together within a Catalyst application. [Source: catalyst-java-sdk-authentication.md]

## Key Aspects

### SDK Operations (Authentication)
- Org ID is a unique numeric identifier (e.g., `"35712181"`, `10062701096`)
- Created automatically on first user signup
- New users can be added to existing orgs using `ZCUser.getInstance().addUser()` with `setOrgId()`
- All org IDs retrievable via `ZCUser.getInstance().getAllOrgs()`
- All users in an org retrievable via `ZCUser.getInstance().getAllUser(orgId)`
- Users can be updated to different orgs via `updateUser()` with `setZAAID()`

### Platform-Level Organization Management [Source: catalyst-general-topics.md]
- Create and manage **multiple organizations** from same Catalyst account
- Organization creator is the **owner** by default
- Console URL format: `https://console.catalyst.zoho.com/baas/{{OrgID}}/index`
- **Collaborator types**: Owner (creator), Admin, Project Member
- **Default organization**: First-ever org created at signup. Login always goes to default org.
- API calls execute against default org unless Org ID explicitly specified
- Cannot delete the default org (must change default first)
- Deleting an org permanently removes **all** its projects

### Operations (Console)
- **Create**: From multi-org portal → "Create Organization"
- **Switch**: From profile icon drop-down → select org
- **Set Default**: Ellipsis menu → "Set as Default"
- **Delete**: Ellipsis menu → "Remove Org" (owner only, permanent)

## Sources

- [[catalyst-java-sdk-authentication]] — Documents org operations (get all orgs, add user to org, get users in org)
- [[catalyst-general-topics]] — Platform-level multi-org management, roles, default org behavior

## Related Concepts

- [[catalyst-authentication]] — Orgs are part of the authentication system
- [[sdk-scopes]] — Scopes may interact with org-level access
- [[catalyst-projects]] — Projects belong to organizations
- [[catalyst-console]] — Multi-org portal accessible from console header

## Evolution

_First documented from authentication SDK source (SDK operations). Enriched with general platform documentation covering multi-org management, default org concept, and console operations._
