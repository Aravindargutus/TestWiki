---
title: Third-Party Authentication
type: concept
created: 2026-04-05
updated: 2026-04-05
sources: [catalyst-java-sdk-authentication.md]
tags: [catalyst, authentication, third-party, tokens, security]
---

# Third-Party Authentication

## Definition

A Catalyst Authentication mode where an external authentication service handles user authorization and validation, then passes data to Catalyst. The server-side logic generates a custom token via the Java SDK, which is passed to the Web SDK client. [Source: catalyst-java-sdk-authentication.md]

## Key Aspects

- User is redirected to third-party service for authentication
- After auth, credentials are passed to a Catalyst function
- Function generates a custom server token via `ZCUser.getInstance().generateCustomToken()`
- Token is then passed to the Web SDK using the Web SDK's custom token API
- **Token must be regenerated every login session** — not persistent
- Requires **Public Signup** to be enabled
- Security depends on the third-party service quality

## Key Classes

| Class | Purpose |
|-------|---------|
| `ZCCustomTokenDetails` | Container for token generation request |
| `ZCCustomTokenUserDetails` | User details (email, name, role) for the token |
| `ZCCustomTokenResponse` | Response containing the generated token |

## Flow

1. User authenticates with third-party service
2. Third-party redirects to your Catalyst function with user credentials
3. Function calls `generateCustomToken()` with user details
4. Generated token passed to Web SDK client
5. Web SDK uses token to establish Catalyst session

## Sources

- [[catalyst-java-sdk-authentication]] — Server-side token generation code

## Related Concepts

- [[catalyst-authentication]] — Third-party auth is one of three auth types
- [[custom-user-validation]] — Different approach (validates during native signup, not third-party)
- [[third-party-sdk-integration]] — Related but different: using the SDK outside Catalyst vs using third-party auth

## Evolution

_First documented from Java SDK auth source. Complements the third-party SDK integration concept from the overview source._
