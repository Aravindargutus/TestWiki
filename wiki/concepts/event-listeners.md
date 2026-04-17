---
title: Event Listeners
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [catalyst-serverless-functions.md]
tags: [cloud-scale, event-bus, events, listeners, catalyst]
---

# Event Listeners

## Definition

Event Listeners is a Catalyst Cloud Scale component functioning as an **event bus service**. It listens for predefined events and automatically triggers the corresponding [[event-functions]] or [[circuits]]. It enables event-driven architecture for automating recurring tasks tied to specific events. [Source: [Event Listeners Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/event-listeners/introduction/)]

> **Note**: Catalyst now offers **[[catalyst-signals]]** as a brand-new event bus service. Signals is positioned as a significant upgrade to Cloud Scale Event Listeners.

## Key Aspects

### 3 Types of Event Listeners

| Type | Trigger | Created |
|---|---|---|
| **Catalyst Component Event Listener** | An event occurring in a Catalyst component (Data Store, Cache, etc.) | **By default** (one per project) |
| **Custom Event Listener** | An auto-generated URL being invoked externally | On demand |
| **Zoho Events Listener** | An event in an external configured Zoho service (e.g., Zoho CRM) | On demand |

### Supported Component Events (for Component Event Listener)

| Component | Events | Description |
|---|---|---|
| **[[data-store]]** | Insert, Update, Delete | Row-level changes in associated tables |
| **[[cache]]** | Put | Data written to associated Cache segments |
| **[[catalyst-authentication]]** | Sign Up, Delete | User registration or deletion |
| **[[file-store]]** | Upload | File uploaded to associated folders |
| **GitHub** | Success, Failure | GitHub deployment outcome |
| **Web Client** | Hosted | Web client hosting event |

### Targets
An event listener's rule can invoke:
- An [[event-functions|Event Function]]
- A [[circuits|Circuit]]

### Console Capabilities
- Configure listeners and per-rule targets
- View performance statistics
- Browse **Queued Events** and **Processed Events**
- See sample data payload that will be sent to the target
- Configure **application alerts** directly from the Rules section (email alerts on failure, code exception, or process timeout)

### CLI & SDK Limitations
- **Cannot** configure or manage event listeners from the CLI or SDKs
- **Can** generate sample payloads via `catalyst event:generate` for testing event functions locally

## Relationship to Other Concepts
- [[event-functions]] are a common target — the function type designed specifically to be invoked by listeners
- [[circuits]] can also be an event listener target
- [[catalyst-signals]] is the successor event bus service

## Sources
- [Event Listeners Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/event-listeners/introduction/) (Introduction, Processed Events, Queued Events)
- [[catalyst-serverless-functions]] — Event Functions section references listeners

## Related Concepts
- [[event-functions]]
- [[circuits]]
- [[catalyst-signals]] — Upgraded event bus
- [[data-store]], [[cache]], [[catalyst-authentication]], [[file-store]] — Event sources
