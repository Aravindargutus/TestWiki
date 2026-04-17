---
title: Environment Variables
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com environment-variables serverless appsail slate]
tags: [catalyst, environment-variables, configuration, functions, appsail, slate, secrets]
---

# Environment Variables

## Definition

**Environment Variables** are key-value configuration entries declared **outside** of application source code that Catalyst injects into runtime at execution time. A single key can hold **different values per environment** (Development vs Production), enabling the same code to behave differently across stages without edits. [Source: docs.catalyst.zoho.com]

## Platform Overview

### Scope of Support

Environment variables are configured **per component**:

- **[[serverless-functions]]** — per-function Configuration tab; scoped to all sub-functions within a function directory
- **[[appsail]]** — via `app-config.json` (`env_variables` key) for CLI-deployed apps, or via the Console Configurations tab for console-deployed apps; supports both **Catalyst-Managed Runtimes** and **Custom Runtimes**; pushed into the platform instance at spawn time
- **[[catalyst-slate]]** — per deployment via Configuration → Environment Variables in the deployment pane; applies to private Git, public repo, or template-based deployments

### Dual-Environment Values

- Declare a **single key**, configure **separate values** for Development and Production
- Use drop-down toggle in console to switch views between the two
- **Production values** only configurable/accessible once the project is deployed to Production
- If production environment is **disabled**, production variables are inaccessible

### Runtime Access Syntax

| Runtime | Access |
|---|---|
| **Java** | `System.getenv("keyname")` |
| **Node.js** | `process.env.keyname` (or `process.env["keyname"]`) |
| **Python** | `os.getenv("keyname")` |

### Special Platform Variables

- **`ZOHO_CATALYST_ZCQL_PARSER`** → set value to `V2` to opt into ZCQL V2 syntax in function code
- **`X_ZOHO_CATALYST_LISTEN_PORT`** → injected by AppSail; the HTTP listen port the app must bind to (e.g., `sh -c 'python3 -m http.server ${X_ZOHO_CATALYST_LISTEN_PORT}'`)

### Slate-Specific Notes

- Default scope is **Development** for apps deployed via Git/Slate template/public repo
- Access in code with standard runtime syntax (e.g., `process.env.[variable_name]` for Node.js)
- Production variables require prior deployment to production

### AppSail Configuration Paths

- **CLI-deployed**: `app-config.json` → `env_variables` object (set at init/add/deploy time)
- **Standalone CLI deploy**: configure in the console afterwards
- **Console-deployed**: Configurations tab on the deployment page

## Key Aspects

- One key, two values (dev + prod) — single declaration per key
- Runtime-specific access syntax (Java / Node.js / Python)
- Supports functions, AppSail (managed + custom runtimes), and Slate
- Special system vars: `X_ZOHO_CATALYST_LISTEN_PORT` (AppSail port), `ZOHO_CATALYST_ZCQL_PARSER` (opt-in ZCQL V2)
- Production values locked until initial production deployment is complete

## Sources

- Catalyst Functions → Configuration → Environment Variables
- AppSail Catalyst-Managed Runtimes / Custom Runtimes Key Concepts
- Slate Environment Variables docs

## Related Concepts

- [[catalyst-config]] — Function-level config file (separate from env vars)
- [[serverless-functions]] — Primary consumer of function-scoped env vars
- [[appsail]] — Uses env vars via `app-config.json` or console
- [[catalyst-slate]] — Per-deployment env vars for frontend apps
- [[catalyst-environments]] — Dev vs Prod value separation
- [[zcql]] — `ZOHO_CATALYST_ZCQL_PARSER=V2` opt-in path
