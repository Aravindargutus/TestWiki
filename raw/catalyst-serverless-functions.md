# Catalyst Serverless Functions — Help Documentation

> Source: https://docs.catalyst.zoho.com/en/serverless/help/functions/
> Pages fetched: Introduction, Functions Stack, Basic I/O Functions, Advanced I/O Functions, Event Functions, Cron Functions, Browser Logic Functions, Integration Functions, Job Functions
> Date fetched: 2025-07-10

---

## 1. Introduction

Functions are custom-built serverless code structures that contain the business logic of your application. They are fundamental units of your Catalyst serverless application.

### Supported Languages
- **Java** (versions 8, 11, 17)
- **Node.js** (versions 20, 18, 16, 14, 12 — some EOL)
- **Python** (version 3.9 only)

### Function Types (7 total)
1. **Basic I/O Functions** — Simple JSON input/output
2. **Advanced I/O Functions** — Native HTTP request/response with full framework support
3. **Event Functions** — Async, triggered by Event Listeners
4. **Cron Functions** — Periodic execution via Cron jobs
5. **Browser Logic Functions** — Browser automation via SmartBrowz/Playwright
6. **Integration Functions** — Backend for other Zoho services (Cliq)
7. **Job Functions** — Invoked by Job Scheduling (Function Job Pool)

### Management Tools
- **Security Rules**: Control access to functions
- **API Gateway**: Route and manage function endpoints
- **Logs**: Monitor function execution
- **APM (Application Performance Management)**: Performance monitoring
- **Circuits**: Circuit breaker pattern for fault tolerance

### Function Lifecycle
1. Initialize a Catalyst project (CLI or console)
2. Write function code
3. Test locally (Node shell execution or localhost serve)
4. Deploy to remote console
5. Delete when no longer needed

### Key Notes
- Java and Python functions can only be created via the CLI
- Node.js functions can be created via CLI or console
- Functions are the core compute layer of Catalyst serverless architecture

---

## 2. Functions Stack

### Java
- **Versions**: 8, 11, 17
- **Directory Structure**:
  - Main `.java` file (entry point)
  - `catalyst-config.json` (configuration)
  - `lib/` directory (JAR dependencies)
- **Dependencies**: Added as JAR files in the `lib/` folder

### Node.js
- **Versions**: 20, 18, 16, 14, 12 (12 is EOL)
- **SDK**: `zcatalyst-sdk-node` (install via `npm install zcatalyst-sdk-node`)
- **Directory Structure**:
  - Main `.js` file (entry point)
  - `catalyst-config.json` (configuration)
  - `package.json` (npm manifest)
  - `node_modules/` (dependencies)
  - `package-lock.json`
- **Dependencies**: Managed via npm

### Python
- **Version**: 3.9 only
- **SDK**: `zcatalyst-sdk` (install via `pip install zcatalyst-sdk`)
- **Directory Structure**:
  - `main.py` (entry point)
  - `catalyst-config.json` (configuration)
  - `requirements.txt` (pip dependencies)
- **Dependencies**: Managed via pip/requirements.txt

### catalyst-config.json
Common configuration file across all runtimes. Contains function metadata, type, and runtime settings.

---

## 3. Basic I/O Functions

### Overview
Basic I/O functions accept input as JSON arguments and produce output as JSON. They are the simplest function type — suitable for straightforward request-response operations.

### URL Pattern
```
https://{domain}.catalystserverless.com/server/{function_name}/execute
```
Note: URL ends with `/execute`.

### Timeout
30 seconds (implied maximum execution time).

### Modules

#### context
- `close()` — Terminates function execution
- `log()` — Logs messages (max 1500 characters per log entry)

#### basicIO
- `write()` — Writes output (max 1MB output size)
- `setStatus()` — Sets response status code
- `getArgument(key)` — Gets a specific input argument
- `getAllArguments()` — (Java only) Gets all input arguments
- `getOrigin()` — (Java only) Gets the request origin

### Node.js Example
```javascript
module.exports = (context, basicIO) => {
  const name = basicIO.getArgument("name");
  basicIO.write("Hello " + name);
  context.close();
};
```

### Java Example
```java
public void runner(Context context, BasicIO basicIO) throws Exception {
    String name = basicIO.getArgument("name");
    basicIO.write("Hello " + name);
    context.close();
}
```

### Python Example
```python
def handler(context, basicio):
    name = basicio.get_argument("name")
    basicio.write("Hello " + name)
    context.close()
```

### Key Constraints
- Output limited to 1MB
- Log entries limited to 1500 characters
- Simple JSON in, JSON out — no streaming, no custom headers

---

## 4. Advanced I/O Functions

### Overview
Advanced I/O functions provide native HTTP request/response handling. They support full web frameworks, allowing multiple API endpoints within a single function. They can stream data, render views, download files, and set custom headers.

### URL Pattern
```
https://{domain}.catalystserverless.com/server/{function_name}/
```
Note: URL does NOT end with `/execute` (unlike Basic I/O).

### Timeout
30 seconds maximum execution time.

### Framework Support
- **Node.js**: Express.js (recommended), or blank template
- **Java**: javax.servlet (HttpServlet)
- **Python**: Flask

### Node.js (Express.js) Example
```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello from Advanced I/O!");
});

app.post("/data", (req, res) => {
  const body = req.body;
  res.json({ received: body });
});

module.exports = app;
```

### Java (Servlet) Example
```java
public class AdvancedIOFunction extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) 
        throws ServletException, IOException {
        resp.getWriter().write("Hello from Advanced I/O!");
    }
}
```

### Python (Flask) Example
```python
from flask import Flask, request, jsonify
app = Flask(__name__)

@app.route("/", methods=["GET"])
def hello():
    return "Hello from Advanced I/O!"

@app.route("/data", methods=["POST"])
def data():
    body = request.json
    return jsonify(received=body)
```

### Two Node.js Templates
1. **Express.js template** — Pre-configured with Express routing
2. **Blank template** — Minimal setup for custom frameworks

### Capabilities (vs Basic I/O)
- Multiple API routes in one function
- Custom HTTP headers
- Streaming responses
- File downloads
- View rendering (HTML templates)
- Full middleware support (Express/Flask)
- Path parameters and query strings

---

## 5. Event Functions

### Overview
Event Functions are asynchronous functions triggered by Event Listeners. They cannot be invoked manually and do not have a Function URL. They respond to events from Catalyst components or custom URL invocations.

### Event Listener Types
1. **Default Event Listeners** — Triggered by Catalyst component events (DataStore, Cache, Auth, FileStore)
2. **Custom Event Listeners** — Triggered via URL invocation, GitHub webhooks, or webclient events

### Maximum Execution Time
15 minutes (much longer than I/O functions).

### Modules

#### context
- `closeWithSuccess()` — Terminates with success status
- `closeWithFailure()` — Terminates with failure status
- `getRemainingExecutionTimeMs()` — Returns remaining execution time in milliseconds
- `getMaxExecutionTimeMs()` — Returns maximum execution time (15 minutes)

#### event
- `getAction()` — The action that triggered the event (e.g., "Insert", "Update", "Delete")
- `getSourceEntityId()` — ID of the entity that triggered the event
- `getProjectDetails()` — Project metadata
- `getData()` — The event payload data
- `getTime()` — Timestamp of the event
- `getSource()` — Source component: DataStore, Cache, Auth, FileStore, Github, webclient
- `getEventBusDetails()` — Event bus metadata

### Java Return Values
- `EVENT_STATUS.SUCCESS`
- `EVENT_STATUS.FAILURE`

### Node.js Example
```javascript
module.exports = (event, context) => {
  const action = event.getAction();
  const data = event.getData();
  console.log("Event action:", action, "Data:", data);
  context.closeWithSuccess();
};
```

### Event Sources
- **DataStore** — Row insert, update, delete
- **Cache** — Cache operations
- **Auth** — User signup, login events
- **FileStore** — File upload, delete events
- **Github** — Webhook events (push, PR, etc.)
- **webclient** — Custom client-side events

### Key Constraints
- Cannot be invoked manually
- No Function URL
- Must be associated with an Event Listener
- Asynchronous execution only

---

## 6. Cron Functions

### Overview
Cron Functions are periodic functions associated with Cron jobs. They execute on a schedule — either one-time or recurring. They cannot be invoked manually.

### Schedule Types
- **One-time**: Execute once at a specific date/time
- **Recurring**: Execute at regular intervals (cron expression)

### Maximum Execution Time
15 minutes.

### Modules

#### CronRequest
- `getCronParam(key)` — Gets a specific cron parameter
- `getAllCronParams()` — Gets all cron parameters
- `getRemainingExecutionCount()` — Returns remaining scheduled executions
- `getCronDetails()` — Cron job metadata (schedule, ID, etc.)
- `getProjectDetails()` — Project metadata

#### Context
- `getMaxExecutionTimeMs()` — Returns max execution time (15 minutes)
- `getRemainingExecutionTimeMs()` — Returns remaining execution time

### Java Return Values
- `CRON_STATUS.SUCCESS`
- `CRON_STATUS.FAILURE`

### Node.js Example
```javascript
module.exports = (cronRequest, context) => {
  const param = cronRequest.getCronParam("key");
  console.log("Cron executing with param:", param);
  // Perform scheduled task
  context.closeWithSuccess();
};
```

### Key Constraints
- Cannot be invoked manually
- Must be associated with a Cron job
- No Function URL
- Parameters passed via Cron job configuration

---

## 7. Browser Logic Functions

### Overview
Browser Logic Functions are part of the SmartBrowz service. They enable browser automation tasks like web scraping, PDF generation, screenshot capture, and headless browser testing. They use Playwright under the hood.

### Supported Runtimes
- **Java** and **Node.js** only (Python is NOT supported)

### URL Pattern
Same as Basic I/O:
```
https://{domain}.catalystserverless.com/server/{function_name}/execute
```

### Node.js Function Signature
```javascript
module.exports.playwright = async (request, response, page) => {
  // `page` is a Playwright page object
  await page.goto("https://example.com");
  const title = await page.title();
  response.write(JSON.stringify({ title }));
  response.end();
};
```

### Capabilities
- Web scraping (extract data from websites)
- PDF generation from web pages
- Screenshot capture
- Headless browser testing
- Form filling and submission
- Navigation and interaction automation

### Key Details
- Export must be `module.exports.playwright`
- The `page` parameter is a pre-configured Playwright page object
- SmartBrowz manages the browser instance lifecycle
- Ideal for tasks requiring a full browser context

---

## 8. Integration Functions

### Overview
Integration Functions serve as the backend for other Zoho services. Currently, only **Zoho Cliq** is supported as an integration target. They are NOT accessible via URL — they are invoked internally by the external Zoho service.

### Data Center Availability
**NOT available** in: EU, AU, IN, JP, SA, CA data centers.
(Effectively US-only at time of documentation.)

### Zoho Cliq Integration
Integration Functions include the Cliq SDK with handler classes for:
- **Bots** — Automated chatbot handlers
- **Commands** — Slash command handlers
- **Message Actions** — Context menu actions on messages
- **Widgets** — UI widget handlers
- **Functions** — Custom function handlers for Cliq

### Key Constraints
- No Function URL
- Cannot be invoked manually or via HTTP
- Invoked only by the integrated Zoho service (Cliq)
- Limited data center availability
- Tightly coupled to the external service's SDK

---

## 9. Job Functions

### Overview
Job Functions are invoked by the Job Scheduling service (Function Job Pool). They are non-HTTPS functions without a URL, designed for long-running background tasks managed by job pools.

### Supported Runtimes
- **Java**: 7, 11, 17 (note: Java 7 is available here but not for other function types)
- **Node.js**: 18
- **Python**: 3.9

### Maximum Execution Time
15 minutes.

### Modules

#### JobRequest
- `getProjectDetails()` — Project metadata
- `getJobDetails()` — Job-specific details
- `getJobMetaDetails()` — Job metadata
- `getJobpoolDetails()` — Job pool configuration
- `getJobCapacityAttributes()` — Memory/CPU capacity info
- `getAllJobParams()` — All job parameters
- `getJobParam(key)` — Specific job parameter

#### Context
- `getRemainingExecutionTimeMs()` — Remaining execution time
- `getMaxExecutionTimeMs()` — Max execution time (15 minutes)

### Java Return Values
- `JOB_STATUS.SUCCESS`
- `JOB_STATUS.FAILURE`

### Node.js Example
```javascript
module.exports = (jobRequest, context) => {
  const param = jobRequest.getJobParam("key");
  const jobDetails = jobRequest.getJobDetails();
  console.log("Job executing:", jobDetails);
  // Perform long-running task
  context.closeWithSuccess();
};
```

### Key Constraints
- Non-HTTPS, no Function URL
- Invoked only by Job Scheduling service
- Function memory must be less than job pool memory
- Cannot be invoked manually
- Designed for background batch processing

---

## Cross-Cutting Summary

### Function Type Comparison

| Feature | Basic I/O | Advanced I/O | Event | Cron | Browser Logic | Integration | Job |
|---------|-----------|-------------|-------|------|---------------|-------------|-----|
| Has URL | Yes (/execute) | Yes (/) | No | No | Yes (/execute) | No | No |
| Manual Invoke | Yes | Yes | No | No | Yes | No | No |
| Max Timeout | 30s | 30s | 15min | 15min | 30s (implied) | — | 15min |
| Java | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Node.js | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Python | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✓ |
| Framework | None | Express/Servlet/Flask | None | None | Playwright | Cliq SDK | None |
| Trigger | HTTP | HTTP | Event Listener | Cron Job | HTTP | Zoho Service | Job Pool |

### URL-Accessible Functions
Only 3 of 7 function types have URLs:
1. Basic I/O: `https://{domain}.catalystserverless.com/server/{function_name}/execute`
2. Advanced I/O: `https://{domain}.catalystserverless.com/server/{function_name}/`
3. Browser Logic: `https://{domain}.catalystserverless.com/server/{function_name}/execute`

### Execution Time Tiers
- **30 seconds**: Basic I/O, Advanced I/O, Browser Logic (HTTP-facing)
- **15 minutes**: Event, Cron, Job (background/async)

### Runtime Availability
- All 7 types support Java and Node.js
- 5 of 7 types support Python (except Browser Logic and — effectively — Integration for non-US)
