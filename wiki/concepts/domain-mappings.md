---
title: Domain Mappings
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: []
tags: [cloud-scale, host-and-manage, custom-domain, ssl, domain-mapping]
---

# Domain Mappings

## Definition

Domain Mappings is a Catalyst Cloud Scale component that maps a **custom (owned) domain** to the production URL of a Catalyst application, enabling end-users to access the app from a branded domain instead of the default Catalyst URL. Includes free **Group SSL certificates** for all mapped domains. [Source: [Domain Mappings Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/domain-mappings/introduction/)]

## Key Aspects

### Prerequisites
- Domain must already be **hosted live externally** before it can be mapped
- DNS control is required to configure records pointing to Catalyst

### Mapping Rules
| Rule | Value |
|---|---|
| **Max domains per app** | **5** |
| **Domain uniqueness** | A single domain can map to only **one** Catalyst application |
| **Default URL** | Always remains active after mapping (dev + prod URLs are not disabled) |
| **Disable / Delete** | Can be toggled off any time, reverting to default URL |

### SSL / Security
- **Group SSL certificates** provided by Catalyst at **no cost** for all mapped domains
- Managed automatically from the console

### Target
Maps to the **Production URL** of the hosted application. Typically used with [[web-client-hosting]] but conceptually applies to any app with a public URL.

### Console Experience
- Integrated section in the Catalyst console to create, view, enable/disable, and delete domain mappings per project
- Unified management of all mapped domains across a project

## Sources
- [Domain Mappings Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/domain-mappings/introduction/)
- [Domain Mappings Architecture](https://docs.catalyst.zoho.com/en/cloud-scale/help/domain-mappings/architecture/)
- [Domain Mappings Implementation](https://docs.catalyst.zoho.com/en/cloud-scale/help/domain-mappings/implementation/)

## Related Concepts
- [[web-client-hosting]] — Primary target for domain mapping
- [[mobile-device-management]] — Sibling "Host and Manage" component
- [[catalyst-environments]] — Domain mapping targets Production URL
- [[catalyst-slate]] — Slate has its own custom-domain flow separate from Domain Mappings
