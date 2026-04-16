---
title: AppSail
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-overview-serverless.md]
tags: [serverless, appsail, web-service, deployment]
---

# AppSail

## Definition

AppSail is Catalyst's fully-managed platform for deploying web services. Unlike serverless functions (which are event/request-driven with timeouts), AppSail hosts persistent web applications. Using the Catalyst SDK in AppSail requires implementing the `AuthHeaderProvider` interface for initialization. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Aspects

- **SDK initialization**: Requires `AuthHeaderProvider` implementation — SDK cannot auto-initialize like in Functions
- **Two servlet API versions**:
  - `javax.servlet.http.HttpServletRequest` (Servlet API ≤4)
  - `jakarta.servlet.http.HttpServletRequest` (Servlet API ≥5)
- **Maven dependency**: `com.zoho.catalyst:java-sdk:1.15.0` from `https://maven.zohodl.com`
- **Supported runtimes**: Java, Node.js, Python
- **Persistent**: Unlike functions, AppSail runs continuously as a web service

## SDK Initialization

```java
public class AuthProviderImpl implements AuthHeaderProvider {
    HttpServletRequest request;
    public AuthProviderImpl(HttpServletRequest request) {
        this.request = request;
    }
    @Override
    public String getHeaderValue(String key) {
        return request.getHeader(key);
    }
}

// In request handler:
CatalystSDK.init(new AuthProviderImpl(request));
```

## Node.js SDK Initialization

Node.js AppSail initialization is simpler than Java — no `AuthHeaderProvider` interface needed. [Source: catalyst-nodejs-sdk-overview-serverless.md]

```js
const catalyst = require('zcatalyst-sdk-node');
const express = require('express');
const app = express();

app.get((req, res) => {
  let catalystApp = catalyst.initialize(req);
  // Full access to all Catalyst components
});

app.listen(process.env('X_ZOHO_CATALYST_LISTEN_PORT') || 9000);
```

**Key difference**: Java requires implementing `AuthHeaderProvider` interface and using `CatalystSDK.init()`. Node.js uses the same `catalyst.initialize(req)` as Advanced I/O Functions. Not available for OCI images through custom runtime.

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]
- [[catalyst-nodejs-sdk-overview-serverless]] — Node.js AppSail initialization

## Related Concepts

- [[serverless-functions]] — Functions are event-driven; AppSail is persistent web service
- [[connections-sdk]] — Connections is accessible from AppSail context
- [[third-party-sdk-integration]] — AppSail init pattern is similar to third-party init

## Evolution

- AppSail fills the gap between serverless functions (30s/15min timeout) and full infrastructure management, providing always-on web service hosting within the Catalyst ecosystem.
