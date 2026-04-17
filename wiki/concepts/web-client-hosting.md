---
title: Web Client Hosting
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: []
tags: [cloud-scale, host-and-manage, web-client, hosting, deployment]
---

# Web Client Hosting

## Definition

Web Client Hosting is a Catalyst Cloud Scale component that hosts the **client-side** of web applications built on Catalyst. Catalyst provides a feature-rich **Web SDK** (JavaScript) for integrating Cloud Scale components into the frontend. One web app is hosted per project. [Source: [Web Client Hosting Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/web-client-hosting/introduction/)]

## Key Aspects

### Scope
- **One web application per Catalyst project**
- Suitable for **basic/simple** web apps. For modern frameworks (Next.js, Angular, React, Vue) with Git-based auto-deploy, use **[[catalyst-slate|Slate]]** instead
- Hosted only in the **Development environment** initially; must deploy the project to Production to make the app live

### Default Application URL
- Catalyst auto-generates a default URL per environment (Development + Production) once the app is hosted
- Accessible from the default URL, or you can map a custom domain via **[[domain-mappings]]**

### Three Deployment Paths
| Method | How |
|---|---|
| **Catalyst CLI** | Deploy web client + functions together or separately via `catalyst deploy` |
| **GitHub Integration** | Connect GitHub account → deploy directly from repo in console |
| **Console Upload** | Upload the web client bundle from the *Web Client Hosting* section (covered in the help doc) |

### Versioning
- Manage versions (upgrade / rollback) from the console
- Production app is unaffected by Dev version updates until explicitly migrated to Production

### Integration with Other Components
- **[[push-notifications]]** — Web push SDK embedded directly
- **[[domain-mappings]]** — Map own domain to the hosted web client
- **[[catalyst-authentication]]** — Auth via Web SDK classes
- All other Cloud Scale components accessible via Web SDK v4

## Sources
- [Web Client Hosting Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/web-client-hosting/introduction/)
- [Web SDK v4](https://docs.catalyst.zoho.com/en/sdk/web/v4/overview/)

## Related Concepts
- [[catalyst-slate]] — Advanced frontend platform (successor for complex apps)
- [[domain-mappings]] — Custom domain mapping
- [[catalyst-environments]] — Dev vs Production hosting
- [[catalyst-devops]] — GitHub Integration deployment path
