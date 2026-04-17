---
title: Profiles and Permissions
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com/en/getting-started/set-up-a-catalyst-project/profiles-and-permissions]
tags: [catalyst, console, settings, profiles, permissions, rbac, access-control, gdpr, hipaa]
---

# Profiles and Permissions

## Definition

**Profiles** are bundles of permissions that define what a **project member** can do inside a Catalyst project. Permissions are **project-scoped** — the same user can hold different profiles across different projects. Profiles do **not** apply to account admins or the Super Admin (they have org-wide access by role). [Source: docs.catalyst.zoho.com]

## Platform Overview

### Access Types

- **Editor** — modify configs + data in permitted components; available for **all** permissions
- **Viewer** — read-only on permitted components; available for only **4** permissions: Development Environment, Production Environment, Manage Settings, Configuration

### Permission Categories

1. **Manage Collaborators**
   - `Add/Update/Remove Collaborators` — invite members, change profiles, remove from projects
2. **Development Environment** / **Production Environment** (mirrored pair)
   - `Data Store` — GDPR/HIPAA-sensitive, listed separately
   - `File Store` — GDPR/HIPAA-sensitive, listed separately
   - `Logs` — sensitive, listed separately
   - `Event Listeners` — sensitive, listed separately (requires Data Store + File Store prior)
   - `Other Components` — Circuits, Functions, Cron, Cache, etc. bundled
   - **"Other Components" in Development is the minimum permission** — cannot be unselected
   - In **Production**: selecting any of Data Store / File Store / Logs / Event Listeners auto-selects Other Components
3. **Manage Add-On Services**
   - `Enable/Disable Add-On Services` — toggle Catalyst Add-Ons for the project
4. **Manage Settings and Configurations**
   - `Perform Migrations` — dev↔prod project migration
   - `Enable/Disable Production Environment` — flip prod on/off post-deployment
   - `Access Billing Components` — reports, budgets (NOT Overview or Manage Billing — admin-only)
   - `Add/Delete Mobile Packages` — Android/iOS SDK packages in Developer Tools
   - `Access Audit Logs` — view [[audit-logs]]; **requires Data Store + File Store + Event Listeners + Other Components permissions first**

### Default Profiles (cannot be modified or deleted)

| Profile | Permissions |
|---|---|
| **Project Owner** | Editor access to **all** permissions (highest project-member tier; still excludes account-admin exclusives) |
| **Contributor** | Editor on Dev + Prod environments, plus `Perform Migrations` + `Enable/Disable Production Environment`. No Manage-Collaborators, Add-On, Billing, Mobile, or Audit Logs access. |
| **Viewer (profile)** | Viewer access to Dev + Prod environments only. Note: **not** the same as the Viewer **access type** — the Viewer profile is one pre-built profile; custom profiles can also use the Viewer access type selectively. |

### Custom Profiles

- **50 custom profiles** per account in **Development** (no cap in Production)
- Created by account admin or Super Admin only (project members cannot create profiles)
- **Add New Profile** workflow: name + description → pick Access Type → pick permissions (or `Select All`) → `Confirm`
- `Other Components` in Dev Environment is **mandatory** on every profile — cannot be deselected
- Edit / Delete available via ellipsis
- **Delete requires replacement**: pick a replacement profile → all assigned members migrate to it

### Compliance Alignment

The separation of Data Store / File Store / Logs / Event Listeners from "Other Components" exists specifically to provide **granular control over sensitive user data** per **GDPR** and **HIPAA** guidelines. This dovetails with [[audit-logs]] (sensitive-data activity tracking) and [[data-store]] / [[file-store]] sensitive-column/folder markers.

### Assignment Semantics

- Project member is assigned a profile **per project** at invite time
- Profile can be changed anytime via Collaborators → member → Change Profile (requires `Add/Update/Remove Collaborators` permission)
- Same person can be Project Owner in project A and Viewer in project B

### Admin Override

Catalyst account admins and the Super Admin **bypass profiles entirely** — they have implicit Project Owner on every project plus additional admin-only privileges (see [[collaborators]]).

## Key Aspects

- Project-scoped RBAC; same user → different profiles per project
- 3 default profiles (Project Owner, Contributor, Viewer) — immutable
- 50 custom profiles/account in dev; unlimited in prod
- Sensitive-data components (Data Store, File Store, Logs, Event Listeners) separated for GDPR/HIPAA
- "Other Components" in Dev = mandatory minimum permission
- Access Audit Logs permission has explicit prerequisites (Data Store + File Store + Event Listeners + Other Components)
- Delete-with-replacement: profile deletion requires a replacement profile for migration

## Sources

- [docs.catalyst.zoho.com/en/getting-started/set-up-a-catalyst-project/profiles-and-permissions](https://docs.catalyst.zoho.com/en/getting-started/set-up-a-catalyst-project/profiles-and-permissions/)

## Related Concepts

- [[collaborators]] — Profiles assigned to project members
- [[audit-logs]] — Access requires specific permission prerequisites
- [[catalyst-environments]] — Dev vs Prod permissions mirror each other
- [[data-store]] / [[file-store]] / [[event-listeners]] / [[catalyst-logs]] — Separately-permissioned sensitive components
- [[catalyst-projects]] — Profile scope
- [[catalyst-organizations]] — Profile storage scope
- [[security-rules]] — Different concept: runtime function access, not console permissions
