---
title: Security Rules
type: concept
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-java-sdk-nosql-security-apigateway.md]
tags: [catalyst, security, serverless, access-control]
---

# Security Rules

## Definition

Security Rules is a Catalyst Serverless component that defines the invocation and access rules for Basic I/O and Advanced I/O functions. It is a JSON file that configures which HTTP methods can access a function and whether authentication is required or optional. Security Rules is the default access control — created automatically when a function is deployed. [Source: catalyst-java-sdk-nosql-security-apigateway.md]

## Key Aspects

- **Scope**: Only applies to Basic I/O and Advanced I/O functions. Does NOT apply to Cron, Event, or other function types.
- **Configuration**: HTTP methods (GET, PUT, POST, DELETE, PATCH) + authentication (required/optional)
- **Default behavior**: Auto-created with default values when function is deployed via console or CLI
- **Authentication types**: Catalyst Users Authentication, OAuth-based Authentication (2 types)
- **Same for all runtimes**: Java, Node.js, and Python functions share the same configuration parameters
- **Relationship with API Gateway**: Mutually exclusive at runtime
  - API Gateway enabled → Security Rules automatically disabled
  - API Gateway disabled → Security Rules automatically re-enabled
  - Definitions can be migrated to API Gateway via auto-create

## Sources

- [[catalyst-java-sdk-nosql-security-apigateway]] — Security Rules introduction and key concepts

## Related Concepts

- [[api-gateway]] — Enhancement to Security Rules with additional features
- [[serverless-functions]] — Functions whose access is controlled by Security Rules
- [[catalyst-authentication]] — The authentication methods referenced by Security Rules

## Evolution

_Previously listed as a knowledge gap. Now documented from the help pages. Security Rules has 3 pages (Introduction, Key Concepts, Implementation) under Serverless → FAAS._
