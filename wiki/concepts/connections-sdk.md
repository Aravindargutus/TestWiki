---
title: Connections SDK
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, connections, oauth, zoho-services]
---

# Connections SDK

## Definition

Connections is Catalyst's mechanism for managing OAuth authentication credentials to Zoho services and third-party APIs from within Functions and AppSail. The SDK automatically handles token refresh and provides headers/parameters needed for authenticated calls to connected services. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Aspects

- **Internal-only**: Only accessible within Catalyst Functions and [[appsail]] — cannot be used from third-party applications
- **2 SDK operations**: Get instance, get connection credentials
- **Automatic token management**: OAuth tokens are refreshed automatically
- **Returns headers + parameters**: `ZCConnectionResponse` provides both for authenticated API calls
- **Named connections**: Referenced by connection name (e.g., `"payrollcon"`)

## SDK Entry Point

```java
ZCConnections connections = ZCConnections.getInstance();
ZCConnectionResponse response = connections.getConnectionCredentials("payrollcon");
Map headers = response.getHeaders();
Map params = response.getParameters();
```

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]

## Related Concepts

- [[third-party-sdk-integration]] — External integration pattern (vs Connections for internal)
- [[catalyst-authentication]] — User auth vs service-to-service auth
- [[appsail]] — Connections available in AppSail context

## Evolution

- Connections simplifies what would otherwise require manual OAuth token management. The concept of "Default Services" is mentioned but the specific available services are not documented in the SDK reference.
