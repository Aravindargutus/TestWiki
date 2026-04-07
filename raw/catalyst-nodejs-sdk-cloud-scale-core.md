# Catalyst Node.js SDK v2 — Cloud Scale Core (Authentication, Data Store, NoSQL, Stratus)

Source: https://docs-ea.catalyst.zoho.com/en/sdk/nodejs/v2/cloud-scale/
Fetched: 2026-04-07

---

## Authentication (11 pages)

### Get Authentication Instance
```js
let userManagement = app.userManagement();
```

### Add New User
Create a signup config and user config, then register:
```js
const signupConfig = {
  platform_type: 'web',
  template_details: {
    senders_mail: 'noreply@example.com',
    subject: 'Welcome to %APP_NAME%',
    message: '<p>Hello,</p><p>Follow this link to join %APP_NAME%.</p><p><a href="%LINK%">%LINK%</a></p>'
  },
  redirect_url: 'home.html'
};
var userConfig = {
  first_name: 'Dannie',
  last_name: 'Boyle',
  email_id: 'p.boyle@zylker.com',
  role_id: '3376000000159024'
};

let registerPromise = userManagement.registerUser(signupConfig, userConfig);
registerPromise.then(userDetails => console.log(userDetails));
```
Response includes: zaid, user_details (zuid, zaaid, org_id, status, email_id, first_name, last_name, role_details, user_type, user_id), redirect_url, platform_type.

> Dev environment limit: 25 users. Production: unlimited.

### Get All Org IDs
```js
userManagement.getAllOrganizations().then(orgs => console.log(orgs));
```

### Add User to Existing Org
```js
userManagement.addUserToOrg(orgId, signupConfig, userConfig).then(result => console.log(result));
```

### Get All Users in Organization
```js
userManagement.getAllUsersOfOrg(orgId).then(users => console.log(users));
```

### Reset Password
```js
userManagement.resetPassword(signupConfig, userConfig).then(result => console.log(result));
```

### Generate Custom Server Token
```js
userManagement.getServerToken().then(token => console.log(token));
```

### Custom User Validation
```js
userManagement.validateUser(userConfig).then(result => console.log(result));
```

### Get User Details

**Current user:**
```js
let userPromise = userManagement.getCurrentUser();
userPromise.then(currentUser => console.log(currentUser));
```

**By User ID:**
```js
let userPromise = userManagement.getUserDetails(1510000000109587);
userPromise.then(userDetails => console.log(userDetails));
```

**All users:**
```js
let allUserPromise = userManagement.getAllUsers();
allUserPromise.then(allUserDetails => console.log(allUserDetails));
```

Response fields: zuid, zaaid, org_id, status, is_confirmed, email_id, last_name, created_time, role_details (role_name, role_id), user_type, user_id, locale, time_zone, project_profiles.

### Update User Details
```js
userManagement.updateUser(userId, updateConfig).then(result => console.log(result));
```

### Enable or Disable a User
```js
userManagement.enableUser(userId).then(result => console.log(result));
userManagement.disableUser(userId).then(result => console.log(result));
```

### Delete a User
```js
let deleteUserPromise = userManagement.deleteUser(1510000000109587);
deleteUserPromise.then(deletedUser => console.log(deletedUser));
```

---

## Data Store (11 pages)

### Get Data Store Instance
```js
let datastore = app.datastore();
```

### Get Table Metadata
```js
datastore.getAllTables().then(tables => console.log(tables));
```

### Get Table Instance
```js
let table = datastore.table('SampleTable'); // by name
let table = datastore.table(1510000000004507); // by ID
```

### Get Column Metadata
```js
table.getAllColumns().then(columns => console.log(columns));
```

### Get Rows
```js
// Get all rows with pagination
let rows = await table.getPagedRows({ maxRows: 200, nextToken: 'token' });

// Get a single row by ROWID
let row = await table.getRow(1510000000008508);
```

### Insert Rows
```js
let rowData = [{ Name: 'Amelia', Age: 25 }];
let insertPromise = table.insertRows(rowData);
insertPromise.then(result => console.log(result));
```

### Update Rows
```js
let rowData = [{ ROWID: '1510000000008508', Name: 'Updated' }];
let updatePromise = table.updateRows(rowData);
updatePromise.then(result => console.log(result));
```

### Delete Row
```js
let deletePromise = table.deleteRow(1510000000008508);
deletePromise.then(result => console.log(result));
```

### Bulk Read Rows
```js
let bulkReadJob = await datastore.bulkRead({
  table_name: 'SampleTable',
  // optional: criteria, columns
});
// Returns job with job_id, status
```

### Bulk Write Rows
```js
let bulkWriteJob = await datastore.bulkWrite({
  table_name: 'SampleTable',
  file_id: fileId // CSV file uploaded to File Store
});
```

### Bulk Delete Rows
```js
let bulkDeleteJob = await datastore.bulkDelete({
  table_name: 'SampleTable',
  ids: [rowid1, rowid2]
});
```

---

## NoSQL (10 pages)

Catalyst NoSQL is a fully managed non-relational, document-type data storage (key-value JSON). Supports partition key + sort key, conditional CRUD, master-slave consistency.

### Get Component Instance
```js
const nosql = app.nosql();
```

### Get Table Metadata
```js
const tableMetadata = await nosql.getTableDetails('TableName');
```

### Get Table Instance
```js
const table = nosql.table('TableName');
```

### Construct NoSQL Item
Create items with supported data types (String, Number, Boolean, List, Map, Set, Null, Binary):
```js
const item = nosql.item();
item.put('pk', 'partition_value');
item.put('sk', 'sort_value');
item.put('name', 'John');
item.put('age', 30);
```

### Insert Items
```js
const result = await table.insertItems([item1, item2]);
```

### Update Items
```js
const result = await table.updateItems([updatedItem]);
```

### Fetch Items
```js
const items = await table.fetchItems({
  partition_key_value: 'pk_value',
  sort_key_value: 'sk_value'
});
```

### Query Table
```js
const result = await table.queryTable({
  partition_key: { value: 'pk_value' },
  sort_key: { value: 'sk_value', condition: 'BEGINS_WITH' }
});
```

### Query Index
```js
const result = await table.queryIndex('IndexName', {
  partition_key: { value: 'pk_value' }
});
```

### Delete Items
```js
const result = await table.deleteItems([{
  partition_key_value: 'pk_value',
  sort_key_value: 'sk_value'
}]);
```

---

## Stratus (19 pages)

Catalyst Stratus is the cloud-scale object storage service. The Node.js SDK provides comprehensive bucket and object operations.

### Create Stratus Instance
```js
const stratus = app.stratus();
```

### Check Bucket Availability
```js
const isAvailable = await stratus.checkBucketAvailability('bucket-name');
```

### List Buckets
```js
const buckets = await stratus.listBuckets();
```

### Create Bucket Instance
```js
const bucket = stratus.bucket('bucket-name');
```

### Get Bucket Details
```js
const details = await bucket.getDetails();
```

### Get Bucket CORS
```js
const cors = await bucket.getCORS();
```

### List Objects in Bucket
```js
const objects = await bucket.listObjects();
```

### Check Object Availability
```js
const isAvailable = await bucket.checkObjectAvailability('object-key');
```

### Download Object
```js
const stream = await bucket.downloadObject('object-key');
```

### Upload Object
```js
const result = await bucket.uploadObject('object-key', fileStream, options);
```

### Extract Zipped Object
```js
const result = await bucket.extractObject('archive.zip', 'destination/');
```

### Copy Object
```js
const result = await bucket.copyObject('source-key', 'dest-bucket', 'dest-key');
```

### Rename and Move Object
```js
const result = await bucket.renameObject('old-key', 'new-key');
const result = await bucket.moveObject('source-key', 'dest-bucket', 'dest-key');
```

### Delete Objects
```js
const result = await bucket.deleteObjects(['key1', 'key2']);
```

### Create Object Instance
```js
const objectIns = bucket.object('object-key');
```

### List Object Versions
```js
const versions = await objectIns.listVersions();
```

### Get Object Details
```js
const details = await objectIns.getDetails();
```

### Put Object Metadata
Requires Admin scope initialization.
```js
const objectMeta = { "key1": "value1", "key2": "value2" };
const objMeta = await objectIns.putMeta(objectMeta);
```

Notes:
- Passing new meta without existing meta deletes old meta
- Alphanumeric, underscores, whitespace, hyphens only
- Max metadata size: 2047 characters including colons
- Fetch metadata via HEAD request (x-user-meta header)
