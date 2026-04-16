---
title: Zoho Catalyst
type: entity
created: 2026-04-05
updated: 2026-04-16
sources: [catalyst-java-sdk-overview.md, catalyst-java-sdk-authentication.md, catalyst-java-sdk-data-store.md, catalyst-java-sdk-stratus.md, catalyst-serverless-functions.md, catalyst-java-sdk-cloud-scale-remaining.md, catalyst-java-sdk-zia-services.md, catalyst-java-sdk-smartbrowz.md, catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md, catalyst-java-sdk-nosql-security-apigateway.md, catalyst-nodejs-sdk-overview-serverless.md, catalyst-nodejs-sdk-cloud-scale-core.md, catalyst-nodejs-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md, catalyst-general-topics.md]
tags: [zoho, catalyst, serverless, baas, platform]
---

# Zoho Catalyst

## Overview

Zoho Catalyst is a serverless Backend-as-a-Service (BaaS) platform by Zoho Corporation. It provides cloud-scale components for building microservices, web, and mobile applications without managing infrastructure. [Source: catalyst-java-sdk-overview.md]

## Key Facts

- Provides SDKs for Java (v1), Node.js (v2), Python (v1), Web (v4), Android (v2), iOS (v2), Flutter (v2)
- Java SDK supports Java 8, 11, 17
- Node.js SDK package: `zcatalyst-sdk-node` (current: 2.5.0)
- Console at: console.catalyst.zoho.com
- API domain: api.catalyst.zoho.com
- Supports development and production environments
- Uses OAuth2 for authentication (via api-console.zoho.com)

## Components (11 Services)

The platform provides 11 major services [Source: catalyst-general-topics.md]:

1. **Catalyst Serverless** — Functions, cron jobs, FaaS
2. **Catalyst Cloud Scale** — [[data-store]], [[nosql]], [[file-store]], [[zcql]], [[stratus]], Authentication, [[cache]], [[catalyst-search]], [[catalyst-mail]], [[push-notifications]], [[connections-sdk]]
3. **Catalyst Zia Services** — AI/ML capabilities (OCR, NLP, Vision)
4. **Catalyst DevOps** — [[catalyst-devops|Monitoring]], APM, Logs, Metrics, Testing, GitHub Integration
5. **Catalyst SmartBrowz** — [[smartbrowz|Browser automation]], PDF/Screenshot, Dataverse
6. **Catalyst QuickML** — [[quickml|No-code ML]] pipeline builder
7. **Catalyst ConvoKraft** — [[catalyst-convokraft|AI chatbot]] builder embeddable in Catalyst apps
8. **Catalyst Job Scheduling** — [[job-scheduling-sdk|Job Pools]], Jobs, Cron
9. **Catalyst Slate** — [[catalyst-slate|Frontend deployment]] platform with JS framework support
10. **Catalyst Pipelines** — [[catalyst-pipelines|CI/CD]] solutions for deployment
11. **Catalyst Signals** — [[catalyst-signals|Event Bus]] for event-driven architectures

Additional cross-cutting:
- **Security & Identity** — [[security-rules]], [[api-gateway]]
- **Connectors** — Third-party integrations

## Appearances

- [[catalyst-java-sdk-overview]] — Primary SDK documentation for Java
- [[catalyst-java-sdk-authentication]] — Full authentication SDK operations (11 methods)
- [[catalyst-java-sdk-data-store]] — Full Data Store SDK operations (10 methods)
- [[catalyst-java-sdk-stratus]] — Full Stratus SDK operations (17 sub-pages: buckets, objects, multipart, transfer manager, presigned URLs)
- [[catalyst-serverless-functions]] — Help documentation for all 7 serverless function types
- [[catalyst-java-sdk-cloud-scale-remaining]] — Remaining Cloud Scale + Serverless SDK ops: File Store, ZCQL, Cache, Connections, Search, Mail, Push Notifications, Functions, Circuits, AppSail
- [[catalyst-java-sdk-zia-services]] — All 15 Zia Services SDK ops: OCR, AutoML, Face Analytics, Identity Scanner (5), Text Analytics (4), Image Moderation, Object Recognition, Barcode Scanner
- [[catalyst-java-sdk-smartbrowz]] — All 7 SmartBrowz SDK ops: Browser Grid (6) + PDF & Screenshot (1)
- [[catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors]] — 20 SDK pages: Job Scheduling (15), Pipelines (3), QuickML (1), Connectors (1)
- [[catalyst-java-sdk-nosql-security-apigateway]] — 18 pages filling remaining gaps: NoSQL SDK (10), Security Rules (3), API Gateway (5)
- [[catalyst-nodejs-sdk-overview-serverless]] — Node.js SDK v2: Overview, upgrade, third-party integration, serverless operations
- [[catalyst-nodejs-sdk-cloud-scale-core]] — Node.js SDK v2: Authentication (11), Data Store (11), NoSQL (10), Stratus (19)
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js SDK v2: File Store, Cache, Connections, Search, ZCQL, Mail, Push Notifications
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js SDK v2: Zia (14), SmartBrowz (9), Job Scheduling (17), Pipelines (3), QuickML (1), Connectors (1)
- [[catalyst-general-topics]] — Platform introduction, Quick Start, CLI, Projects, Organizations, Environments, DevOps, ConvoKraft, Signals, Slate

## Relationships

- Part of [[zoho-corporation]] ecosystem
- Java SDK base class: [[zcproject]]
- Node.js SDK package: [[zcatalyst-sdk-node]]

## Notes

- Both Java and Node.js SDKs can be used inside Catalyst-hosted functions (auto-initialized) and in external third-party applications via OAuth.
- Node.js SDK uses promise-based API (`async/await` or `.then()`) vs Java SDK's synchronous bean pattern.
- Node.js SDK v2 covers ~131 pages across 9 sections, comparable to Java SDK v1's ~124 pages.
- Platform supports 4 client SDKs (Web v4, Android v2, iOS v2, Flutter v2) and 3 server languages (Java, Node.js, Python 3.9).
- Max 50 projects per account. Two pricing models: Subscription or Pay-as-you-go.
- Development (free sandbox) and Production (billed per API call) environments per project.
- [[catalyst-cli]] (`zcatalyst-cli`) is the primary developer tool for init, test, deploy lifecycle.
