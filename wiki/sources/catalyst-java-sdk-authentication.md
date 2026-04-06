---
title: Catalyst Java SDK — Authentication
type: source
created: 2026-04-05
updated: 2026-04-05
source_file: raw/catalyst-java-sdk-authentication.md
tags: [catalyst, java, sdk, authentication, users, cloud-scale]
---

# Catalyst Java SDK — Authentication

## Summary

The Authentication component of the Catalyst Java SDK provides a complete set of user management operations for Catalyst applications. It covers the full user lifecycle: registration (with email invitations), organization management, password reset, third-party auth token generation, custom signup validation, user detail retrieval/update, enable/disable, and deletion. The central class is `ZCUser`, with `ZCSignUpData` handling registration flows and `ZCMailTemplateDetails` handling email notifications. [Source: catalyst-java-sdk-authentication.md]

## Key Takeaways

- **11 distinct operations** covering the full user lifecycle [Source: catalyst-java-sdk-authentication.md]
- All operations use the `getInstance()` pattern via `ZCUser.getInstance()` [Source: catalyst-java-sdk-authentication.md]
- **User registration** requires EmailId + FirstName (mandatory); creates ZUID, userID, and org assignment [Source: catalyst-java-sdk-authentication.md]
- **Dev environment limit**: Max 25 users; unlimited in production [Source: catalyst-java-sdk-authentication.md]
- **Email templates** support placeholders: `%APP_NAME%`, `%LINK%`; sender email must be verified in Catalyst Mail Component [Source: catalyst-java-sdk-authentication.md]
- **Org ID (ZAAID)** is auto-generated per user signup; users can be added to existing orgs [Source: catalyst-java-sdk-authentication.md]
- **Third-party auth** uses custom server tokens — token must be regenerated every login [Source: catalyst-java-sdk-authentication.md]
- **Custom user validation** via Basic I/O function allows custom signup logic (whitelist/blacklist emails, assign roles dynamically) [Source: catalyst-java-sdk-authentication.md]
- **User status** can be toggled (enable/disable) without deletion [Source: catalyst-java-sdk-authentication.md]

## SDK Method Reference

| Operation | Method | Key Parameters |
|-----------|--------|----------------|
| Register user | `ZCUser.getInstance().registerUser(signUpData)` | EmailId, FirstName, RoleId, PlatformType, mail template |
| Get all org IDs | `ZCUser.getInstance().getAllOrgs()` | — |
| Add user to existing org | `ZCUser.getInstance().addUser(signUpData)` | EmailId, FirstName, OrgId |
| Get users in org | `ZCUser.getInstance().getAllUser(orgId)` | Org ID (long) |
| Reset password | `ZCUser.getInstance().resetPassword(signUpData)` | EmailId, mail template |
| Generate custom token | `ZCUser.getInstance().generateCustomToken(tokenDetails)` | EmailId, FirstName, LastName, RoleName |
| Custom validation | `ZCSignupUserService.getSignupValidationRequest(basicIO)` | BasicIO context |
| Get current user | `ZCUser.getInstance().getCurrentUser()` | — |
| Get all users | `ZCUser.getInstance().getAllUser()` | — |
| Get user by ID | `ZCUser.getInstance().getUser(userId)` | User ID (long) |
| Update user | `ZCUser.getInstance().updateUser(userId, userDetail)` | User ID, ZCUserDetail |
| Enable/disable user | `ZCUser.getInstance().updateUserStatus(userId, status)` | User ID, USER_STATUS enum |
| Delete user | `ZCUser.getInstance().deleteUser(userId)` | User ID (long) |

## Entities Mentioned

- [[zcuser]] — Central class for all authentication operations
- [[zcsignupdata]] — Handles user registration data
- [[zoho-catalyst]] — The platform providing these authentication features

## Concepts Introduced

- [[catalyst-authentication]] — The authentication component and lifecycle
- [[catalyst-organizations]] — Org ID (ZAAID) and multi-org user management
- [[custom-user-validation]] — Serverless function-based signup validation
- [[third-party-authentication]] — External auth with custom server tokens

## Open Questions

- What user properties are returned by `getCurrentUser()` / `getUser()` / `getAllUser()`?
- How do roles interact with scopes — is RoleId linked to Admin/User scope?
- What happens to a user's data in Data Store/File Store when they are deleted?
- Are there rate limits on user registration or token generation?
