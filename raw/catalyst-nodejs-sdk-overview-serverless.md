# Catalyst Node.js SDK v2 — Overview & Serverless

Source: https://docs-ea.catalyst.zoho.com/en/sdk/nodejs/v2/
Fetched: 2026-04-07

---

## Overview (1/9)

Node JS SDK has all the necessary methods to access the Catalyst Components and services. It allows you to declare and define Catalyst components whose behavior are predefined. Each Catalyst component has its equivalent Node.js object in SDK, and API equivalents are called methods in Node.js.

### Include Catalyst SDK in Project

```bash
npm install zcatalyst-sdk-node
# Or specific version:
npm install zcatalyst-sdk-node@2.5.0
```

> All versions earlier than 2.5.0, including beta releases, are deprecated.

### Initialize the SDK

**Advanced I/O Functions (Basic Template):**
```js
var catalyst = require('zcatalyst-sdk-node');
module.exports = (req, res) => {
  var app = catalyst.initialize(req);
  // app is used to access Catalyst components
};
```

**Advanced I/O Functions (Express.js):**
```js
var catalyst = require('zcatalyst-sdk-node');
const express = require('express');
const expressApp = express();
expressApp.get('/', (req, res) => {
  var app = catalyst.initialize(req);
});
module.exports = expressApp;
```

**Basic I/O Functions:**
```js
const catalyst = require('zcatalyst-sdk-node');
module.exports = (context, basicIO) => {
  const app = catalyst.initialize(context);
};
```

**Event Functions:**
```js
const catalyst = require('zcatalyst-sdk-node');
module.exports = (event, context) => {
  const app = catalyst.initialize(context);
};
```

**Cron Functions:**
```js
const catalyst = require('zcatalyst-sdk-node');
module.exports = (cronDetails, context) => {
  const app = catalyst.initialize(context);
};
```

### Initialize SDK With Scopes

Two scopes:
- **Admin**: Unrestricted access to all components (default)
- **User**: Restricted access, configurable per component

Scopes only apply to Data Store, File Store, and ZCQL.

```js
// Admin scope
const adminApp = catalyst.initialize(req, { scope: 'admin' });
await adminApp.zcql().executeZCQLQuery('select * from test');

// User scope
const userApp = catalyst.initialize(req, { scope: 'user' });
await userApp.zcql().executeZCQLQuery('select * from test');
```

---

## Upgrade Node.js SDK (2/9)

Two methods to upgrade:
1. `npm update zcatalyst-sdk-node`
2. `npm install zcatalyst-sdk-node` (latest) or `npm install zcatalyst-sdk-node@2.5.1` (specific)

Must apply upgrade to every Node.js function in the project.

---

## Integrate SDK in Third-Party Apps (3/9)

External apps (e.g., React on Vercel, Flask on EC2) can use Catalyst Node.js SDK to access Catalyst components via OAuth.

Prerequisites:
- Project ID (from Settings → General in console)
- ZAID (Zoho Account ID, from Authentication setup)
- Environment: "Development" or "Production"
- OAuth Credentials: Client ID, Client Secret, Refresh Token (from Zoho API Console self-client)

```js
var catalyst = require("zcatalyst-sdk-node");
const express = require("express");
const app = express();

const project_id = "PROJECT_ID";
const project_key = "ZAID";
const environment = "Development";
const credentials = {
  refresh_token: "YOUR_REFRESH_TOKEN",
  client_id: "CLIENT_ID",
  client_secret: "CLIENT_SECRET",
};

const CatalystCred = catalyst.credential.refreshToken(credentials);

app.get("/listbuckets", async (req, res) => {
  req.project_id = project_id;
  req.project_key = project_key;
  req.environment = environment;
  req.credential = CatalystCred;
  let catalystApp = catalyst.initializeApp(req);
  const stratus = catalystApp.stratus();
  const bucket_data = await stratus.listBuckets();
  res.send(bucket_data);
});

app.listen(3006);
```

---

## General — Retrieve Cached Project Data (4/9)

Cache project data as a named app object during initialization and retrieve later:

```js
catalyst.initialize(req, { scope: "user", appName: 'user_app' });
const app = catalyst.app('user_app');
```

---

## Serverless — Functions (5–6/9)

### Get Functions Instance
```js
let functions = app.functions();
```

### Create Configuration (JSON)
```js
let conf = { args: { Name: 'Amelia' } };
```

### Execute Function
Pass function ID (or name as string) and configuration:
```js
let promiseResult = functions.execute(1510000000059262, conf);
promiseResult.then((functionResponse) => {
  console.log(functionResponse);
});
```

---

## Serverless — Circuits (7–8/9)

> Circuits is currently not available in EU, AU, IN, JP, SA or CA data centers.

### Get Circuit Instance
```js
let circuit = app.circuit();
```

### Execute Circuit
```js
circuit.execute('195000000041001', 'sampleName', { name: 'Aaron Jones' })
  .then((result) => console.log(result))
  .catch((err) => console.log(err.toString()));
```

### Get Execution Status
```js
circuit.status('195000000041001', '195000000043002')
  .then((result) => console.log(result));
```

### Abort Circuit
```js
circuit.abort('195000000041001', '195000000043002')
  .then((result) => console.log(result));
```

---

## Serverless — AppSail: Implement Catalyst SDK (9/9)

AppSail supports Catalyst-managed runtimes. SDK implementation for Node.js:

```bash
npm install zcatalyst-sdk-node --save
```

```js
const catalyst = require('zcatalyst-sdk-node');
const express = require('express');
const app = express();
app.get((req, res) => {
  let catalystApp = catalyst.initialize(req);
  // Your code goes here
});
app.listen(process.env("X_ZOHO_CATALYST_LISTEN_PORT") || 9000);
```

Not available for apps deployed as OCI images through custom runtime.
