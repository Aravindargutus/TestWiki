---
title: Connectors (External)
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [connectors, oauth, zoho-services, external]
---

# Connectors (External)

## Definition

Connectors provide OAuth-based connections between Catalyst and external Zoho services (CRM, WorkDrive, etc.). Unlike [[connections-sdk]] (which is internal to Functions/AppSail), Connectors are configured manually with client credentials and manage token refresh via [[cache]]. [Source: catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]

## Key Aspects

- **Zoho services only** — cannot connect to third-party services
- **Manual setup**: Requires client_id, client_secret, auth_url, refresh_url, refresh_token from Zoho API Console
- **Automatic token refresh**: Stores access tokens in Catalyst Cache; refreshes automatically on expiry
- **Unique names required**: Each connector name must be unique; different users need different connector names
- **Package**: `com.zc.auth.connectors` (note: different from `com.zc.component.connections`)

## SDK Entry Point

```java
ZCConnection conn = ZCConnection.getInstance(connectorJson);
ZCConnector crmConnector = conn.getConnector("CRMConnector");
String accessToken = crmConnector.getAccessToken();
```

## Node.js SDK Access Pattern

```js
const connector = app.connection({
  ConnectorName: {
    client_id: '...', client_secret: '...', auth_url: '...',
    refresh_url: '...', refresh_token: '...', refresh_in: '...'
  }
}).getConnector('ConnectorName');

const token = await connector.getAccessToken();
```

**Important Node.js-specific notes**: Different users in the same app need unique connector names — tokens are stored in Cache segments, and the same connector name overwrites existing tokens. Must register app in Zoho API Console first. [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

**Key difference**: Java uses `ZCConnection.getInstance(connectorJson)` + `getConnector()`. Node.js chains `app.connection({config}).getConnector('name')`.

## Sources

- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]]
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js Connectors (1 page)

## Related Concepts

- [[connections-sdk]] — Internal OAuth management (simpler, Functions/AppSail only)
- [[cache]] — Connectors store access tokens in Cache segments
- [[third-party-sdk-integration]] — Another pattern for external integration
