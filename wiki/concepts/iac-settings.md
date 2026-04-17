---
title: IaC Settings (Infrastructure as Code)
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [docs.catalyst.zoho.com/en/getting-started/set-up-a-catalyst-project/iac-settings]
tags: [catalyst, console, settings, iac, infrastructure-as-code, export, import, project-template]
---

# IaC Settings (Infrastructure as Code)

## Definition

**Catalyst IaC Settings** provide **project export** and **project import** from the console (and CLI). A project's full schema — component configurations, function code, client code — is packaged as a standard-format ZIP for cross-account / cross-DC transfer, duplication, or Git-based version control. **Data is not included.** [Source: docs.catalyst.zoho.com]

## Platform Overview

### What Gets Exported (config, not data)

**Included:**
- [[data-store]] table + column **metadata** (schema only, not rows)
- [[file-store]] folder **structure** (no files)
- Functions directory — each function as a ZIP with `catalyst-config.json` and SDK components
- Web client directory — initialized client as a ZIP
- **`project-template.json`** — master config file containing all component definitions

**Excluded (by design):**
- Data Store **rows**
- File Store **files**
- Authentication **users**
- Any other runtime-captured data

### `project-template.json` Structure

Canonical JSON manifest listing every project component with `type`, `name`, `properties`, and `dependsOn` relationships. Components captured:

- `Circuits` — states, transitions, configs
- `Functions` — stack, memory, type (basicio/advancedio/event/cron/etc.), code path
- `WebClient` — app name + code path
- `Cron` — schedule details, URL/job, timezone
- `Datastore` — tables + columns + `tableScope` + `tablePermission` per role
- Plus: Cache segments, Security Rules, Event Listeners + rules, API Gateway APIs, Email Templates, Email domains, Profiles (from [[profiles-and-permissions]]), etc.

**`dependsOn`** arrays enforce import ordering (e.g., a column depends on its table).

### ZIP Format

```
project.zip
├── functions/        (ZIP per function, standard project-directory-structure)
├── webclient/        (ZIP of the initialized client)
└── project-template.json   ← MANDATORY for import
```

**Without `project-template.json`, import fails.** Functions and webclient directories are optional.

### Export Flow

1. **Settings → General Settings → Infrastructure as Code → IaC Exports** tab
2. **Export a Project** → select project + environment (Dev or Prod if prod enabled)
3. `Export ID` generated → status displayed in console
4. On success: **Download ZIP** link available for **15 days**
5. **Re-Export**: overwrites previous export; only the **latest** export ZIP is stored per project
6. Error path: `View Error Log` link for diagnosis

### Import Flow

1. **Settings → Infrastructure as Code → IaC Imports** → **Import New Project**
2. Provide **new project name** + upload the source ZIP
3. `Import ID` generated → status displayed
4. On success: **Access Project** link opens the newly-created project
5. Error path: `View Error Log` for diagnosis

**Restriction**: you **cannot import directly into Production** — imports always land in the **Development** environment. Migrate via normal [[catalyst-environments]] deployment afterward.

### Use Cases

- **Cross-DC transfer** — move a project between Zoho data centers (US/EU/IN/AU/JP/SA/CA)
- **Cross-account transfer** — hand off a project to another Catalyst account
- **Git-based versioning** — commit the exported ZIP (or its contents) to Git; clone + import elsewhere
- **Duplicate / branch** — create parallel copies for different use cases
- **CI automation** — scripted export + import via CLI ([CLI Export & Import docs](https://docs.catalyst.zoho.com/en/cli/v1/export-and-import-projects/introduction/))

### CLI Parity

All IaC operations have CLI equivalents: export to local directory, import from local ZIP, generate import-ready ZIP, check export/import job status by `Export ID` / `Import ID`. CLI is the preferred surface for automation.

### Permission Requirements

- Exporting/Importing requires an **account admin** role (creating projects is an admin-exclusive permission — see [[collaborators]])
- Project members can trigger exports on projects they have access to but cannot create new projects via import

## Key Aspects

- Config + code only; **no runtime data**
- `project-template.json` is the single source of truth; mandatory for import
- 15-day download link expiry; only latest export stored per project
- Imports always land in **Dev** — no direct-to-prod import
- `dependsOn` array encodes component ordering for safe import
- CLI-first automation path via `Export ID` / `Import ID`
- Enables cross-DC, cross-account, Git-versioned, and branched workflows

## Sources

- [docs.catalyst.zoho.com/en/getting-started/set-up-a-catalyst-project/iac-settings](https://docs.catalyst.zoho.com/en/getting-started/set-up-a-catalyst-project/iac-settings/)

## Related Concepts

- [[catalyst-projects]] — Export/import unit
- [[catalyst-environments]] — Exports capture one env; imports land in Dev
- [[catalyst-cli]] — Primary automation surface
- [[catalyst-pipelines]] — Complementary CI/CD for deployment (not schema transfer)
- [[collaborators]] / [[profiles-and-permissions]] — Admin-only for creating imported projects
- [[data-store]] / [[file-store]] — Schema exported, data excluded
