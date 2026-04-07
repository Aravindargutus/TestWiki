---
title: API Gateway
type: concept
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-java-sdk-nosql-security-apigateway.md]
tags: [catalyst, api-gateway, security, routing, throttling, cloud-scale]
---

# API Gateway

## Definition

Catalyst Cloud Scale API Gateway is an advanced, optional, paid API management component that acts as a reverse proxy between clients and Catalyst services. It provides custom routing, authentication (3 methods including API Key), and throttling for Basic I/O Functions, Advanced I/O Functions, and Web Client targets. Enhancement to [[security-rules]]. [Source: catalyst-java-sdk-nosql-security-apigateway.md]

## Key Aspects

- **Targets**: Basic I/O Functions, Advanced I/O Functions, Web Client
- **Routing**:
  - HTTP methods: GET, PUT, POST, DELETE, PATCH + ANY (aggregates all)
  - Custom request URL and target URL (decoupled)
  - Regex support in URLs for dynamic values
  - Web client APIs only support GET
- **Authentication** (3 methods, any or all):
  1. **API Key**: Auto-generated, different per environment (dev vs prod). Pass via `ZCFKEY` header or query param
  2. **Catalyst Users Authentication**: For registered app users
  3. **OAuth-Based**: Via access token in Authorization header
  - Not available for web client APIs
- **Throttling** (2 types):
  1. **General**: Max hits per time unit for all users. Sliding window rate limiting algorithm.
  2. **IP-Based**: Max hits per time unit from specific IP. Returns HTTP 429 when exceeded.
- **API types**: Auto-created (migrates from Security Rules), Custom (fully configurable), Default (Login Redirect for web client, auto-created, not deletable)
- **Limits**: 1000 APIs/project in dev, unlimited in prod

## Security Rules vs API Gateway

| Feature | Security Rules | API Gateway |
|---------|---------------|-------------|
| Request/Target URLs | Same | Custom, decoupled |
| Auth methods | 2 (Catalyst Users, OAuth) | 3 (adds API Key) |
| Throttling | None | General + IP-based |
| Web client support | No | Yes |
| HTTP method aggregation | No | Yes (ANY) |
| Regex in URLs | Yes | Yes |
| Cost | Free (default) | Paid (optional) |

## Architecture Flow

1. Read request method and URL
2. Search for matching API configuration
3. Initiate API or deny request (no match)
4. Check throttling limits (if configured)
5. Check authentication (if configured)
6. Redirect to target URL

## Sources

- [[catalyst-java-sdk-nosql-security-apigateway]] — All 5 API Gateway help pages (Introduction, Key Concepts, Architecture, Benefits, Implementation)

## Related Concepts

- [[security-rules]] — Default access control, replaced when API Gateway is enabled
- [[serverless-functions]] — Functions whose access API Gateway manages
- [[function-urls]] — URL patterns that API Gateway overrides with custom routing

## Evolution

_Previously listed as a knowledge gap. Now fully documented from the 5 help pages._
