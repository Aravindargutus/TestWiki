---
title: Collaborators
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com/en/getting-started/set-up-a-catalyst-project/collaborators]
tags: [catalyst, console, settings, collaborators, admin, super-admin, team, access-control]
---

# Collaborators

## Definition

**Collaborators** are co-developers invited to work on a Catalyst project or across a whole Catalyst account. They are managed from **General Settings → Collaborators** in the console, and their access extends to the **Catalyst CLI** automatically. [Source: docs.catalyst.zoho.com]

## Platform Overview

### Collaborator Hierarchy (highest → lowest)

1. **Super Admin** (exactly **one per account**)
   - The original account creator by default
   - Has every admin permission **plus** the exclusive right to **permanently delete the Catalyst account**
   - Transfer-only: cannot be invited directly; must first become an admin, then be promoted by the current super admin
   - Can create new [[catalyst-organizations]] within the account
2. **Admin** (account-scoped)
   - Access to **all projects** in assigned organizations — implicit Project Owner profile everywhere
   - **Exclusive admin privileges**: Create/Update/Delete Projects, Create/Update/Delete Profiles, Permanently Delete Collaborators, Manage Git Integration, Manage Billing
3. **Project Member** (project-scoped)
   - Access only to assigned projects
   - Permissions defined by the [[profiles-and-permissions]] assigned per project

**Note**: An admin in the org cannot be removed from a specific project — admin access is org-wide.

### Limits

- **100 collaborators per organization** (total admins + members, flexible distribution)
- **10 invites at a time** in a single "Add or Assign" action
- Limit increase available by emailing `support@zohocatalyst.com`

### Invitation Flow

1. **Settings → Collaborators → Add or Assign**
2. Choose **Project Member** (+ project + profile) OR **Admin for the Whole Account**
3. Enter email(s); existing org members auto-suggested
4. Invitees receive email → sign in with their Zoho account (or sign up if none)
5. Until accepted: **"Invitation Pending"** banner; **Reinvite** available

### State Indicators

- **Yellow star** — Admin
- **Orange star** — Super Admin
- **Pending Invites** / **Active Members** / **Admins** / **Deleted Collaborators** — filter categories
- Search by name/email; filter by project to scope view

### Type Transitions

| From | To | Who Can Do It |
|---|---|---|
| Project Member | Admin | Any Admin |
| Admin | Project Member | Any Admin |
| Admin | Super Admin | **Only current Super Admin** |
| Project Member | Super Admin | Two-step (Member→Admin, then Admin→Super Admin) |

### Removal Semantics

- **Remove from Project** — member loses access to one project; profile remains for other projects
  - Requires the `Add/Update/Remove Collaborators` permission or admin role
  - **Admins cannot be removed from individual projects** (org-wide access)
- **Delete Collaborator (permanent)** — entire org access revoked; typing `DELETE` required
  - Invitees with unaccepted invites: delete invalidates their invite link
  - Removed collaborators still see their own orgs and Zoho account
  - Listed under "Deleted Collaborators" filter afterward
- **Delete Catalyst Account (Super Admin only)** — all projects/resources/orgs erased
  - **Must delete all collaborators first**
  - Pending bill payments block account deletion

### CLI Access Parity

Project members can only pull/deploy projects they're assigned to. Admins can access every project in the account via the CLI.

## Key Aspects

- Three collaborator types: Super Admin (1) → Admin (N) → Project Member (N)
- Super Admin = sole account deleter; transfer-only; can be changed (not invited)
- 100-collaborator org cap; 10-invite batch limit
- Admin permissions: project CRUD, profile CRUD, Git/Billing management, permanent collaborator deletion
- Permissions applied per-project for members (via Profiles), org-wide for admins
- Console + CLI access parity — same membership boundary

## Sources

- [docs.catalyst.zoho.com/en/getting-started/set-up-a-catalyst-project/collaborators](https://docs.catalyst.zoho.com/en/getting-started/set-up-a-catalyst-project/collaborators/)

## Related Concepts

- [[profiles-and-permissions]] — Defines what project members can do in each project
- [[catalyst-organizations]] — Collaborators exist within orgs; admins are org-wide
- [[catalyst-projects]] — Assignment unit for project members
- [[catalyst-console]] — Settings → Collaborators UI surface
- [[catalyst-cli]] — CLI inherits the same collaborator-based access
- [[audit-logs]] — Tracks collaborator activity per project
- [[iac-settings]] — Admin-only project export/import
