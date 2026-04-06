---
title: Zoho Catalyst
type: entity
created: 2026-04-05
updated: 2026-04-06
sources: [catalyst-java-sdk-overview.md, catalyst-java-sdk-authentication.md, catalyst-java-sdk-data-store.md, catalyst-java-sdk-stratus.md, catalyst-serverless-functions.md, catalyst-java-sdk-cloud-scale-remaining.md, catalyst-java-sdk-zia-services.md, catalyst-java-sdk-smartbrowz.md, catalyst-java-sdk-job-scheduling-pipelines-quickml-connectors.md]
tags: [zoho, catalyst, serverless, baas, platform]
---

# Zoho Catalyst

## Overview

Zoho Catalyst is a serverless Backend-as-a-Service (BaaS) platform by Zoho Corporation. It provides cloud-scale components for building microservices, web, and mobile applications without managing infrastructure. [Source: catalyst-java-sdk-overview.md]

## Key Facts

- Provides SDKs for Java (also Node.js and others)
- Java SDK supports Java 8, 11, 17
- Console at: console.catalyst.zoho.com
- API domain: api.catalyst.zoho.com
- Supports development and production environments
- Uses OAuth2 for authentication (via api-console.zoho.com)

## Components

The platform provides these major component categories:
- **General** — Project-level operations
- **Serverless** — Functions, cron jobs
- **Cloud Scale** — [[data-store]], [[file-store]], [[zcql]], [[stratus]], Authentication
- **Zia Services** — AI/ML capabilities
- **SmartBrowz** — Browser automation
- **Job Scheduling** — Scheduled tasks
- **Pipelines** — Data pipelines
- **QuickML** — Machine learning
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

## Relationships

- Part of [[zoho-corporation]] ecosystem
- SDK base class: [[zcproject]]

## Notes

- The Java SDK can be used both inside Catalyst-hosted functions (auto-initialized) and in external third-party applications via OAuth.
