---
title: Node.js SDK Initialization
type: concept
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-nodejs-sdk-overview-serverless.md]
tags: [nodejs, sdk, initialization, scopes, promises]
---

# Node.js SDK Initialization

## Definition

The process of creating a Catalyst app object in Node.js that provides access to all Catalyst components. Unlike the [[zcproject|Java SDK's class-based approach]], the Node.js SDK uses a functional initialization pattern that returns a promise-compatible app object.

## Key Aspects

### Initialization by Function Type

| Function Type | Initialization |
|---|---|
| Advanced I/O (Basic) | `catalyst.initialize(req)` |
| Advanced I/O (Express) | `catalyst.initialize(req)` inside route handler |
| Basic I/O | `catalyst.initialize(context)` |
| Event Functions | `catalyst.initialize(context)` |
| Cron Functions | `catalyst.initialize(context)` |
| AppSail | `catalyst.initialize(req)` |
| Third-party | `catalyst.initializeApp(req)` with credentials |

### Scope Initialization

```js
const adminApp = catalyst.initialize(req, { scope: 'admin' }); // full access
const userApp = catalyst.initialize(req, { scope: 'user' });   // restricted
```

Scopes only apply to [[data-store]], [[file-store]], and [[zcql]].

### Named App Caching

```js
catalyst.initialize(req, { scope: 'user', appName: 'user_app' });
const app = catalyst.app('user_app'); // retrieve cached instance later
```

## Sources

- [[catalyst-nodejs-sdk-overview-serverless]]

## Related Concepts

- [[sdk-scopes]] — Admin vs User access levels
- [[instance-objects]] — Java SDK's equivalent pattern
- [[third-party-sdk-integration]] — External app initialization via OAuth

[Source: catalyst-nodejs-sdk-overview-serverless.md]
