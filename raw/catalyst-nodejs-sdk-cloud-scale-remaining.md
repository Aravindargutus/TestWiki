# Catalyst Node.js SDK v2 — Cloud Scale Remaining (File Store, Cache, Connections, Search, ZCQL, Mail, Push Notifications)

Source: https://docs-ea.catalyst.zoho.com/en/sdk/nodejs/v2/cloud-scale/
Fetched: 2026-04-07

---

## File Store (6 pages)

> Note: Stratus is a significant upgrade to File Store, available in Early Access.

### Get File Store Instance
```js
let filestore = app.filestore();
```

### Get Folder Instance
```js
let folder = filestore.folder(folderId);
```

### Retrieve Folder Details
```js
folder.getDetails().then(details => console.log(details));
```

### Upload File
```js
let uploadPromise = folder.uploadFile({ code: fileStream, name: 'filename.ext' });
uploadPromise.then(result => console.log(result));
```

### Download File from Folder
```js
let downloadPromise = folder.downloadFile(fileId);
downloadPromise.then(stream => { /* handle stream */ });
```

### Delete a File
```js
let deletePromise = folder.deleteFile(fileId);
deletePromise.then(result => console.log(result));
```

---

## Cache (6 pages)

### Get Component Instance
```js
let cache = app.cache();
```

### Get Segment Instance
```js
let segment = cache.segment(); // default segment
let segment = cache.segment(segmentId); // specific segment
```

### Retrieve Data from Cache
```js
let getPromise = segment.getValue('keyName');
getPromise.then(value => console.log(value));
```

### Insert Data into Cache
```js
let putPromise = segment.put('keyName', 'value');
putPromise.then(result => console.log(result));
```

### Update Data in Cache
```js
let updatePromise = segment.update('keyName', 'newValue');
updatePromise.then(result => console.log(result));
```

### Delete Key-Value Pair
```js
let deletePromise = segment.delete('Name');
deletePromise.then(entity => console.log(entity));
```

---

## Connections (2 pages)

> This SDK can only be accessed within Catalyst services like Functions and AppSail. Cannot integrate with third-party services.

### Get Connections Instance
```js
const connections = app.connections();
```

### Get Authentication Credentials
```js
const credential = await connections.getConnector('ConnectorName');
const token = await credential.getAccessToken();
```

---

## Search (2 pages)

### Get Search Instance
```js
let search = app.search();
```

### Search Data
Create config and execute:
```js
let config = {
  search: 'santh*',
  search_table_columns: {
    SampleTable: ['SearchIndexedColumn'],
    Users: ['SearchTest']
  }
};
let searchPromise = search.executeSearchQuery(config);
searchPromise.then(searchResult => console.log(searchResult));
```

Response: JSON object with table names as keys, arrays of matching rows as values.

---

## ZCQL (2 pages)

ZCQL is Catalyst's own query language for Data Store operations. Supports DML queries, SQL Join, GroupBy, OrderBy, and built-in SQL functions. Also supports OLAP database for analytical queries.

### Get ZCQL Instance
```js
let zcql = app.zcql();
```

### Execute Query
```js
let queryPromise = zcql.executeZCQLQuery('SELECT * FROM SampleTable WHERE Name = "Amelia"');
queryPromise.then(result => console.log(result));
```

---

## Mail (2 pages)

### Get Mail Instance
```js
let email = app.email();
```

### Send Email
```js
let mailConfig = {
  from_email: 'noreply@example.com',
  to_email: ['user@example.com'],
  subject: 'Test Email',
  content: '<p>Hello!</p>'
};
let sendPromise = email.sendMail(mailConfig);
sendPromise.then(result => console.log(result));
```

Supports: public domains, organization domains, external SMTP clients.

---

## Push Notifications (3 pages)

### Get Push Notifications Instance
```js
const pushNotification = app.pushNotification();
```

### Send Notifications to Web Apps
```js
pushNotification.sendWebNotification({
  message: 'Test notification!',
  recipients: [{ recipient_id: userId, target_url: 'https://app.example.com' }]
}).then(result => console.log(result));
```

Prerequisites: Enable push notifications in web app using web initialization script, user must allow notifications.

### Send Notifications to Mobile Apps

**Get Mobile Notification Instance:**
```js
const notification = app.pushNotification().mobile("appId");
```

**Send Android Push:**
```js
notification.sendAndroidNotification({
  message: 'Test notification!',
  badge_count: 1
}, 'emma.b@zylker.com');
```

**Send iOS Push:**
```js
notification.sendIOSNotification({
  message: 'Test notification!',
  badge_count: 1
}, 'emma@zylker.com');
```

Prerequisites: Register mobile app with Catalyst, configure platform-specific push services, user must be logged in via Catalyst Authentication.
