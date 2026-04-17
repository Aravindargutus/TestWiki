---
title: Mobile Device Management (MDM)
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: []
tags: [cloud-scale, host-and-manage, mdm, mobile, ios, android, manageengine]
---

# Mobile Device Management (MDM)

## Definition

Mobile Device Management is a Catalyst Cloud Scale component for **hosting, distributing, and managing** Android and iOS mobile apps. It is built on **ManageEngine Mobile Device Manager Plus** and provides enterprise-grade MDM integrated with the Catalyst console. One Android app + one iOS app are hosted per project. [Source: [MDM Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/mobile-device-management/introduction/)]

## Key Aspects

### Hosting Scope
- **One Android app + one iOS app per project**
- Host and manage app versions from the console
- **Live only in Production**: Dev hosting is for testing only; users receive the app only after deploying the Catalyst project to Production

### User Distribution
- **Invite end-users from the console** to install the mobile apps
- Track devices per invite (installed counts, user statistics)

### Integration
- **[[push-notifications]]** for iOS (APNs) and Android (FCM) — send test pushes directly from console
- Tied to [[catalyst-authentication]] for user identity

### Powered By
ManageEngine [Mobile Device Manager Plus](https://www.manageengine.com/mobile-device-management/) — enterprise MDM engine.

### Limitations
- **Not CLI-deployable** — unlike web clients, mobile resources cannot be deployed via Catalyst CLI; console-only
- **Not available in EU, AU, IN, JP, SA, or CA data centers** — restricted to other DCs (e.g., US)

## Sources
- [MDM Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/mobile-device-management/introduction/)
- ManageEngine Mobile Device Manager Plus (powering engine)

## Related Concepts
- [[push-notifications]] — iOS/Android push via MDM-hosted apps
- [[catalyst-environments]] — Production-only live distribution
- [[web-client-hosting]] — Sibling "Host and Manage" component for web
- [[domain-mappings]] — Sibling component for custom domains
