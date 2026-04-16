---
title: ZCUser
type: entity
created: 2026-04-05
updated: 2026-04-16
sources: [catalyst-java-sdk-authentication.md, catalyst-java-sdk-overview.md, catalyst-nodejs-sdk-cloud-scale-core.md]
tags: [catalyst, java, nodejs, sdk, class, authentication, users]
---

# ZCUser

## Overview

`ZCUser` is the central SDK class for all authentication and user management operations in the Catalyst Java SDK. It provides methods covering the entire user lifecycle — registration, retrieval, update, status management, password reset, token generation, and deletion. [Source: catalyst-java-sdk-authentication.md]

## Key Facts

- Always accessed via `ZCUser.getInstance()` (uses the [[instance-objects]] pattern)
- Returns `ZCUserDetail` objects for user data
- Works with `ZCSignUpData` for registration flows
- Part of the `com.zc.component.users` package

## Methods

| Method | Purpose | Returns |
|--------|---------|---------|
| `registerUser(ZCSignUpData)` | Register a new user | ZCSignUpData |
| `addUser(ZCSignUpData)` | Add user to existing org | ZCSignUpData |
| `getAllOrgs()` | Get all organization IDs | — |
| `getAllUser()` | Get all users in application | `List<ZCUserDetail>` |
| `getAllUser(orgId)` | Get all users in an org | — |
| `getCurrentUser()` | Get current user details | ZCUserDetail |
| `getUser(userId)` | Get user by ID | ZCUserDetail |
| `updateUser(userId, userDetail)` | Update user details | — |
| `updateUserStatus(userId, status)` | Enable or disable a user | — |
| `resetPassword(ZCSignUpData)` | Send password reset email | — |
| `generateCustomToken(tokenDetails)` | Generate third-party auth token | ZCCustomTokenResponse |
| `deleteUser(userId)` | Permanently delete a user | — |

## Node.js SDK Equivalent

In the Node.js SDK, user management is accessed via `app.userManagement()` instead of `ZCUser.getInstance()`. All methods return promises. [Source: catalyst-nodejs-sdk-cloud-scale-core.md]

| Java SDK | Node.js SDK |
|----------|-------------|
| `ZCUser.getInstance()` | `app.userManagement()` |
| `registerUser(ZCSignUpData)` | `registerUser(signupConfig, userConfig)` |
| `addUser(ZCSignUpData)` | `addUserToOrg(orgId, signupConfig, userConfig)` |
| `getAllOrgs()` | `getAllOrganizations()` |
| `getAllUser()` | `getAllUsers()` |
| `getAllUser(orgId)` | `getAllUsersOfOrg(orgId)` |
| `getCurrentUser()` | `getCurrentUser()` |
| `getUser(userId)` | `getUserDetails(userId)` |
| `updateUser(userId, detail)` | `updateUser(userId, config)` |
| `updateUserStatus(userId, status)` | `enableUser(userId)` / `disableUser(userId)` |
| `resetPassword(ZCSignUpData)` | `resetPassword(signupConfig, userConfig)` |
| `generateCustomToken(details)` | `getServerToken()` |
| `deleteUser(userId)` | `deleteUser(userId)` |

**Key difference**: Java uses `ZCSignUpData` beans; Node.js uses plain JSON objects for `signupConfig` and `userConfig`.

## Appearances

- [[catalyst-java-sdk-authentication]] — Full documentation of all methods
- [[catalyst-java-sdk-overview]] — Referenced as part of the SDK component hierarchy
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js SDK equivalent via `app.userManagement()`

## Relationships

- Works with [[zcsignupdata]] for registration
- Returns [[zcuserdetail]] objects
- Part of [[class-hierarchy]] under [[zcproject]]
- Operations relate to [[catalyst-authentication]] and [[catalyst-organizations]]

## Notes

- `getAllUser()` (no args) returns all app users; `getAllUser(orgId)` returns users in a specific org — same method name, different signatures (overloaded).
- Node.js SDK uses separate method names (`getAllUsers` vs `getAllUsersOfOrg`) instead of overloading.
- Node.js SDK splits `updateUserStatus(userId, status)` into explicit `enableUser(userId)` / `disableUser(userId)`.
