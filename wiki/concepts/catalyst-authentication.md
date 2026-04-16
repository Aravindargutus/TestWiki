---
title: Catalyst Authentication
type: concept
created: 2026-04-05
updated: 2026-04-16
sources: [catalyst-java-sdk-authentication.md, catalyst-java-sdk-overview.md, catalyst-nodejs-sdk-cloud-scale-core.md]
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

## Full Operation Set (Node.js SDK)

All via `app.userManagement()`, returning promises. [Source: catalyst-nodejs-sdk-cloud-scale-core.md]

1. Add New User — `registerUser(signupConfig, userConfig)`
2. Get All Org IDs — `getAllOrganizations()`
3. Add User to Existing Org — `addUserToOrg(orgId, signupConfig, userConfig)`
4. Get All Users in Org — `getAllUsersOfOrg(orgId)`
5. Reset Password — `resetPassword(signupConfig, userConfig)`
6. Generate Server Token — `getServerToken()`
7. Custom Validation — `validateUser(userConfig)`
8. Get User Details — `getCurrentUser()`, `getAllUsers()`, `getUserDetails(userId)`
9. Update User — `updateUser(userId, config)`
10. Enable/Disable — `enableUser(userId)` / `disableUser(userId)`
11. Delete User — `deleteUser(userId)`

Node.js response fields: `zuid, zaaid, org_id, status, is_confirmed, email_id, last_name, created_time, role_details { role_name, role_id }, user_type, user_id, locale, time_zone, project_profiles`.

## Sources

- [[catalyst-java-sdk-authentication]] — Complete SDK documentation for all 11 operations
- [[catalyst-java-sdk-overview]] — Mentions Authentication as part of Cloud Scale
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js SDK authentication (11 operations)

## Related Concepts

- [[sdk-scopes]] — Admin/User scopes interact with user roles
- [[catalyst-organizations]] — Org management is part of authentication
- [[custom-user-validation]] — Custom logic for signup authorization
- [[third-party-authentication]] — External auth service integration

## Evolution

_First documented from Java SDK auth pages. Node.js SDK v2 provides identical coverage with 11 matching operations, using promises and plain JSON configs instead of Java beans. Key naming differences: `validateUser` (Node) vs custom validation function (Java), `enableUser`/`disableUser` (Node) vs `updateUserStatus` (Java)._
