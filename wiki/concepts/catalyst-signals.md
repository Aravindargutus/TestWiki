---
title: Catalyst Signals
type: concept
created: 2026-04-16
updated: 2026-04-16
sources: [catalyst-general-topics.md]
tags: [catalyst, signals, event-bus, event-driven, architecture]
---

# Catalyst Signals

## Definition

Catalyst Signals is an Event Bus Service that enables event-driven architectures for communication between decoupled applications. It routes events from publishers to targets based on configurable rules. [Source: catalyst-general-topics.md]

## Key Aspects

### Components
- **Events** — Different types with schemas and statuses, representing things that happen in the system
- **Publishers** — Sources that emit events (default publishers for Catalyst services + custom publishers)
- **Targets** — Destinations for events, triggering downstream workflows (e.g., Functions, Webhooks)
- **Webhooks** — HTTP callbacks for event-driven communication between decoupled applications
- **Rules** — Logic that guides event routing from publishers to targets based on business requirements

### Architecture Pattern
Publishers → Events → Rules (filtering/routing) → Targets → Downstream Workflows

## Sources

- [[catalyst-general-topics]]

## Related Concepts

- [[event-functions]] — Previous event-driven mechanism (triggered by Event Listeners for DataStore, Cache, Auth, etc.)
- [[circuits]] — Workflow orchestration that can be triggered by events
- [[job-scheduling-sdk]] — Scheduled execution complements event-driven execution

## Evolution

_Signals appears to be a more formalized and extensible event-driven system compared to the simpler Event Listener mechanism used by [[event-functions]]. Event Functions respond to specific component events; Signals provides a full event bus with publishers, targets, and routing rules._

[Source: catalyst-general-topics.md]
