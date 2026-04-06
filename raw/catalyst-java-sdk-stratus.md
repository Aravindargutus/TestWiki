# Catalyst Java SDK — Stratus (Object Storage)

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/cloud-scale/stratus/
> Fetched: 2026-04-05
> Sub-pages: 17

---

## Overview

Cloud Scale Stratus is Catalyst's robust and powerful storage solution. You can store data of any format in the form of Objects in containers called Buckets. Each Bucket and every individual object in the bucket has a secure Object URL and Bucket URL. You can perform upload and download operations on objects and even provide custom permissions for each object.

### Operation Categories

| Category | Operations |
|----------|-----------|
| General Stratus Operations | Create Stratus Instance, Check Bucket Availability, List Buckets |
| Bucket Operations | Create Bucket Instance, Get Bucket Details, Get Bucket CORS, List Objects (Paginated + Iterable), Check Object Availability, Download Object (full/partial/Transfer Manager/Presigned URL), Upload Object (stream/string/options/zip extract/multipart/Transfer Manager/Presigned URL), Extract Zipped Object, Copy Object, Rename/Move Object, Delete Objects (single/multiple/truncate/path) |
| Object Operations | Create Object Instance, List Versions (Paginated + Iterable), Get Object Details (latest/specific version), Put Object Meta Data |

---

## General Stratus Operations

### Create Stratus Instance

Gets the stratus component reference. Does NOT fire a server-side call. Used as a base reference in all Stratus operations.

**Package:** `com.zc.component.stratus`

```java
import com.zc.component.stratus.ZCStratus;

ZCStratus stratus = ZCStratus.getInstance();
```

### Check Bucket Availability

Uses `headBucket()` to check if a bucket exists and if the user has permissions to access it.

**Parameters:**
| Name | Type | Description |
|------|------|-------------|
| bucket_name | String | Mandatory. Unique name of the bucket |
| throwErr | Boolean | Optional. If true, throws error instead of returning false. Default: false |

**Returns:** `Boolean` — true if bucket exists and user has access; false otherwise.

```java
Boolean throwErr = false;
Boolean res = stratus.headBucket("bucket_name", throwErr);
System.out.println(res);
```

**Possible Errors (when throwErr=true):**
| Code | Description |
|------|-------------|
| 404 | Not Found. Bucket Not found in Stratus. |
| 401 | Unauthorized/Access Denied |
| 403 | Permission Denied |

### List Buckets

Returns all buckets in the project.

**Scope:** Admin scope required.

```java
import java.util.List;
import com.zc.component.stratus.ZCStratus;
import com.zc.component.stratus.ZCBucket;

ZCStratus stratus = ZCStratus.getInstance();
List<ZCBucket> buckets = stratus.listBuckets();
```

---

## Bucket Operations

### Create Bucket Instance

Creates a bucket reference for performing bucket-level operations. Does NOT fire a server-side call.

**Package:** `com.zc.component.stratus.ZCBucket`

```java
import com.zc.component.stratus.ZCBucket;

ZCBucket bucket = stratus.bucketInstance("bucketName");
```

### Get Bucket Details

Returns details of a single bucket.

```java
import com.zc.component.stratus.ZCBucket;

ZCBucket bucketDetails = bucket.getDetails();
```

### Get Bucket CORS

Returns current CORS configuration of a bucket.

**Scope:** Admin scope required. Requires Write permission for Stratus.

```java
import com.zc.component.stratus.beans.ZCStratusCorsResponse;
import java.util.List;

List<ZCStratusCorsResponse> res = bucket.getCors();
for (ZCStratusCorsResponse cors : res) {
    System.out.println(cors.getDomain());
}
```

### List Objects in a Bucket

#### List All Objects by Pagination

Uses `listPagedObjects()` with continuation token pattern.

**Parameters:**
| Name | Type | Description |
|------|------|-------------|
| maxKey | String | Mandatory. Maximum number of objects per page. Default: 1000 |
| nextToken | String | Mandatory. Continuation token for next page |
| prefix | String | Optional. Filter objects by prefix |
| orderBy | String | Optional. "asc" (default) or "desc" |
| folderListing | String | Optional. Default: "false" |

**Scope:** Admin scope required.

```java
import com.zc.component.stratus.ZCBucket;
import com.zc.component.stratus.ZCStratus;
import com.zc.component.stratus.beans.ZCListObjectOptions;
import com.zc.component.stratus.beans.ZCPagedObjectResponse;
import com.zc.component.stratus.ZCObject;

String nextToken = null;
String maxKey = "10";
String prefix = "Sam";
do {
    ZCListObjectOptions options = new ZCListObjectOptions();
    options.setMaxKey(maxKey);
    options.setContinuationToken(nextToken);
    options.setFolderListing("true");
    options.setOrderBy("desc");
    options.setPrefix(prefix);
    ZCPagedObjectResponse res = bucket.listPagedObjects(options);
    System.out.println("Object count: " + res.getKeyCount());
    System.out.println("Max key: " + res.getMaxKey());
    System.out.println("Is truncated: " + res.getTruncated());
    for (ZCObject key : res.getContents()) {
        System.out.println("Object name: " + key.getKey());
        System.out.println("Content type: " + key.getContentType());
        System.out.println("Size: " + key.getSize());
        System.out.println("Metadata: " + key.getMetaData());
        System.out.println("Version ID: " + key.getVersionId());
        System.out.println("ETag: " + key.getEtag());
        System.out.println("Object type: " + key.getKeyType());
        System.out.println("Cached URL: " + key.getCachedUrl());
    }
    nextToken = res.getNextToken();
} while (nextToken != null);
```

**Response Properties:**
- key count, max keys, Truncated, contents (list of object details)
- continuation_token, next_continuation_token

#### List Objects Through Iteration

Single API call using iterable technique.

**Scope:** Admin scope required.

```java
import java.util.Iterator;
import com.zc.component.stratus.ZCObject;
import com.zc.component.stratus.beans.ZCListObjectOptions;
import java.util.List;

ZCListObjectOptions options = new ZCListObjectOptions();
options.setFolderListing("true");
options.setMaxKey("2");
options.setOrderBy("desc");
Iterable<List<ZCObject>> paginationIterable = bucket.listIterableObjects(options);
Iterator<List<ZCObject>> iterator = paginationIterable.iterator();
while (iterator.hasNext()) {
    List<ZCObject> objectList = iterator.next();
    for (ZCObject obj : objectList) {
        System.out.println(obj.getKey());
    }
}
```

### Check Object Availability

Uses `headObject()` to check if an object exists and if the user has permission to access it.

**Scope:** Admin scope required.

**Parameters:**
| Name | Type | Description |
|------|------|-------------|
| key | String | Mandatory. Complete name of the object |
| versionId | String | Optional. Unique version ID if Versioning enabled |
| throwErr | Boolean | Optional. If true, throws error. Default: false |

```java
Boolean throwErr = true;
Boolean headObjectRes = bucket.headObject("sam/out/sample.txt", "versionId", throwErr);
System.out.println(headObjectRes);
```

**Possible Errors (when throwErr=true):**
| Code | Description |
|------|-------------|
| 404 | Object Not found |
| 401 | Unauthorized/Access Denied |
| 403 | Permission Denied |

### Download Object

#### Basic Download

Returns an InputStream for the object.

```java
import java.nio.file.Path;
import java.nio.file.Files;
import java.nio.file.StandardCopyOption;
import java.io.*;

InputStream dataStream = bucket.getObject("sam/out/sample.txt");
Path path = Path.of("file_path");
Files.copy(dataStream, path, StandardCopyOption.REPLACE_EXISTING);
```

**Versioning note:** If Versioning enabled, pass versionId to download specific version. If no versionId passed, latest version downloaded. If Versioning was enabled then disabled, the principal first object downloaded by default — use `"topVersion"` to get latest.

#### Download a Portion of the Object

Uses `ZCGetObjectOptions` with `setRange()`.

```java
import com.zc.component.stratus.beans.ZCGetObjectOptions;

ZCGetObjectOptions options = ZCGetObjectOptions.getInstance();
options.setVersionId("3yt5ehjbjghds3i28");
options.setRange("20-200"); // start and end range in bytes
InputStream dataStream = bucket.getObject("sam/out/sample.txt", options);
Path path = Path.of("file_path");
Files.copy(dataStream, path, StandardCopyOption.REPLACE_EXISTING);
```

#### Download Using Transfer Manager

For large objects — splits into multiple byte ranges, each returned as a stream.

**Create Transfer Manager Instance:**
```java
import com.zc.component.stratus.transfer.ZCTransferManager;

ZCTransferManager transferManager = ZCTransferManager.getInstance(bucket);
```

**Download as Iterable Part Streams:**
```java
Iterable<InputStream> iterable = transferManager.getIterableObject("sam/out/sample.txt", 100L);
Path path = Path.of("file_path");
Iterator<InputStream> res = iterable.iterator();
while (res.hasNext()) {
    InputStream data = res.next();
    Files.copy(data, path, StandardCopyOption.REPLACE_EXISTING);
}
```

**Generate Part Downloaders:**
```java
import com.zc.component.stratus.beans.ZCStratusGetObject;

Path path = Path.of("file_path");
List<ZCStratusGetObject> parts = transferManager.generatePartDownloaders("sam/out/sample.txt", 100L);
Files.createFile(path);
int partNumber = 1;
for (ZCStratusGetObject part : parts) {
    InputStream inputStream = part.getPart();
    System.out.println("Part " + partNumber++ + " Downloaded");
    Files.write(path, inputStream.readAllBytes(), StandardOpenOption.APPEND);
}
```

#### Generate Presigned URL to Download

**Scope:** Admin scope required.

**Parameters:**
| Name | Type | Description |
|------|------|-------------|
| key | String | Mandatory. Complete object name with path |
| URL_ACTION | Enum | Mandatory. `URL_ACTION.GET` for download |
| expiry | String | Optional. URL validity in seconds. Default: 3600. Min: 30. Max: 7 days |
| activeFrom | String | Optional. Time after which URL is valid. Max: 7 days |

```java
import com.zc.component.stratus.enums.URL_ACTION;
import org.json.simple.JSONObject;

JSONObject res = bucket.generatePreSignedUrl("sam/out/sample.txt", URL_ACTION.GET);
System.out.println(res.get("signature"));
```

**With Expiry and Active Time:**
```java
JSONObject res = bucket.generatePreSignedUrl("object_name", URL_ACTION.GET, "expiry_in", "active_from");
System.out.println(res.get("signature"));
```

### Upload Object

**Character restrictions:** Double quote, angular brackets, hashtag, backward slash, pipe symbol, and spaces are NOT supported in path/object names.

#### Upload Object as a Stream

```java
InputStream file = new FileInputStream("filePath");
Boolean res = bucket.putObject("sam/out/sample.txt", file);
System.out.println(res);
```

#### Upload Object as a String

```java
Boolean res = bucket.putObject("sam/out/sample.txt", "content of the file");
System.out.println(res);
```

#### Upload Object with Options

Options available via `ZCPutObjectOptions`:
- `setOverwrite("true")` — Required if Versioning not enabled. Default: false
- `setTTL("1000")` — Time-to-Live in seconds (>= 60)
- `setMetaData(map)` — Metadata key-value pairs
- `contentType` — MIME type of the object

```java
import com.zc.component.stratus.beans.ZCPutObjectOptions;

ZCPutObjectOptions options = ZCPutObjectOptions.getInstance();
options.setTTL("1000");
options.setOverwrite("true");
Map<String, String> metaData = new HashMap<>();
metaData.put("author", "John");
options.setMetaData(metaData);
InputStream file = new FileInputStream("filePath");
Boolean res = bucket.putObject("sam/out/sample.txt", file, options);
System.out.println(res);
```

#### Upload with Extract Option (Zip)

Uploads a zip file and extracts contents as individual objects. Async operation — returns taskId.

```java
ZCPutObjectOptions options = ZCPutObjectOptions.getInstance();
options.setOverwrite("true");
InputStream stream = new FileInputStream("file_path");
JSONObject object = bucket.putZipObject("sam.zip", stream, options);
// Returns: { "task_id": "1234263749" }
```

#### Upload Using Multipart Operations

For large objects. Parts: 1-1000, min 50 MB per part.

**Initiate:**
```java
import com.zc.component.stratus.beans.ZCInitiateMultipartUpload;

ZCInitiateMultipartUpload multipart = bucket.initiateMultipartUpload("sam/out/sample.txt");
String uploadId = multipart.getUploadId();
```

**Upload Parts:**
```java
int partNumber = 1;
InputStream part = new FileInputStream("filePath");
Boolean res = bucket.uploadPart("sam/out/sample.txt", "uploadId", part, partNumber);
```

**Get Upload Summary:**
```java
import com.zc.component.stratus.beans.ZCMultipartObjectSummary;

ZCMultipartObjectSummary summaryRes = bucket.getMultipartUploadSummary("sam/out/sample.txt", "uploadId");
System.out.println("Object Name:" + summaryRes.getKey());
System.out.println("Upload Id:" + summaryRes.getUploadId());
System.out.println("Status:" + summaryRes.getStatus());
System.out.println(summaryRes.getParts().get(0).getUploadedAt());
System.out.println(summaryRes.getParts().get(0).getPartNumber());
```

**Complete:**
```java
Boolean completeRes = bucket.completeMultipartUpload("sam/out/sample.txt", "uploadId");
```

**Full Parallel Example:** Uses `CompletableFuture` with `ExecutorService.newFixedThreadPool(4)` to upload parts in parallel, then calls `completeMultipartUpload`.

#### Upload Using Transfer Manager

**Create Instance:**
```java
import com.zc.component.stratus.transfer.ZCTransferManager;

ZCTransferManager transferManager = ZCTransferManager.getInstance(bucket);
```

**Create Multipart Instance:**
```java
import com.zc.component.stratus.beans.ZCMultipartUpload;

ZCMultipartUpload multipart = transferManager.createMultipartInstance("sam/out/sample.txt");
// Or for existing upload:
ZCMultipartUpload multipart = transferManager.createMultipartInstance("sam/out/sample.txt", "uploadId");
```

**Upload Part:**
```java
int partNumber = 1;
InputStream part = new FileInputStream("filePath");
Boolean uploadRes = multipart.uploadPart(part, partNumber);
```

**Upload Summary:**
```java
ZCMultipartObjectSummary summaryRes = multipart.getUploadSummary();
```

**Complete:**
```java
Boolean completeRes = multipart.completeUpload();
```

**Wrapper (putObjectAsParts):** Handles entire multipart process in one call.
```java
InputStream file = new FileInputStream("filePath");
int partSize = 50; // MB
ZCMultipartObjectSummary res = transferManager.putObjectAsParts("objectName", file, partSize);
```

> Note: For objects larger than 2GB, use individual SDK methods instead of the wrapper.

#### Generate Presigned URL to Upload

**Scope:** Admin scope required.

```java
import com.zc.component.stratus.enums.URL_ACTION;
import org.json.simple.JSONObject;

JSONObject res = bucket.generatePreSignedUrl("sam/out/sample.txt", URL_ACTION.PUT);
System.out.println(res.get("signature"));
```

**With Expiry and Active Time:**
```java
JSONObject res = bucket.generatePreSignedUrl("object_name", URL_ACTION.PUT, "expiry_in", "active_from");
System.out.println(res.get("signature"));
```

**Response:**
```json
{
  "signature": "https://...",
  "expiry_in_seconds": "100",
  "active_from": "1726492859577"
}
```

### Extract a Zipped Object

Extracts a zip file inside Stratus — each content becomes an individual object. Async process.

**Scope:** Admin scope required.

**Parameters:**
| Name | Type | Description |
|------|------|-------------|
| key | String | Mandatory. Name of the zip file |
| destination | String | Mandatory. Complete path where extracted objects will be stored |

```java
import com.zc.component.stratus.beans.ZCStratusZipExtractResponse;

ZCStratusZipExtractResponse res = bucket.unzipObject("sam/out/sample.zip", "output/");
System.out.println(res.getObjectName());
System.out.println(res.getTaskId());
```

**Get Extraction Status:**
```java
JSONObject res = object.getUnzipStatus("sam/out/sample.zip", "taskId");
// Response: { "task_id": "6963000000272049", "status": "SUCCESS" }
```

### Copy Object

Copies an object within a bucket.

**Scope:** Admin scope required.

**Parameters:** key (source with full path), destination (target with full path).

```java
JSONObject copyRes = bucket.copyObject("sam/out/sample.txt", "output/sample.txt");
// Response: { "copy_to": "output/sample.txt", "object_key": "sam/out/sample.txt", "message": "Object copied successfully." }
```

### Rename and Move Object

Both use the same `renameObject()` method.

**Scope:** Admin scope required.

> Note: Cannot rename/move objects in a bucket with Versioning enabled.

#### Rename

```java
JSONObject res = bucket.renameObject("sam/out/sample.txt", "sam/out/update_sample.txt");
```

#### Move

```java
JSONObject res = bucket.renameObject("sam/out/sample.txt", "output/sample.txt");
```

### Delete Objects

**Scope:** Admin scope required.

**Parameters:**
| Name | Type | Description |
|------|------|-------------|
| key | String | Mandatory. Complete object name with path |
| versionId | String | Optional. Version ID if Versioning enabled |
| ttl | int | Optional. Schedule delete in N seconds (>= 60) |

#### Delete Single Object

```java
int ttl = 200;
JSONObject deleteRes = bucket.deleteObject("sam/out/sample.txt", "versionId", ttl);
```

> Note: If Versioning enabled and no versionId provided, ALL versions deleted.

#### Delete Multiple Objects

```java
import com.zc.component.stratus.beans.ZCDeleteObjectRequest;

ZCDeleteObjectRequest deleteRequest = ZCDeleteObjectRequest.getInstance();
deleteRequest.setObject("sam/out/sample.txt", "76dhe7yr738rud");
deleteRequest.setObject("sam/out/add.txt", "cjdhf73673g7yt7d");
deleteRequest.setTTL(70);
JSONObject res = bucket.deleteObjects(deleteRequest);
```

#### Truncate Bucket

Deletes every single object in the bucket.

```java
JSONObject truncateRes = bucket.truncate();
```

#### Delete a Path

Deletes all objects in a specified path.

```java
JSONObject res = bucket.deletePath("sam/");
```

---

## Object Operations

### Create Object Instance

Creates an object reference for object-level operations. Uses a bucket instance.

```java
import com.zc.component.stratus.ZCObject;

ZCObject object = bucket.getObjectInstance("sam/out/sample.txt");
```

### List Versions of an Object

**Scope:** Admin scope required.

#### List by Pagination

```java
import com.zc.component.stratus.beans.ZCObjectVersions;
import com.zc.component.stratus.beans.ZCObjectVersions.ZCVersionDetail;

String nextToken = null;
int maxVersion = 5;
do {
    ZCObjectVersions res = object.listPagedVersions(maxVersion, nextToken);
    for (ZCVersionDetail version : res.getVersion()) {
        System.out.println("version id: " + version.getVersionId());
    }
    nextToken = res.getNextToken();
} while (nextToken != null);
```

#### List in Iterable Manner

```java
int maxVersion = 10;
Iterable<List<ZCVersionDetail>> paginationIterable = object.listIterableVersions(maxVersion);
Iterator<List<ZCVersionDetail>> iterator = paginationIterable.iterator();
while (iterator.hasNext()) {
    List<ZCVersionDetail> objects = iterator.next();
    for (ZCVersionDetail obj : objects) {
        System.out.println(obj.getVersionId());
    }
}
```

### Get Object Details

**Scope:** Admin scope required.

#### Get Details (Latest Version)

```java
import com.zc.component.stratus.ZCObject;

ZCObject objectRes = object.getDetails();
```

> Note: If Versioning enabled, returns only the latest version's details.

#### Get Details (Specific Version)

```java
ZCObject objectRes = object.getDetails("versionId");
```

> Use `"topVersion"` to get the latest version.

### Put Object Meta Data

Adds/replaces metadata for an object.

**Scope:** Admin scope required.

**Constraints:**
- Alphanumeric, underscores, whitespace, hyphens only for metadata keys/values
- Maximum 2047 characters total (including `:` separators)
- Passing new meta without existing meta DELETES existing — pass all meta together

```java
import org.json.simple.JSONObject;
import java.util.HashMap;

HashMap<String, String> objectMeta = new HashMap<>();
objectMeta.put("key1", "value1");
objectMeta.put("key2", "value2");
JSONObject res = object.putMeta(objectMeta);
// Response: { "message": "Metadata added successfully" }
```

---

## Key Classes Summary

| Class | Package | Purpose |
|-------|---------|---------|
| ZCStratus | com.zc.component.stratus | Stratus component entry point |
| ZCBucket | com.zc.component.stratus | Bucket operations (most object ops too) |
| ZCObject | com.zc.component.stratus | Object-level operations (versions, details, meta) |
| ZCTransferManager | com.zc.component.stratus.transfer | Large file upload/download via parts |
| ZCPutObjectOptions | com.zc.component.stratus.beans | Upload options (TTL, overwrite, metadata) |
| ZCGetObjectOptions | com.zc.component.stratus.beans | Download options (version, byte range) |
| ZCListObjectOptions | com.zc.component.stratus.beans | List options (maxKey, token, prefix, order) |
| ZCInitiateMultipartUpload | com.zc.component.stratus.beans | Multipart upload initiation |
| ZCMultipartUpload | com.zc.component.stratus.beans | Transfer Manager multipart instance |
| ZCMultipartObjectSummary | com.zc.component.stratus.beans | Upload summary with part details |
| ZCPagedObjectResponse | com.zc.component.stratus.beans | Paginated object list response |
| ZCDeleteObjectRequest | com.zc.component.stratus.beans | Multi-delete request builder |
| ZCStratusCorsResponse | com.zc.component.stratus.beans | CORS configuration response |
| ZCStratusZipExtractResponse | com.zc.component.stratus.beans | Zip extraction response |
| ZCStratusGetObject | com.zc.component.stratus.beans | Part downloader for Transfer Manager |
| ZCObjectVersions | com.zc.component.stratus.beans | Version listing response |
| URL_ACTION | com.zc.component.stratus.enums | Enum for presigned URL actions (GET/PUT) |
