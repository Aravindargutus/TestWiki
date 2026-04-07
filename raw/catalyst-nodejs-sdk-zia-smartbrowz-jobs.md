# Catalyst Node.js SDK v2 — Zia Services, SmartBrowz, Job Scheduling, Pipelines, QuickML, Connectors

Source: https://docs-ea.catalyst.zoho.com/en/sdk/nodejs/v2/
Fetched: 2026-04-07

---

## Zia Services (14 pages)

### Get Zia Instance
```js
let zia = app.zia();
```

### OCR (Optical Character Recognition)
```js
let fs = require('fs');
zia.extractOpticalCharacters(fs.createReadStream('./document.png'))
  .then(result => console.log(result))
  .catch(err => console.log(err.toString()));
```

### AutoML
```js
zia.getRegression(modelId, { feature1: 'value1', feature2: 'value2' })
  .then(result => console.log(result));
```

### Face Analytics
```js
zia.detectFaces(fs.createReadStream('./photo.jpg'))
  .then(result => console.log(result));
```

### Identity Scanner

> Document Processing is only relevant to Indian users (IN DC only).

**Facial Comparison:**
```js
zia.compareFaces(fs.createReadStream('./face1.jpg'), fs.createReadStream('./face2.jpg'))
  .then(result => console.log(result));
```

**Aadhaar Scanner:**
```js
zia.extractOpticalCharacters(fs.createReadStream('./aadhaar.jpg'), { modelType: 'AADHAAR' })
  .then(result => console.log(result));
```

**PAN Scanner:**
```js
zia.extractOpticalCharacters(fs.createReadStream('./pan.jpg'), { modelType: 'PAN' })
  .then(result => console.log(result));
```

**Passbook Scanner:**
```js
zia.extractOpticalCharacters(fs.createReadStream('./passbook.jpg'), { modelType: 'PASSBOOK' })
  .then(result => console.log(result));
```

**Cheque Scanner:**
```js
zia.extractOpticalCharacters(fs.createReadStream('./cheque.webp'), { modelType: 'CHEQUE' })
  .then(result => console.log(result));
```
- Only CTS-2010 format, English only
- Allowed: .webp, .jpeg, .png (max 15 MB)
- Response: date, account_number, amount, branch_name, bank_name, ifsc

### Text Analytics

**Sentiment Analysis:**
```js
zia.getSentimentAnalysis(['Text to analyze...'], ['optional', 'keywords'])
  .then(result => console.log(result));
```
Response: document_sentiment, sentence_analytics (per-sentence sentiment + confidence scores), overall_score. Max 1500 characters.

**Named Entity Recognition:**
```js
zia.getNamedEntityRecognition(['Text with entities...'])
  .then(result => console.log(result));
```

**Keyword Extraction:**
```js
zia.getKeywordExtraction(['Text to extract keywords from...'])
  .then(result => console.log(result));
```

**All Text Analytics (combined):**
```js
zia.getAllTextAnalytics(['Text...'])
  .then(result => console.log(result));
```

### Image Moderation
```js
zia.moderateImage(fs.createReadStream('./image.jpg'))
  .then(result => console.log(result));
```

### Object Recognition
```js
zia.detectObject(fs.createReadStream('./image.jpg'))
  .then(result => console.log(result));
```

### Barcode Scanner
Supported formats: Codabar, EAN-13, ITF, UPC-A, QR Code, and more.
```js
zia.scanBarcode(fs.createReadStream('./barcode.png'), { format: 'code39' })
  .then(result => console.log(result))
  .catch(err => console.log(err.toString()));
```
Input: .jpg/.jpeg or .png. Use `format: 'ALL'` for auto-detect.

---

## SmartBrowz (9 pages)

### Create SmartBrowz Instance
```js
const smartbrowz = app.smartbrowz();
```

### Browser Grid (6 pages)

Overview: Browser Grid is an auto-scaling component for managing multiple headless browsers.

**Get Browser Grid Instance:**
```js
const browserGrid = smartbrowz.browserGrid();
```

SDK Operations (all require Admin scope):
- `browserGrid.getAllGrids()` — Get all browser grid details
- `browserGrid.getGrid(gridId)` / `browserGrid.getGrid('gridName')` — Get specific grid
- `browserGrid.getNodes(gridId)` — Get nodes of a grid
- `browserGrid.getNode(gridId, nodeId)` — Get details of a specific node
- `browserGrid.stopGrid(gridId)` / `browserGrid.stopGrid('gridName')` — Stop grid

### PDF & Screenshot
```js
const result = await smartbrowz.generatePDF(options);
const result = await smartbrowz.generateScreenshot(options);
```

### Dataverse

**Lead Enrichment:**
```js
const response = await smartbrowz.getEnrichedLead({
  leadName: 'zoho',
  websiteUrl: 'https://www.zoho.com',
  email: 'sales@zohocorp.com'
});
```
Returns: employee_count, website, address, social, description, organization_name, ceo, revenue, founding_year, industries, organization_type, business_model, etc.

**Tech Stack Finder:**
```js
const response = await smartbrowz.findTechStack('https://www.zoho.com');
```
Returns: website, technographic_data, organization_name.

**Similar Companies:**
```js
const response = await smartbrowz.getSimilarCompanies({
  leadName: 'zoho',
  websiteUrl: 'https://www.zoho.com'
});
```
Returns: Array of company names.

> Any browser automation or web scraping is at your own risk. Use on domains that permit the actions.

---

## Job Scheduling (17 pages)

### Overview
Schedule job submissions to trigger Circuits, Webhooks, Job Functions, and AppSail endpoints.

### Initialize Job Scheduling Instance
```js
const jobScheduling = app.jobScheduling();
```

### Job Pool (2 pages)

**Get All Job Pools:**
```js
const allJobpools = await jobScheduling.getAllJobpools();
```

**Get Specific Job Pool:**
```js
const jobpool = await jobScheduling.getJobpool('test'); // by name
const jobpool = await jobScheduling.getJobpool('123456789'); // by ID
```

### Jobs (3 pages)

**Create Job (Function):**
```js
const functionJob = await jobScheduling.JOB.submitJob({
  job_name: 'test_job',
  jobpool_name: 'test',
  target_type: 'Function',
  target_name: 'target_function',
  params: { arg1: 'test', arg2: 'job' },
  job_config: { number_of_retries: 2, retry_interval: 15 * 60 }
});
```
Also supports: Circuit, Webhook, AppSail target types.

**Get Job Details:**
```js
const jobDetails = await jobScheduling.JOB.getJob('jobId');
```

**Delete Job:**
```js
const deleted = await jobScheduling.JOB.deleteJob('jobId');
```

### Cron (10 pages)

**Create One-Time Cron:**
```js
const oneTimeCron = {
  cron_name: 'one_time',
  description: 'one_time_cron',
  cron_status: true,
  cron_type: 'OneTime',
  cron_detail: {
    time_of_execution: Math.floor(Date.now() / 1000) + (60 * 60) + ''
  },
  job_meta: jobMeta
};
const cronDetails = await jobScheduling.CRON.createCron(oneTimeCron);
```

**Create Recurring Cron:**
```js
const recurringCron = {
  cron_name: 'recurring',
  cron_type: 'Recurring',
  cron_detail: { frequency: 'hourly', every: 2 },
  job_meta: jobMeta
};
const cronDetails = await jobScheduling.CRON.createCron(recurringCron);
```

**Create Cron Using Cron Expressions:**
```js
const cronExpr = {
  cron_name: 'expr_cron',
  cron_type: 'CronExpression',
  cron_detail: { cron_expression: '0 0/5 * * * ?' },
  job_meta: jobMeta
};
const cronDetails = await jobScheduling.CRON.createCron(cronExpr);
```

**Other Cron Operations:**
```js
// Get details
const details = await jobScheduling.CRON.getCronDetails('cronId');
const allCrons = await jobScheduling.CRON.getAllCronDetails();

// Manage
await jobScheduling.CRON.updateCron('cronId', updatedConfig);
await jobScheduling.CRON.pauseCron('cronId');
await jobScheduling.CRON.resumeCron('cronId');
await jobScheduling.CRON.runCron('cronId');

// Delete
await jobScheduling.CRON.deleteCron('test_cron'); // by name
await jobScheduling.CRON.deleteCron('1234567890'); // by ID
```

> Use SDK for Dynamic Crons. Use UI Builder for Pre-defined Crons.

---

## Pipelines (3 pages)

### Get Pipeline Instance
```js
const pipelines_service = app.pipeline();
```

### Get Pipeline Details
```js
const details = await pipelines_service.getPipelineDetails('pipelineId');
```

### Execute Pipeline
```js
let execution_details = pipelines_service.runPipeline(
  "8431000000162051", // pipeline ID
  "main", // branch name
  { "EVENT": "push", "URL": "https://www.google.com" } // optional env vars
);
```
Response: history_id, pipeline_id, event_time, event_details, history_status ("Queued").

---

## QuickML (1 page)

No-code ML pipeline builder. Execute published endpoints:

```js
const input_data = {
  column_name1: "value1",
  column_name2: "value2",
  column_name3: "value3"
};
const quickml = app.quickML();
const result = await quickml.predict("{endpoint_key}", input_data);
console.log(result);
```

Response: `{ status: 'success', result: ["results..."] }`

> Not available in JP, SA, or CA data centers.
> Must have ML pipeline published in Catalyst console first.

---

## Connectors (1 page)

Connects Catalyst to external Zoho services via Zoho OAuth. Handles automatic token refresh via Cache.

```js
var connector = app.connection({
  ConnectorName: {
    client_id: '{add_client_id}',
    client_secret: '{add_client_secret}',
    auth_url: '{add_auth_url}',
    refresh_url: '{add_refresh_url}',
    refresh_token: '{add_refresh_token}',
    refresh_in: '{add_refresh_in}'
  }
}).getConnector('{ConnectorName}');

connector.getAccessToken().then((accessToken) => {
  // Use accessToken for external Zoho API calls
});
```

Notes:
- Only works with Zoho services (not third-party)
- Each connector name must be unique
- Different users in same app need unique connector names (tokens stored in Cache segments, same connector overwrites tokens)
- Register app in Zoho API Console first, generate Authorization Code + Access Token
