---
title: AppSail
type: concept
created: 2026-04-06
updated: 2026-04-17
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-overview-serverless.md]
tags: [serverless, appsail, web-service, deployment, oci, container]
---

# AppSail

## Definition

AppSail is Catalyst's fully-managed platform for deploying web services. Unlike serverless functions (which are event/request-driven with timeouts), AppSail hosts persistent web applications. Using the Catalyst SDK in AppSail requires implementing the `AuthHeaderProvider` interface for initialization. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Platform Overview

> Source: [AppSail Introduction](https://docs.catalyst.zoho.com/en/appsail/help/introduction/) and related help pages. Distinct from the SDK-specific material below.

### Two Runtime Modes

| Mode | What it is | Languages / frameworks |
|---|---|---|
| **Catalyst-Managed Runtime** | Deploy raw build files; Catalyst provisions the runtime | Native support for **Java, Node.js, Python** (any framework/library that is not platform-dependent) |
| **Custom Runtime** | Deploy an OCI container image | **Any** language/framework — PHP, Go, Ruby, or unsupported Java/Node/Python versions |

**Container constraints**: Only **OCI-compliant images built for Linux AMD64 (x86-64)** are accepted.

**Container sources** (Custom Runtime):
- Docker Hub
- AWS Elastic Container Registry (ECR)
- Google Artifact Registry
- Local registry

**CLI protocols for local images**:
- `docker://localhost/<image>:<tag>` — Docker Image Protocol
- `docker-archive://<file>.tar` — Docker Archive Protocol (via `docker save`)

### How AppSail differs from [[serverless-functions]]
- **Persistent**, always-on web service (vs. event-driven, time-boxed functions)
- **No Catalyst-specific coding template required** — any standard web app works
- Catalyst auto-scales instances based on traffic
- Each service gets an auto-generated AppSail URL; custom domains can be mapped

### Deployment Methods
1. **From the Catalyst CLI**
   - Catalyst-Managed: `catalyst appsail:init` → deploy; also supports standalone deploy and `appsail:add` to attach to an existing project
   - Custom Runtime: init with Docker Image or Docker Archive protocol, then deploy
   - Local testing via localhost is supported for both
2. **From the Catalyst Console**
   - Catalyst-Managed: upload build file in the Serverless section, configure, deploy
   - Custom Runtime: connect a registry integration, point to image URL, deploy

### Configuration: `app-config.json`
Created on init for Catalyst-Managed Runtime apps. Configurable settings:
- **Memory** allocation
- **Disk size**
- **Port** the app listens on (`X_ZOHO_CATALYST_LISTEN_PORT` env var is exposed at runtime)
- **Environment variables**
- **Startup command**
- **Stack / runtime** version

For Custom Runtime, these details are already baked into the OCI image, so most `app-config.json` fields aren't required — only a service name.

### Build File Notes
- Java WAR: main file must be named `root.war` (or use explicit controllers)

### Instance Management
- Catalyst spawns server instances on demand and scales up/down with traffic
- Live instances are visible in the console; individual instances can be deleted
- Execution logs, performance stats, and DevOps [[catalyst-devops]] tools (Logs, APM) apply

### SDK Interop
AppSail apps can still use any Catalyst component via the platform-specific SDK — see the SDK sections below. **Not** available for OCI/Custom Runtime images (since the Catalyst SDK isn't auto-injected into arbitrary container runtimes).

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
