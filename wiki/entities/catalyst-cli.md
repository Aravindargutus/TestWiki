---
title: Catalyst CLI
type: entity
created: 2026-04-16
updated: 2026-04-16
sources: [catalyst-general-topics.md]
tags: [catalyst, cli, npm, development-tool]
---

# Catalyst CLI

## Overview

`zcatalyst-cli` is the npm package providing command-line tools for initializing, developing, testing, and deploying Catalyst projects from a local machine. It supports all major Catalyst services (Cloud Scale, Serverless, SmartBrowz) and works across Windows, macOS, and Linux. [Source: catalyst-general-topics.md]

## Key Facts

| Property | Value |
|---|---|
| Package name | `zcatalyst-cli` |
| Install | `npm install -g zcatalyst-cli` |
| Alt install | `yarn add zcatalyst-cli` |
| Requires | Node.js v14+ |
| Supported OS | Windows, macOS, Linux |

## Key Commands

| Command | Purpose |
|---------|---------|
| `catalyst login` | Authenticate with Zoho account via browser |
| `catalyst init` | Initialize project directory, select components |
| `catalyst serve` | Test locally (functions, client, AppSail) |
| `catalyst deploy` | Deploy to development environment |
| `catalyst --version` | Verify installation |

## Appearances

- [[catalyst-general-topics]] — CLI installation, usage in Quick Start Guide

## Relationships

- Part of [[zoho-catalyst]] developer toolchain
- Works with [[catalyst-environments|development environment]] only — production deploy via console
- Initializes [[serverless-functions]], [[appsail]], and Cloud Scale client

## Notes

- First project must be created from web console, not CLI. Subsequent projects can be created from CLI.
- CLI works in development environment only. Production deployment requires console.
- `sudo` may be needed for install on macOS/Linux.
