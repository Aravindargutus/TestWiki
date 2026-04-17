---
title: Integration Functions
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [functions, integration, cliq, zoho, serverless, catalyst]
---

# Integration Functions

## Definition

Integration Functions are [[serverless-functions]] in [[zoho-catalyst]] that serve as the backend for other Zoho services. Currently only **Zoho Cliq** is supported. They are NOT accessible via URL — they are invoked internally by the external Zoho service. [Source: catalyst-serverless-functions.md]

## Key Aspects

### Data Center Availability
**NOT available** in: EU, AU, IN, JP, SA, CA data centers. Available in US (and any other DCs not on the exclusion list).

### Zoho Cliq Integration
Includes the Cliq SDK with handler classes for:
- **Bots** — Automated chatbot handlers
- **Commands** — Slash command handlers
- **Message Actions** — Context menu actions on messages
- **Widgets** — UI widget handlers
- **Functions** — Custom function handlers for Cliq

### Constraints
- No Function URL
- Cannot be invoked manually or via HTTP
- Invoked only by the integrated Zoho service (Cliq)
- Limited data center availability
- Tightly coupled to the external service's SDK

### When to Use
- Building Zoho Cliq bots and integrations
- Custom slash commands for Cliq
- Cliq message action handlers
- Only when operating in a supported data center (not EU/AU/IN/JP/SA/CA)

## Sources

- [[catalyst-serverless-functions]] — Help documentation

## Related Concepts

- [[serverless-functions]] — Parent concept
- [[zoho-catalyst]] — Platform; note DC restrictions
