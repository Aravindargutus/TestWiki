---
title: ZCSignUpData
type: entity
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-authentication.md]
tags: [catalyst, java, sdk, class, authentication]
---

# ZCSignUpData

## Overview

`ZCSignUpData` is a data holder class in the Catalyst Java SDK used to package all information required for user registration and password reset operations. [Source: catalyst-java-sdk-authentication.md]

## Key Facts

- Package: `com.zc.component.users`
- Created via `ZCSignUpData.getInstance()`
- Used by `registerUser()`, `addUser()`, and `resetPassword()` methods of [[zcuser]]
- Contains a `userDetail` sub-object for setting user properties
- Has a `mailTemplateInstance()` factory method to create email template config

## Properties & Methods

| Method | Purpose |
|--------|---------|
| `getInstance()` | Static factory to create instance |
| `mailTemplateInstance()` | Create a `ZCMailTemplateDetails` for email config |
| `setTemplateDetails(mailData)` | Set the email template |
| `setPlatformType(PlatformType.WEB)` | Set platform (WEB) |
| `userDetail.setEmailId(email)` | Set user email (mandatory) |
| `userDetail.setFirstName(name)` | Set first name (mandatory for some ops) |
| `userDetail.setLastName(name)` | Set last name |
| `userDetail.setRoleId(roleId)` | Set role (long ID from console) |
| `userDetail.setOrgId(orgId)` | Set org ID (for `addUser` to existing org) |

## Email Template Placeholders

- `%APP_NAME%` — Application name
- `%LINK%` — Signup/reset link

## Appearances

- [[catalyst-java-sdk-authentication]] — Used in registration, add-to-org, and reset-password flows

## Relationships

- Used by [[zcuser]] methods: `registerUser()`, `addUser()`, `resetPassword()`
- Contains `ZCMailTemplateDetails` for email configuration
- Uses `PlatformType` enum (e.g., `PlatformType.WEB`)
