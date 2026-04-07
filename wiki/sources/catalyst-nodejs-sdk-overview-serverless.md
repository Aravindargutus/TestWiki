---
title: Catalyst Node.js SDK v2 — Overview & Serverless
type: source
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-nodejs-sdk-overview-serverless.md]
tags: [nodejs, sdk, v2, overview, serverless, functions, circuits, appsail]
---

# Catalyst Node.js SDK v2 — Overview & Serverless

## Summary

The Catalyst Node.js SDK v2 (`zcatalyst-sdk-node` ≥ 2.5.0) provides a promise-based API for accessing all Catalyst components from Node.js serverless functions and AppSail applications. It mirrors the [[catalyst-java-sdk-overview|Java SDK]] in scope but uses JavaScript idioms — promises instead of Java beans, JSON configs instead of typed builders.

## Key Takeaways

- **Installation**: `npm install zcatalyst-sdk-node` (current: 2.5.0; all earlier versions deprecated)
- **Initialization**: `catalyst.initialize(req)` in Advanced I/O, `catalyst.initialize(context)` in Basic I/O / Event / Cron — returns an `app` object for accessing all components
- **Scopes**: Admin (default, unrestricted) and User (restricted) — only apply to [[data-store]], [[file-store]], and [[zcql]]
- **Third-party integration**: External apps can use the SDK via OAuth (refresh token, client ID/secret, project ID, ZAID)  → `catalyst.credential.refreshToken(credentials)` + `catalyst.initializeApp(req)`
- **Cached project data**: Named app instances via `catalyst.initialize(req, { appName: 'name' })`, retrieved later via `catalyst.app('name')`
- **Functions SDK**: `app.functions().execute(idOrName, { args: {...} })` — returns promise
- **Circuits SDK**: `app.circuit().execute(circuitId, name, inputJSON)` + `.status()` + `.abort()` — not available in EU, AU, IN, JP, SA, CA
- **AppSail**: Install SDK, initialize with `catalyst.initialize(req)`, listen on `X_ZOHO_CATALYST_LISTEN_PORT || 9000`

## Entities Mentioned

- [[zoho-catalyst]] — the platform
- [[zcatalyst-sdk-node]] — the npm package (Node.js SDK v2)

## Concepts Introduced

- [[sdk-scopes]] — Admin vs User access
- [[third-party-sdk-integration]] — OAuth-based SDK use outside Catalyst
- [[circuits]] — Workflow orchestration
- [[appsail]] — Managed web service deployment

## Open Questions

- How does the Node.js SDK handle connection pooling in high-concurrency AppSail apps?
- What are the performance differences between Node.js v2 SDK and Java v1 SDK for the same operations?

[Source: catalyst-nodejs-sdk-overview-serverless.md]
