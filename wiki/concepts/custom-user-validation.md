---
title: Custom User Validation
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-authentication.md]
tags: [catalyst, authentication, validation, serverless, functions]
---

# Custom User Validation

## Definition

A Catalyst feature that lets you authorize and validate end-users during signup using a custom Basic I/O function. You write your own logic to process user credentials and grant or deny access to your application. [Source: catalyst-java-sdk-authentication.md]

## Key Aspects

- Implemented as a [[catalyst-functions]] Basic I/O function implementing `ZCFunction`
- Uses `ZCSignupUserService.getSignupValidationRequest(basicIO)` to get the signup request
- Returns `ZCSignupUserValidationResponse` with `SUCCESS` or `FAILURE` status
- On success, can customize: FirstName, LastName, RoleIdentifier, OrgId
- On failure, user is rejected from signing up
- The validation function receives a JSON payload with: email_id, first_name, last_name, org_id, role_details, auth_type

## Key Classes

| Class | Package | Purpose |
|-------|---------|---------|
| `ZCSignupUserValidationRequest` | `com.zc.component.auth` | Incoming signup request |
| `ZCSignupUserValidationResponse` | `com.zc.component.auth` | Validation result to send back |
| `ZCSignupResponseUserDetails` | `com.zc.component.auth` | User details to set on success |
| `ZCSignupUserService` | `com.zc.component.users` | Service to parse BasicIO into request |
| `ZCSignupValidationStatus` | `com.zc.api.APIConstants` | SUCCESS / FAILURE enum |

## Example Use Case

Block users with disallowed email domains (`@notallowedmail`), while allowing others and assigning them a role and org.

## Sources

- [[catalyst-java-sdk-authentication]] — Full code example with test JSON payload

## Related Concepts

- [[catalyst-authentication]] — Validation is part of the authentication pipeline
- [[third-party-authentication]] — Both involve custom auth logic, but validation is for native signup

## Evolution

_First documented from Java SDK auth source._
