# Catalyst Java SDK — Remaining Cloud Scale + Serverless + General Operations

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/
> Pages fetched: General (1), Serverless (3: Functions, Circuits, AppSail), Cloud Scale (File Store 5, ZCQL 1, Connections 2, Cache 5, Search 1, Mail 1, Push Notifications 2)
> Date fetched: 2026-04-06

---

## 1. General — Projects

### Retrieve Project Data Cached During Initialization
Catalyst caches project data in the backend as an app object during initialization. Use `getProject()` to retrieve the cached app object at any time.

```java
import com.zc.common.ZCProject;
import com.zc.component.zcql.ZCQL;

ZCProject userProject = ZCProject.getProject("user");
ZCQL.getInstance(userProject).executeQuery("select * from test");
```
- Use `getInstance()` with a custom scope to create component objects with specific access levels.

---

## 2. Serverless — Functions

### Execute Function
Execute a function endpoint by constructing parameters as JSON objects and passing to `executeFunction()`.

```java
import org.json.simple.JSONObject;
import com.zc.functions.ZCatalystFunction;

JSONObject jsonobj = new JSONObject();
jsonobj.put("Name", "Amelia");
Object result = ZCatalystFunction.getInstance()
    .getFunctionInstance(1510000000054095L)
    .executeFunction(jsonobj);
```
- Function ID is an auto-generated numeric long integer
- Can also pass function name as string to `getFunctionInstance()`

---

## 3. Serverless — Circuits

### Execute Circuit
Circuits allow you to define, organize, and orchestrate a sequence of tasks automatically. Sequential or concurrent function executions with conditions, data, and paths.

**Note**: Circuits not available in EU, AU, IN, JP, SA or CA data centers.

```java
import org.json.simple.JSONObject;
import com.zc.component.circuits.ZCCircuit;
import com.zc.component.circuits.ZCCircuitDetails;
import com.zc.component.circuits.ZCCircuitExecutionDetails;
import com.zc.component.circuits.ZCCircuitExecutionStatus;

ZCCircuitDetails userBackupCircuit = ZCCircuit.getInstance().getCircuitInstance(1239000000L);
JSONObject execInputJson = new JSONObject();
execInputJson.put("key", "value");
ZCCircuitExecutionDetails circuitExecution = userBackupCircuit.execute("Case 1", execInputJson);
String executionId = circuitExecution.getExecutionId();

// Get execution details
ZCCircuitExecutionDetails details = userBackupCircuit.getExecutionDetails(executionId);
if (details.getStatus().equals(ZCCircuitExecutionStatus.SUCCESS)) {
    // Success logic
}

// Abort execution
userBackupCircuit.abortExecution(executionId);
```

Key classes: `ZCCircuit`, `ZCCircuitDetails`, `ZCCircuitExecutionDetails`, `ZCCircuitExecutionStatus`

---

## 4. Serverless — AppSail

### Implement SDK in AppSail
AppSail is a fully-managed platform for deploying web services. Supports Java, Node.js, Python runtimes.

**Java Servlet API ≤4** (javax.servlet):
```java
import javax.servlet.http.HttpServletRequest;
import com.zc.auth.AuthHeaderProvider;

public class AuthProviderImpl implements AuthHeaderProvider {
    HttpServletRequest request;
    public AuthProviderImpl(HttpServletRequest request) {
        this.request = request;
    }
    @Override
    public String getHeaderValue(String key) {
        return request.getHeader(key);
    }
}
// Init:
AuthProviderImpl authProviderImpl = new AuthProviderImpl(req);
CatalystSDK.init(authProviderImpl);
```

**Java Servlet API ≥5** (jakarta.servlet):
```java
import jakarta.servlet.http.HttpServletRequest;
import com.zc.auth.AuthHeaderProvider;
// Same pattern but with jakarta import
CatalystSDK.init(new AuthProviderImpl((HttpServletRequest) servletRequest));
```

**Maven dependency**:
```xml
<repository>
    <id>java-sdk</id>
    <url>https://maven.zohodl.com</url>
</repository>
<dependency>
    <groupId>com.zoho.catalyst</groupId>
    <artifactId>java-sdk</artifactId>
    <version>1.15.0</version>
</dependency>
```

---

## 5. Cloud Scale — File Store (5 operations)

**Note**: Catalyst now offers Stratus as a significant upgrade to File Store.

### 5.1 Create a Folder Instance
```java
import com.zc.component.files.ZCFile;
import com.zc.component.files.ZCFolder;

ZCFile fileStore = ZCFile.getInstance();
// By ID:
ZCFolder folder = fileStore.getFolderInstance(1510000000109393L);
// By Name:
ZCFolder folder = fileStore.getFolderInstance("EmpDetails");
```

### 5.2 Get Folder Details
```java
// Specific folder by ID:
ZCFolder folderDetails = fileStore.getFolder(1510000000109393L);
// By Name:
ZCFolder folderDetails = fileStore.getFolder("EmpDetails");
// All folders:
List<ZCFolder> folderDetails = fileStore.getFolder();
```

### 5.3 Upload a File
Max file size: 100 MB. Dev environment: 1 GB storage per project. Production: unlimited.
```java
File f = new File("empdetails.csv");
ZCFile fileStore = ZCFile.getInstance();
ZCFolder folder = fileStore.getFolderInstance(1510000000109393);
folder.uploadFile(f);
```

### 5.4 Download a File
```java
ZCFolder folder = fileStore.getFolderInstance(1510000000109393L);
InputStream is = folder.downloadFile(1510000000108418L);
```

### 5.5 Delete a File
File once deleted cannot be restored.
```java
ZCFolder folder = fileStore.getFolderInstance(704000000116007l);
folder.deleteFile(704000000122001l);
```

Key classes: `ZCFile`, `ZCFolder`

---

## 6. Cloud Scale — ZCQL (1 operation)

### Execute ZCQL Queries
ZCQL is Catalyst's query language for Data Store operations (SELECT, INSERT, UPDATE, DELETE). Also supports OLAP database for analytical queries (SELECT only).

```java
import com.zc.component.object.ZCRowObject;
import com.zc.component.zcql.ZCQL;

String query = "SELECT * from empDetails limit 10";
ArrayList<ZCRowObject> rowList = ZCQL.getInstance().executeQuery(query, true, false);
```

Parameters: `executeQuery(query: string, isV2?: boolean, isOLAP?: boolean)`
- `isV2`: whether it's a ZCQL v2 query
- `isOLAP`: whether to execute on OLAP database

Key classes: `ZCQL`

---

## 7. Cloud Scale — Connections (2 operations)

### 7.1 Get Connections Instance
**Note**: Only accessible within Catalyst Functions and AppSail. Cannot integrate with third-party services.
```java
import com.zc.component.connections.ZCConnections;
ZCConnections connections = ZCConnections.getInstance();
```

### 7.2 Get Authentication Credentials
```java
ZCConnectionResponse connectionResponse = connections.getConnectionCredentials("payrollcon");
System.out.println("Headers: " + connectionResponse.getHeaders());
System.out.println("Parameters: " + connectionResponse.getParameters());
```

Key classes: `ZCConnections`, `ZCConnectionResponse`

---

## 8. Cloud Scale — Cache (5 operations)

### 8.1 Get a Segment Instance
```java
import com.zc.component.cache.ZCCache;
import com.zc.component.cache.ZCSegment;

ZCCache cacheobj = ZCCache.getInstance();
ZCSegment segment = cacheobj.getSegmentInstance(1510000000054091L);
```

### 8.2 Retrieve Data from Cache
```java
// By key (returns String):
String cacheValue = segment.getCacheValue("Val");
// As cache object (includes key, value, expiry):
ZCCacheObject cacheValue = segment.getCacheObject("Name");
```

### 8.3 Insert Data into Cache
Default expiry: 48 hours. If key exists, value is replaced.
```java
// Simple key-value:
ZCCacheObject cache = segment.putCacheValue("Name", "Amelia Burrows");
// With expiry (hours):
ZCCacheObject cache = segment.putCacheValue("LastName", "S", 1L);
// Via cache object:
ZCCacheObject cacheDetails = ZCCacheObject.getInstance();
cacheDetails.setKeyName("ObjectKey");
cacheDetails.setValue("ObjectValue");
cacheDetails.setExpiryInHours(1L);
ZCCacheObject cache = segment.putCacheObject(cacheDetails);
```

### 8.4 Update Data in Cache
```java
// Update value only (keeps existing expiry):
ZCCache.getInstance().updateCacheValue("time_taken", "10");
// Update value and expiry:
ZCCache.getInstance().updateCacheValue("time_taken", "48", 2L);
```

### 8.5 Delete a Key-Value Pair
Cannot be restored once deleted.
```java
// By key:
segment.deleteCacheValue("Name");
// Via cache object:
ZCCacheObject cacheDetails = ZCCacheObject.getInstance();
cacheDetails.setKeyName("ObjectKey");
segment.deleteCacheObject(cacheDetails);
```

Key classes: `ZCCache`, `ZCSegment`, `ZCCacheObject`

---

## 9. Cloud Scale — Search (1 operation)

### Search Data in Indexed Columns
Search for patterns across multiple tables in search-indexed columns.

```java
import com.zc.component.object.ZCRowObject;
import com.zc.component.search.ZCSearch;
import com.zc.component.search.ZCSearchDetails;

ZCSearchDetails search = ZCSearchDetails.getInstance();
search.setSearch("Sa*");
HashMap<String, List> map = new HashMap();
List searchList1 = new ArrayList();
searchList1.add("SearchIndexedColumn");
map.put("SampleTable", searchList1);
search.setSearchTableColumns(map);
ArrayList<ZCRowObject> rowList = ZCSearch.getInstance().executeSearchQuery(search);
```

Key classes: `ZCSearch`, `ZCSearchDetails`

---

## 10. Cloud Scale — Mail (1 operation)

### Send Email
Must configure domains and email addresses in console first.

Limits per operation:
- To: 10 recipients
- CC: 10
- BCC: 5
- Reply to: 5
- Attachments: 5 files, max 15 MB total

```java
import com.zc.component.mail.ZCMail;
import com.zc.component.mail.ZCMailContent;

ZCMailContent mailContent = ZCMailContent.getInstance();
ArrayList toMailList = new ArrayList();
toMailList.add("vanessa.hyde@zoho.com");
mailContent.setFromEmail("p.boyle@zylker.com");
mailContent.setToEmailList(toMailList);
mailContent.setSubject("Greetings from Zylker Corp!");
mailContent.setContent("Hello, welcome...");
ZCMail.getInstance().sendMail(mailContent);
```

Key classes: `ZCMail`, `ZCMailContent`

---

## 11. Cloud Scale — Push Notifications (2 operations)

### 11.1 Send Push Notifications to Web Apps
Send to up to 50 users per call. Message can be plain text, HTML, or JSON.

```java
import com.zc.component.notifications.ZCWebNotification;

Long[] userList = new Long[5];
userList[0] = 1234556789098L;
// ... add more
ZCWebNotification.getInstance().notifyUser("Task completed.", userList);

// Or by email:
String[] emailList = new String[3];
emailList[0] = "emma@zylker.com";
ZCWebNotification.getInstance().notifyUser("Task completed.", emailList);
```

### 11.2 Send Push Notifications to Mobile Apps
Requires registered Android/iOS app with appID. Must use Catalyst Authentication.

```java
import com.zc.component.notifications.ZCMobileNotification;
import com.zc.component.notifications.ZCPush;
import com.zc.component.notifications.ZCPushMessage;

ZCMobileNotification mobile = ZCMobileNotification.getInstance(1234567890l);

// Android:
ZCPushMessage notificationRes = mobile.sendAndroidPushNotification(new ZCPush() {{
    setMessage("Test notification!");
    setBadgeCount(1);
}}, "emma.b@zylker.com");

// iOS:
ZCPushMessage notificationRes = mobile.sendIOSPushNotification(new ZCPush() {{
    setMessage("Test notification!");
    setBadgeCount(1);
}}, "emma.b@zylker.com");
```

Key classes: `ZCWebNotification`, `ZCMobileNotification`, `ZCPush`, `ZCPushMessage`
