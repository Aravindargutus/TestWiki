---
title: Catalyst Authentication
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-authentication.md, catalyst-java-sdk-overview.md]
tags: [catalyst, authentication, users, cloud-scale, security]
---

# Catalyst Authentication

## Definition

The Authentication component of Catalyst Cloud Scale manages the end-to-end user lifecycle for Catalyst applications: user registration, sign-in, role assignment, organization management, password management, and user CRUD operations. Accessible via the Java SDK through the [[zcuser]] class. [Source: catalyst-java-sdk-authentication.md]

## Key Aspects

- **User Registration**: New users get ZUID + userID + org assignment. Requires EmailId + FirstName. Max 25 users in dev, unlimited in prod. [Source: catalyst-java-sdk-authentication.md]
- **Organizations**: Each user belongs to an org (ZAAID). Users can be added to existing orgs. [Source: catalyst-java-sdk-authentication.md]
- **Roles**: Users are assigned roles (by RoleId) which control access. Roles configured in Authentication console. [Source: catalyst-java-sdk-authentication.md]
- **Email Notifications**: Registration and password reset send emails via Catalyst Mail Component. Sender email must be verified. Templates use `%APP_NAME%` and `%LINK%` placeholders. [Source: catalyst-java-sdk-authentication.md]
- **Password Reset**: Generates a reset link sent to user's email. [Source: catalyst-java-sdk-authentication.md]
- **User Status**: Users can be enabled/disabled without deletion. [Source: catalyst-java-sdk-authentication.md]
- **Authentication Types**: Supports native (hosted), third-party, and custom validation. [Source: catalyst-java-sdk-authentication.md]

## Full Operation Set (Java SDK)

1. Add New User (`registerUser`)
2. Get All Org IDs (`getAllOrgs`)
3. Add User to Existing Org (`addUser`)
4. Get All Users in Org (`getAllUser(orgId)`)
5. Reset Password (`resetPassword`)
6. Generate Custom Server Token (`generateCustomToken`)
7. Custom User Validation (via Basic I/O function)
8. Get User Details (`getCurrentUser`, `getAllUser`, `getUser`)
9. Update User Details (`updateUser`)
10. Enable/Disable User (`updateUserStatus`)
11. Delete User (`deleteUser`)

## Sources

- [[catalyst-java-sdk-authentication]] — Complete SDK documentation for all 11 operations
- [[catalyst-java-sdk-overview]] — Mentions Authentication as part of Cloud Scale

## Related Concepts

- [[sdk-scopes]] — Admin/User scopes interact with user roles
- [[catalyst-organizations]] — Org management is part of authentication
- [[custom-user-validation]] — Custom logic for signup authorization
- [[third-party-authentication]] — External auth service integration

## Evolution

_First comprehensive documentation from the Java SDK auth pages. Previously only mentioned as a Cloud Scale category in the SDK overview._
