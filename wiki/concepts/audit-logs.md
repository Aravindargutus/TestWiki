---
title: Audit Logs
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com audit-logs settings]
tags: [catalyst, audit, compliance, hipaa, gdpr, settings, admin]
---

# Audit Logs

## Definition

**Audit Logs** is a Catalyst **account-level settings feature** that records activity history across all projects in an account. It tracks **who did what, when, and where** for both console configuration changes and sensitive-data operations — primarily for compliance with GDPR, HIPAA, and similar regulations. [Source: docs.catalyst.zoho.com]

## Platform Overview

### Two Log Categories

- **Console Logs** — metadata / configuration activity across components
  - Examples: adding a Data Store column, updating an Event Listener rule, deleting a cron job, changing sign-in methods, modifying row/folder permissions
  - **Development environment only** (config ops aren't performed in prod)
  - Filter by: Project, Features, Operations (Create / Update / Delete)
- **Application Logs** — activity on **sensitive data** in storage components
  - Fires on: insert/update/delete of rows in a column marked **sensitive** (or with **ePHI validator** enabled) in [[data-store]]
  - Fires on: create/delete of files in a folder marked **sensitive** in [[file-store]]
  - Available in **both development and production** environments (separate CSV files per env on export)

### Access Control

- **Admins** access all audit logs by default
- **Editors / Viewers** require an **exclusive Audit Logs permission** granted via **Profiles and Permissions** — stricter than typical feature access
- Only **admins** can export **application logs** to local systems (HIPAA compliance requirement)

### Retention and Export

- **Online view**: last **1 year** of logs across all projects
- **Export**: downloadable ZIP containing logs from **up to 6 years back**
- Export structure: one CSV per half-year (H1 / H2) × up to 6 years
- Current year: separate CSVs for **development** and **production** environments for application logs (single CSV for console logs)
- After first export, download link persists in the Export Logs dialog for re-download

### Compliance Context

Catalyst allows marking Data Store **columns** and File Store **folders** as:
- **Sensitive** — generic data protection tag
- **ePHI validator enabled** — HIPAA-specific PHI marker

Both trigger application-log capture.

## Key Aspects

- Console Logs = config changes (dev only); Application Logs = sensitive data ops (dev + prod)
- 1-year UI retention, 6-year export retention
- Dedicated Audit Logs permission required for non-admins
- Application log export restricted to account admins (HIPAA)
- Filter dimensions: Project, Feature, Operation, Environment

## Sources

- Catalyst Audit Logs (Getting Started → Settings)
- Profiles and Permissions docs
- Data Store sensitive column + File Store sensitive folder docs

## Related Concepts

- [[catalyst-projects]] — Audit scope (all projects in account)
- [[data-store]] — Sensitive columns trigger Application Logs
- [[file-store]] — Sensitive folders trigger Application Logs
- [[catalyst-logs]] — Different: function execution logs, not audit
- [[catalyst-environments]] — Console Logs dev only; Application Logs both
- [[catalyst-organizations]] — Account-level scope
