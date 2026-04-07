---
title: Catalyst Node.js SDK v2 — Zia, SmartBrowz, Job Scheduling & More
type: source
created: 2026-04-07
updated: 2026-04-07
sources: [catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [nodejs, sdk, v2, zia-services, smartbrowz, job-scheduling, pipelines, quickml, connectors]
---

# Catalyst Node.js SDK v2 — Zia, SmartBrowz, Job Scheduling & More

## Summary

Covers 45 SDK pages across Zia Services (14), SmartBrowz (9), Job Scheduling (17), Pipelines (3), QuickML (1), and Connectors (1). These are the non-Cloud Scale SDK operations that enable AI/ML, browser automation, job orchestration, CI/CD, ML prediction, and external service integration.

## Key Takeaways

### Zia Services (14 pages)
- **Instance**: `app.zia()`
- **OCR**: `zia.extractOpticalCharacters(readStream)` — general text extraction
- **Identity Scanner** (5 pages): Same `extractOpticalCharacters()` with `{ modelType: 'AADHAAR' | 'PAN' | 'PASSBOOK' | 'CHEQUE' }`. India (IN DC) only. Facial Comparison via `zia.compareFaces(stream1, stream2)`.
- **AutoML**: `zia.getRegression(modelId, inputData)`
- **Face Analytics**: `zia.detectFaces(readStream)`
- **Text Analytics** (4 pages): `getSentimentAnalysis(texts, keywords)`, `getNamedEntityRecognition(texts)`, `getKeywordExtraction(texts)`, `getAllTextAnalytics(texts)`. Max 1500 chars.
- **Image Moderation**: `zia.moderateImage(readStream)`
- **Object Recognition**: `zia.detectObject(readStream)`
- **Barcode Scanner**: `zia.scanBarcode(readStream, { format: 'code39' | 'ALL' })`

### SmartBrowz (9 pages)
- **Instance**: `app.smartbrowz()`
- **Browser Grid** (6 pages): `smartbrowz.browserGrid()` → `getAllGrids()`, `getGrid(id)`, `getNodes(gridId)`, `getNode(gridId, nodeId)`, `stopGrid(id)`. All require Admin scope.
- **PDF & Screenshot**: `smartbrowz.generatePDF(options)`, `smartbrowz.generateScreenshot(options)`
- **Dataverse** (3 methods): `getEnrichedLead({leadName, websiteUrl, email})`, `findTechStack(url)`, `getSimilarCompanies({leadName, websiteUrl})`

### Job Scheduling (17 pages)
- **Instance**: `app.jobScheduling()`
- **Job Pools**: `getAllJobpools()`, `getJobpool(nameOrId)`
- **Jobs**: `JOB.submitJob({...})` (supports Function, Circuit, Webhook, AppSail targets), `JOB.getJob(id)`, `JOB.deleteJob(id)`
- **Cron** (10 pages): `CRON.createCron({...})` for OneTime, Recurring, CronExpression types. `CRON.getCronDetails(id)`, `CRON.getAllCronDetails()`, `CRON.updateCron()`, `CRON.pauseCron()`, `CRON.resumeCron()`, `CRON.runCron()`, `CRON.deleteCron()`
- Cron types: OneTime (UNIX timestamp), Recurring (frequency-based), CronExpression (standard cron syntax)

### Pipelines (3 pages)
- **Instance**: `app.pipeline()`
- **Ops**: `getPipelineDetails(id)`, `runPipeline(pipelineId, branchName, envVars)`
- CI/CD automation for building, testing, deploying apps

### QuickML (1 page)
- **Instance**: `app.quickML()`
- **Predict**: `quickml.predict(endpointKey, inputData)` — returns `{ status, result }`
- Not available in JP, SA, CA. Requires pre-configured published endpoint.

### Connectors (1 page)
- `app.connection({ConnectorName: {client_id, client_secret, auth_url, refresh_url, refresh_token, refresh_in}}).getConnector('ConnectorName').getAccessToken()`
- Only Zoho services (not third-party). Auto token refresh via Cache. Unique connector name per user.

## Entities Mentioned

- [[zoho-catalyst]], [[zcatalyst-sdk-node]], [[smartbrowz]], [[zcml]]

## Concepts Introduced

- [[zia-services]], [[identity-scanner]], [[text-analytics]], [[browser-grid]], [[pdf-screenshot]], [[job-scheduling-sdk]], [[catalyst-pipelines]], [[quickml]], [[connectors-external]]

## Open Questions

- Does the Node.js SmartBrowz Dataverse have the same rate limits as the Java SDK version?
- How does Browser Grid concurrency compare between Node.js and Java SDKs?

[Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
