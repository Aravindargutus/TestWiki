# Catalyst Java SDK — Job Scheduling + Pipelines + QuickML + Connectors

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/
> Pages: Job Scheduling (15: Overview, Init, Job Pool 2, Jobs 3, Cron 10) + Pipelines (3) + QuickML (1) + Connectors (1)
> Total: 20 pages
> Date fetched: 2026-04-06

---

## 1. Job Scheduling

Schedule job submissions to execute Functions, Webhooks, Circuits, and AppSail endpoints.

### 1.1 Initialize Instance
```java
import com.zc.component.jobscheduling.ZCJobScheduling;
ZCJobScheduling jobScheduling = ZCJobScheduling.getInstance();
```

### 1.2 Job Pool — Get All
```java
ArrayList<ZCJobpoolDetails> jobpools = jobScheduling.getJobpool();
```

### 1.3 Job Pool — Get Specific
```java
ZCJobpoolDetails pool = jobScheduling.getJobpool("test_jobpool"); // by name
ZCJobpoolDetails pool = jobScheduling.getJobpool(1234567889L); // by ID
```

### 1.4 Jobs — Create Job
Jobs can trigger: Job Functions, Webhooks, Circuits, AppSail endpoints.
```java
ZCJobMetaDetail jobMeta = ZCJobBuilder.functionJobBuilder()
    .setJobConfig(2, 15 * 60L) // 2 retries in 15 min
    .setTargetName("target_function") // or .setTargetId(id)
    .setJobpoolName("test") // or .setJobpoolId(id)
    .setParams(new JSONObject() {{ put("arg1", "job"); }})
    .setJobName("job_name")
    .build();
ZCJobDetails functionJob = jobScheduling.job.submitJob(jobMeta);
```
Also supports: `circuitJobBuilder()`, `webhookJobBuilder()`, `appSailJobBuilder()`.

### 1.5 Jobs — Get Details
```java
ZCJobDetails job = jobScheduling.job.getJob(jobId);
```

### 1.6 Jobs — Delete
```java
ZCJobDetails deletedJob = jobScheduling.job.deleteJob(jobId);
```

### 1.7 Cron — Create One-Time
```java
ZCCronDetails oneTimeCron = ZCCronBuilder.zcOneTimeCronBuilder()
    .setCronStatus(true)
    .cronConfig((System.currentTimeMillis() / 1000) + 3600, "America/Los_Angeles")
    .setJobMeta(jobMeta)
    .setCronName("one_time_cron")
    .setCronDescription("description")
    .build();
ZCCronDetails cron = jobScheduling.cron.createCron(oneTimeCron);
```

### 1.8 Cron — Create Recurring (Every)
Intervals from minutes to hours (< 24h).
```java
ZCCronDetails everyCron = ZCCronBuilder.zcEveryCronBuilder()
    .setCronStatus(true)
    .setTime(2, 1, 3) // 2h 1m 3s
    .setJobMeta(jobMeta)
    .setCronName("every_cron")
    .build();
ZCCronDetails cron = jobScheduling.cron.createCron(everyCron);
```
Also supports: Daily, Monthly, Yearly cron builders.

### 1.9 Cron — Create with Cron Expressions
```java
ZCCronDetails exprCron = ZCCronBuilder.zcExpressionCronBuilder()
    .setCronStatus(true)
    .setCronExpression("0 0 * 1 1") // UNIX cron expression
    .setTimezone("America/Los_Angeles") // optional
    .setCronName("expression_cron")
    .setJobMeta(jobMeta)
    .build();
ZCCronDetails cron = jobScheduling.cron.createCron(exprCron);
```

### 1.10 Cron — Get Particular
```java
ZCCronDetails cron = jobScheduling.cron.getCron(cronId); // by ID
ZCCronDetails cron = jobScheduling.cron.getCron("test_cron"); // by name
```

### 1.11 Cron — Get All
Note: Only fetches Pre-Defined Crons, not Dynamic Crons.
```java
List<ZCCronDetails> allCrons = jobScheduling.cron.getCron();
```

### 1.12 Cron — Update
```java
ZCCronDetails cron = jobScheduling.cron.getCron(cronId);
cron.setCronName("updated_cron");
ZCCronDetails updated = jobScheduling.cron.updateCron(cronId, cron); // by ID
// or by name: jobScheduling.cron.updateCron("cron_name", cron);
```

### 1.13 Cron — Pause/Resume/Run/Delete
```java
jobScheduling.cron.pauseCron(cronId);   // or pauseCron("name")
jobScheduling.cron.resumeCron(cronId);  // or resumeCron("name")
jobScheduling.cron.runCron(cronId);     // or runCron("name")
jobScheduling.cron.deleteCron(cronId);  // or deleteCron("name")
```

Key classes: `ZCJobScheduling`, `ZCJobpoolDetails`, `ZCJobMetaDetail`, `ZCJobBuilder`, `ZCJobDetails`, `ZCCronDetails`, `ZCCronBuilder`

Note: SDK recommended for Dynamic Crons only. Use UI Builder for Pre-defined Crons.

---

## 2. Pipelines

CI/CD automation for building, testing, and deploying web/mobile applications.

### 2.1 Get Instance
```java
import com.zc.component.pipeline.ZCPipeline;
ZCPipeline pipelines_service = ZCPipeline.getInstance();
```

### 2.2 Get Pipeline Details
```java
ZCPipelineDetails details = pipelines_service.getPipelineDetails(pipelineId);
// Returns: name, project_details, created_by, created_time, modified_by, pipeline_status, config_id
```

### 2.3 Execute Pipeline
```java
JSONObject env = new JSONObject();
env.put("EVENT", "push");
env.put("URL", "https://www.google.com");
ZCPipelineRunHistory run = pipelines_service.runPipeline(pipelineId, "main", env);
// Returns: history_id, pipeline_id, event_time, event_details, history_status ("Queued")
```

Key classes: `ZCPipeline`, `ZCPipelineDetails`, `ZCPipelineRunHistory`

---

## 3. QuickML

No-code ML pipeline builder. Build, train, and publish ML models with endpoints.

**Note**: Not available in JP, SA, or CA data centers.

### Execute Endpoint
```java
import com.zc.component.quickml.ZCQuickML;
import com.zc.component.quickml.ZCQuickMLDetail;

HashMap<String, String> map = new HashMap<>();
map.put("column1", "value1");
map.put("column2", "value2");
String endpointKey = "c8c7b4bfd8fdf..."; // from Catalyst console
ZCQuickML quickMl = ZCQuickML.getInstance();
ZCQuickMLDetail result = quickMl.predict(endpointKey, map);
result.getStatus(); // result status
result.getResult(); // result data
```

Key classes: `ZCQuickML`, `ZCQuickMLDetail`

---

## 4. Connectors (External)

Connect Catalyst with external Zoho services via OAuth. Not to be confused with Cloud Scale Connections SDK.

**Note**: Only works with Zoho services (not third-party). Each connector name must be unique. Different users need different connector names (tokens stored in Cache).

```java
import com.zc.auth.connectors.ZCConnection;
import com.zc.auth.connectors.ZCConnector;

JSONObject authJson = new JSONObject();
authJson.put("client_id", "{client_id}");
authJson.put("client_secret", "{client_secret}");
authJson.put("auth_url", "{auth_url}");
authJson.put("refresh_url", "{refresh_url}");
authJson.put("refresh_in", "{refresh_in}");
authJson.put("refresh_token", "{refresh_token}");

JSONObject connectorJson = new JSONObject();
connectorJson.put("CRMConnector", authJson);
ZCConnection conn = ZCConnection.getInstance(connectorJson);
ZCConnector crmConnector = conn.getConnector("CRMConnector");
String accessToken = crmConnector.getAccessToken();
```

Key classes: `ZCConnection`, `ZCConnector`
