---
title: Application Alerts
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com/en/devops/help/application-alerts]
tags: [catalyst, devops, monitoring, alerts, email, cron, event-listeners, logs]
---

# Application Alerts

## Definition

**Application Alerts** is a Catalyst **DevOps** component that sends **email alerts** whenever failures or specific events occur in three other components: [[cron]], [[event-listeners]], and [[catalyst-logs]]. Alerts are criteria + frequency-based and can aggregate multiple entities per rule. [Source: docs.catalyst.zoho.com]

## Platform Overview

### Scope

A single alert binds to entities of **one component only** (Cron, Event Listener, **or** Logs — never mixed). Multiple alerts can be configured per project; multiple entities of the same component can be grouped into one alert.

### Cron & Event Listener Alert Conditions

- **Failure** — execution of the cron/event listener (or the associated function) failed
- **Code Exception** — an exception triggered the exception handler
- **Timeout** — the execution ran until timeout

Any combination of the three can be selected per alert.

### Logs Alerts

Automate a log-search query. Inputs:
- **Log type**: `Access Logs` or `Application Logs`
- **Function(s)** to search
- **Keyword** string
- For **Application Logs**: severity level (`Info`, `Error`, `Severe`, `Warning`; Node.js also has `Uncaught Exception`, `Unhandled Rejection`)

### Criteria and Frequency

- **Criteria**: comparator (`>`, `<`, `=`, `>=`) + numeric threshold on the count of events/results
- **Frequency**: time-window intervals (e.g., every 15 min / 1 hr / 12 hr) or a specific time-of-day
- For multiple conditions, threshold is checked **collectively**
- Catalyst aggregates matching events in the frequency window and sends one email per window

### Limits

- **Development environment**: up to **5 alerts**
- **Production environment**: up to **20 alerts**

### Notification

Emails go to the recipient list configured per alert and include context details for immediate action.

## Key Aspects

- Only fires for Cron/Event Listeners/Logs — not all components
- One alert = one component; no cross-component alerts
- Log alerts support both Access and Application logs with keyword + level filters
- Threshold + frequency must both match before an email is sent

## Sources

- [docs.catalyst.zoho.com/en/devops/help/application-alerts/introduction](https://docs.catalyst.zoho.com/en/devops/help/application-alerts/introduction)
- [docs.catalyst.zoho.com/en/devops/help/application-alerts/key-concepts](https://docs.catalyst.zoho.com/en/devops/help/application-alerts/key-concepts)

## Related Concepts

- [[catalyst-devops]] — Parent DevOps umbrella
- [[catalyst-logs]] — Log-search alerts source
- [[cron]] — Cron failure/timeout/exception alerts
- [[event-listeners]] — Event-listener failure/timeout/exception alerts
- [[apm]] — Complementary deep-monitoring tool
- [[catalyst-environments]] — 5 dev vs 20 prod limit
