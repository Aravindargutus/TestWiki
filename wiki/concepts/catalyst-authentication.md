---
title: Catalyst Authentication
type: concept
created: 2026-04-05
updated: 2026-04-16
sources: [catalyst-java-sdk-authentication.md, catalyst-java-sdk-overview.md, catalyst-nodejs-sdk-cloud-scale-core.md, docs.catalyst.zoho.com/en/cloud-scale/help/authentication]
tags: [catalyst, authentication, users, cloud-scale, security, hosted-login, embedded, social-logins]
---

# Catalyst Authentication

## Definition

The Authentication component of Catalyst Cloud Scale manages the end-to-end user lifecycle for Catalyst applications: user registration, sign-in, role assignment, organization management, password management, and user CRUD operations. Accessible via the Java SDK through the [[zcuser]] class. [Source: catalyst-java-sdk-authentication.md]

## Platform Overview

### Authentication Types

Catalyst supports **three authentication type families**; **one instance of each family** is allowed per application. At least one must be set up before users can be added.

1. **Native Catalyst Authentication**
   - **Hosted Authentication Type** — Catalyst-hosted login/signup/password-reset UI at `/__catalyst/auth/login`; branded and styled from the console; zero custom UI code required
   - **Embedded Authentication Type** — Login UI embedded directly into the app's own frontend
2. **[[third-party-authentication]]** — External auth provider with Catalyst-issued custom server tokens

### Console Feature Surface

Five console pages covering the full Authentication component:

- **Users** — User Management: add/remove end-users, enable/disable accounts, view statistics, invite new users
- **Email Templates** — Branded templates for signup invites, password reset, verification; support `%APP_NAME%` + `%LINK%` placeholders
- **Sign-in Method** — Configure authentication types, hosted-login page styling, [[custom-user-validation]], Public Signup toggle, **Social Logins**
- **Roles** — Create/manage roles that define permission tiers; assign users by `RoleId`
- **Authorized Domains** — Whitelist external domains; configure **CORS** + **iFrame** embedding permissions; Cross Domain Access

### Social Logins (Sign-in Method)

Built-in providers: **Zoho**, **Google**, **LinkedIn**, **Microsoft 365**, **Facebook**. Zoho-branded login requires **no OAuth credentials** (since Catalyst is a Zoho product); external providers require OAuth `Client ID` + `Client Secret`.

### Public Signup

Toggle that controls whether **unauthenticated users can self-register**:
- **Enabled** → end-users sign up directly from the hosted/embedded UI
- **Disabled** → users can only enter via admin invite / `registerUser` SDK call

### Authorization Scopes

- **Dev environment**: capped at **25 users**
- **Production**: unlimited users (billing-scaled)

### User Data Model (response fields)

`zuid` (user ID) · `zaaid` (org ID) · `org_id` · `status` · `is_confirmed` · `email_id` · `first_name` / `last_name` · `created_time` · `role_details { role_name, role_id }` · `user_type` · `user_id` · `locale` · `time_zone` · `project_profiles`

### Integrations

- **[[catalyst-mail]]** — delivers signup, invite, and password-reset emails (sender must be verified)
- **[[custom-user-validation]]** — Basic I/O function hook that gates signup/login per custom logic
- **[[web-client-hosting]]** / **[[catalyst-slate]]** — frontend hosts that consume `/__catalyst/auth/login` redirects
- **[[security-rules]]** / **[[api-gateway]]** — consume authenticated user identity for authorization decisions

### Cross-SDK Coverage

Available via **Java SDK**, **Node.js SDK v2**, **Python SDK**, **Web SDK v4**, **REST API**, plus Android / iOS / Flutter client SDKs.

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
- [docs.catalyst.zoho.com/en/cloud-scale/help/authentication/introduction](https://docs.catalyst.zoho.com/en/cloud-scale/help/authentication/introduction/) — Console/Platform overview

## Related Concepts

- [[sdk-scopes]] — Admin/User scopes interact with user roles
- [[catalyst-organizations]] — Org management is part of authentication
- [[custom-user-validation]] — Custom logic for signup authorization
- [[third-party-authentication]] — External auth service integration
- [[catalyst-mail]] — Delivers auth emails (invites, verification, password reset)
- [[security-rules]] — Consumes authenticated identity for authorization
- [[api-gateway]] — API-key + identity-based access control
- [[catalyst-environments]] — 25-user dev cap; unlimited in production

## Evolution

_First documented from Java SDK auth pages. Node.js SDK v2 provides identical coverage with 11 matching operations, using promises and plain JSON configs instead of Java beans. Key naming differences: `validateUser` (Node) vs custom validation function (Java), `enableUser`/`disableUser` (Node) vs `updateUserStatus` (Java)._
