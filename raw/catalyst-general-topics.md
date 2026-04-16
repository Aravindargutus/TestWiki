# Catalyst General Topics — Getting Started, Services Overview, DevOps, Deployment & Billing

Source: https://docs.catalyst.zoho.com/en/
Fetched: 2026-04-16

---

## Introduction to Catalyst (1/10)

Catalyst by Zoho is a full stack, cloud-based, scalable platform that provides developer functionalities to build software services and applications of any scale in various programming environments. You can build solutions ranging from simple microservices and independent software modules to interactive and dynamic cloud apps, enterprise software, robust business suites, and more.

Catalyst is backed by Zoho's powerful, resilient, tried-and-tested infrastructure that ensures solutions are consistent, reliable, and efficient in their performance, and are highly secure.

### Catalyst Services
1. **Catalyst Serverless** — Fully-managed cloud-computing platform providing FaaS and PaaS solutions, supports multiple programming environments.
2. **Catalyst Cloud Scale** — Components for securely building and hosting robust serverless applications and microservices. End-to-end solutions for storage, security, integration, and deployment.
3. **Catalyst Zia Services** — ML and AI-powered microservices (OCR, AutoML, Text Analytics, Image Moderation, etc.)
4. **Catalyst DevOps** — DevOps model implementation: code operations at scale, monitoring, testing, quality checks.
5. **Catalyst SmartBrowz** — Configure and access headless browsers programmatically for web scraping, template-based document generation.
6. **Catalyst QuickML** — No-code ML pipeline builder with end-to-end solutions.
7. **Catalyst ConvoKraft** — Build text-based conversational bots embeddable in Catalyst solutions.
8. **Catalyst Job Scheduling** — Create Job Pools and execute jobs triggering Functions, Circuits, Webhooks, or AppSail.
9. **Catalyst Slate** — Deploy and launch scalable web applications with JS framework support and Git-based automations.
10. **Catalyst Pipelines** — CI/CD solutions for seamless deployment across environments.
11. **Catalyst Signals** — Event Bus Service for event-driven architectures, routing communication between decoupled applications.

### Supported Environments (Client SDKs)
- Web SDK (v4)
- Android SDK (v2)
- iOS SDK (v2)
- Flutter SDK (v2)

### Supported Programming Languages (Server-Side)
- Java (SDK v1) — JDK 8, 11, 17
- Node.js (SDK v2) — v14, 16, 18, 20
- Python (SDK v1) — v3.9 only

### Pricing Models
- **Subscription model** or **Pay-as-you-go model**
- Free-tier benefits in development environment until hard limits exceeded
- Serverless infrastructure = zero downtime

---

## Quick Start Guide (2/10)

### Prerequisites
- Node.js and NPM (for CLI + Node.js functions)
- Java SE Development Kit (for Java functions)
- Python 3.9 (for Python functions)
- Any IDE tool

### 7-Step Quick Start
1. **Create a Catalyst project** — Log in to console, click "Create a new Project", enter name, access project
2. **Install Catalyst CLI** — `npm install -g zcatalyst-cli`, verify with `catalyst --version`
3. **Log in from CLI** — `catalyst login`, sign in via browser, accept permissions
4. **Initialize the project** — `catalyst init` from project folder, select components (functions, client, AppSail), associate with project
5. **Develop the application** — Use SDKs (Java/Node.js/Python/Web/Android/iOS/Flutter) and REST APIs
6. **Test locally** — `catalyst serve` from project directory, access via local URLs
7. **Deploy the project** — `catalyst deploy` to development environment, then deploy to production via console

> First project must be created from web console (not CLI). Subsequent projects can be created from CLI.

---

## Install Catalyst CLI (3/10)

### Introduction
Catalyst CLI contains tools for initializing projects, developing, testing, and deploying components from your local machine. Commands for Cloud Scale, Serverless, SmartBrowz services.

- **Supported OS**: Windows, macOS, Linux
- **Requires**: Node.js v14+ and NPM
- **Package**: `zcatalyst-cli` (via npm or yarn)

### Install
```bash
npm install -g zcatalyst-cli
# Or via Yarn:
yarn add zcatalyst-cli
# Verify:
catalyst --version
```

### Update
```bash
npm install -g zcatalyst-cli
# Or specific version:
npm install -g zcatalyst-cli@1.9.0
```

> CLI works with development environment only. To push to production, deploy via console.

### Programming Language Prerequisites
- **Node.js**: Already present from CLI install
- **Java**: JDK 8, 11, or 17 (exact version recommended)
- **Python**: Version 3.9 from python.org, Pip auto-installed

---

## Catalyst Projects (4/10)

### Key Facts
- Projects are NOT service-specific — access all Catalyst services within a project
- One app per platform per project: 1 web app, 1 Android app, 1 iOS app
- Multiple functions and AppSail solutions per project allowed
- Cross-platform development supported within a single project
- All solutions in same project share backend data (Data Store, File Store, Functions)
- Unique Project ID generated per project
- Max 50 projects per account (can request increase via support)
- Development and production environments per project

### Create a Project
1. Click "Create New Project" from console index page
2. Enter project name (alphanumeric, underscores, hyphens only — no spaces or special chars)
3. Click Create → Access Project

### Console URLs by Data Center
- EU: `console.catalyst.zoho.eu/baas/index`
- IN: `console.catalyst.zoho.in/baas/index`
- US: `console.catalyst.zoho.com/baas/index`

---

## Navigating in the Console (5/10)

### Services and Components
- All services accessible from left menu inside a project
- Components grouped by functionality, features, and purpose
- Must click "Start Exploring" before deploying a service to production
- Serverless and Cloud Scale services have dashboards accessible from service name at top

### Dashboard
Today's Metrics displayed:
- Sync Functions Executions
- Async Functions Executions
- Total Component Calls
- Cron Executions

### Header
- Environment switcher (Development / Production)
- Deploy to Production button
- Notifications for events/processes across services
- User Account info with Catalyst user ID
- Organizations drop-down for multi-org management
- SDK downloads available from Developer Tools settings

---

## Catalyst Organizations (6/10)

### Introduction
- Create and manage multiple organizations from same Catalyst account
- Organization creator is the **owner** by default
- Each org has a unique **Org ID** (part of console URL)
- Collaborators: **Admin** or **Project Member** types
- Console URL format: `https://console.catalyst.zoho.com/baas/{{OrgID}}/index`

### Default Organization
- First-ever org created when you sign up is the default
- Login always goes to the default org (console and CLI)
- API calls execute against the default org unless Org ID explicitly specified
- Cannot delete the default org (must change default first)

### Operations
- **Create**: From multi-org portal → "Create Organization"
- **Switch**: From profile icon drop-down → select org
- **Set Default**: Ellipsis menu → "Set as Default"
- **Delete**: Ellipsis menu → "Remove Org" (owner only, permanent deletion of all projects)

---

## Environments — Deployment & Billing (7/10)

### Two Environments
1. **Development** — Free sandbox for building, configuring, testing. Always available at no cost. Has certain feature limitations.
2. **Production** — Live mode accessible to end users. Billed for API calls.

### Deployment Process
- Development → Production migration of data and configurations
- Can switch between environments in console after deployment
- Must set up payment method before first production deployment

### Environment Sub-Pages
- Development Environment
- Deployment Types
- Initial Deployment
- Production Environment
- Subsequent Deployment
- Billing (payment methods, pricing plans, billing settings)

---

## Catalyst DevOps (8/10)

DevOps service for monitoring, testing, and operations management.

### Monitor
- **Application Alerts** — Automated alerts for failures or specific events across components
- **APM (Application Performance Monitoring)** — Advanced logs and drill-down reports of function executions
- **Logs** — Detailed execution logs with exception/timeout insights
- **Metrics** — Usage metrics with graphs and diagrams

### Repositories
- **GitHub Integration** — Connect Git accounts, deploy repos directly to Catalyst

### Quality
- **Automation Testing** — Execute automated API tests on function endpoints or third-party URLs

---

## Catalyst ConvoKraft (9/10)

AI-powered conversational bot service embeddable in Catalyst solutions.

### Key Components
- **Bots** — Build conversational assistants for user interactions
- **Actions** — Programmed tasks for bots during user interactions (business logic)
- **Handlers** — Functions handling greetings, failures, and exceptions during interactions
- **Bot Operations** — Training, testing, and deploying bots to production

### Developer Tools
- Client JS SDK for ConvoKraft
- Catalyst CLI support
- Server-side logic via Java/Node.js/Python SDKs

---

## Catalyst Signals (10/10)

Event Bus Service for event-driven architectures.

### Key Components
- **Events** — Different event types with schemas and statuses
- **Publishers** — Sources that emit events (default publishers + custom)
- **Targets** — Destinations for events, triggering downstream workflows
- **Webhooks** — HTTP callbacks for event-driven communication between decoupled apps
- **Rules** — Guide event ingestion from publishers to targets based on business requirements

---

## Catalyst Slate (Bonus)

Frontend deployment platform for scalable web applications.

### Key Features
- Native support for JavaScript frameworks (Next.js, React, Vue, etc.)
- Git-based automations (private and public repos)
- Direct upload deployment (zip files)
- CLI deployment support

### Deploy Options
1. Deploy Slate Starter Templates
2. Deploy from Private Repository
3. Deploy from Public Repository
4. Deploy by Direct Upload (build zip)
5. Deploy using CLI

### Configure
- **Cache** — Caching settings per deployment
- **Memory** — Memory allocation (Next.js framework apps)
- **Environment Variables** — Secure env var management
- **Custom Domains** — Map deployments to custom domains

### Manage
- Slate Access URL generation
- Deployment Previews
- Rollback Deployment (revert to previous version)
- Fix Failed Deployment (apply latest commit)
- Monitor Deployment (logs + history)
- Delete Deployment
