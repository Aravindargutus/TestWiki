# Catalyst Java SDK — Data Store (Cloud Scale)

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/cloud-scale/data-store/
> Sub-pages: get-table-meta, get-column-meta, get-table-instance, insert-rows, get-rows, update-rows, delete-row, bulk-read, bulk-write, bulk-delete-rows
> Captured: 2026-04-05

The Data Store is a cloud-scale relational database component of Catalyst. The Java SDK provides operations for table/column metadata, CRUD on rows, and bulk operations.

---

## 1. Get Table Meta

Fetch metadata of tables. Key classes: `ZCObject`, `ZCTable`.

### Get single table by ID
```java
ZCObject object = ZCObject.getInstance();
ZCTable tableMeta = object.getTable(1510000000110121L);
```

### Get single table by name
```java
ZCObject object = ZCObject.getInstance();
ZCTable tableMeta = object.getTable("SampleTable");
```

### Get all tables
```java
ZCObject object = ZCObject.getInstance();
List tableList = object.getAllTables();
```

---

## 2. Get Column Meta

Fetch metadata of columns within a table. Key classes: `ZCObject`, `ZCTable`, `ZCColumn`.

### Get column by ID
```java
ZCObject object = ZCObject.getInstance();
ZCTable table = object.getTable(1510000000110121L);
ZCColumn column = table.getColumn("1510000000110832");
```

### Get column by name
```java
ZCObject object = ZCObject.getInstance();
ZCTable table = object.getTable(1510000000110121L);
ZCColumn column = table.getColumn("Name");
```

### Get all columns
```java
ZCObject object = ZCObject.getInstance();
ZCTable table = object.getTable(1510000000110121L);
List columns = table.getAllColumns();
```

---

## 3. Get a Table Instance

Create an empty table instance without firing a server-side call. Uses `getTableInstance()` (vs `getTable()` which makes an API call).

### By table ID
```java
ZCObject object = ZCObject.getInstance();
ZCTable tableMeta = object.getTableInstance(1510000000110121L);
```

### By table name
```java
ZCObject object = ZCObject.getInstance();
ZCTable tableMeta = object.getTableInstance("SampleTable");
```

---

## 4. Insert Rows

Insert single or multiple rows. Tables and columns must be pre-created from the console.

**Dev limits:** 5,000 records per table, 25,000 records total per project. No limits in production.

### Insert single row
```java
ZCObject object = ZCObject.getInstance();
ZCTable tab = object.getTable("1510000000110121");
ZCRowObject row = ZCRowObject.getInstance();
row.set("Name", "George Smith");
row.set("Age", 25);
tab.insertRow(row);
```

### Insert multiple rows
```java
List rows = new ArrayList();
ZCObject object = ZCObject.getInstance();
ZCTable tab = object.getTable(1510000000110121L);
ZCRowObject row1 = ZCRowObject.getInstance();
ZCRowObject row2 = ZCRowObject.getInstance();
row1.set("Name", "George Smith");
row1.set("Age", 25);
row2.set("Name", "Moana Violet");
row2.set("Age", 22);
rows.add(row1);
rows.add(row2);
tab.insertRows(rows);
```

A unique ROWID is automatically generated for each inserted row.

---

## 5. Get Rows

### Get single row by ROWID
```java
ZCObject obj = ZCObject.getInstance();
ZCTable tab = obj.getTable(1510000000110121L);
ZCRowObject row = tab.getRow(1510000000108103L);
```

### Get all rows through pagination (v1.7.0+)
```java
String nextToken = null;
ZCRowPagedResponse pagedResp;
Long maxRows = 100; // optional, default 200

do {
    pagedResp = ZCObject.getInstance().getTable("EmpDetails").getPagedRows(nextToken, maxRows);
    for (ZCRowObject row : pagedResp.getRows()) {
        // process row.get("empID"), row.get("empName"), etc.
    }
    if (pagedResp.moreRecordsAvailable()) {
        nextToken = pagedResp.getNextToken();
    }
} while (pagedResp.moreRecordsAvailable());
```

**Note:** `getAllRows()` is **deprecated**. Use `getPagedRows()` with pagination instead. `getAllRows()` will be removed in future SDK versions.

---

## 6. Update Rows

Update one or more rows using `updateRows()`. **ROWID must be set** on each row object.

```java
ZCObject object = ZCObject.getInstance();
ZCTable table = object.getTable(1510000000110121L);
List rows = new ArrayList();
ZCRowObject row1 = ZCRowObject.getInstance();
ZCRowObject row2 = ZCRowObject.getInstance();
row1.set("Name", "Amelia S");
row1.set("Age", 19);
row1.set("ROWID", 1510000000109113L);
row2.set("Name", "Walker Don");
row2.set("Age", 19);
row2.set("ROWID", 1510000000109115L);
rows.add(row1);
rows.add(row2);
table.updateRows(rows);
```

---

## 7. Delete Row

Delete a single row by ROWID using `deleteRow()`. Cannot delete more than one row at a time (use bulk delete for multiple).

```java
ZCObject obj = ZCObject.getInstance();
ZCTable tab = obj.getTable(1510000000110121L);
tab.deleteRow(1510000000109115L);
```

---

## 8. Bulk Read Rows

Read thousands of records from a table, generating a CSV output file.

**Key classes:** `ZCBulkReadServices`, `ZCBulkQueryDetails`, `ZCBulkCallbackDetails`, `ZCDataStoreBulk`, `ZCBulkReadDetails`, `ZCBulkResult`

**Max:** 200,000 rows per bulk read job.

**Methods:**
- `createBulkReadJob()` — multiple overloads (table ID only, with query details, with callback details, with bulk read details object)
- `getBulkReadJobStatus(jobId)` — check status and get result

```java
ZCBulkReadServices bulkRead = ZCDataStoreBulk.getInstance().getBulkReadInstance();
bulkRead.createBulkReadJob(12096000000642178L); // simple: table ID only
// Or with query and callback details:
ZCBulkQueryDetails bulkQueryDetails = ZCBulkQueryDetails.getInstance();
ZCBulkCallbackDetails callbackDetails = ZCBulkCallbackDetails.getInstance();
bulkRead.createBulkReadJob(12096000000642178L, bulkQueryDetails, callbackDetails);
// Or with full details object:
ZCBulkReadDetails bulkReadDetails = ZCBulkReadDetails.getInstance();
bulkReadDetails.setTableIdentifier(12096000000642178L);
ZCBulkResult readJob = bulkRead.createBulkReadJob(bulkReadDetails);
bulkRead.getBulkReadJobStatus(readJob.getJobId());
```

---

## 9. Bulk Write Rows

Write thousands of records from a CSV file (uploaded to Stratus) into a table.

**Key classes:** `ZCBulkWriteServices`, `ZCBulkWriteDetails`, `ZCBucketObjectDetails`, `ZCDataStoreBulk`, `ZCBulkResult`

**Max:** 100,000 rows per bulk write job.

**Prerequisites:** CSV file must be uploaded to Stratus first. Reference via bucketName, objectKey, versionID.

**Methods:**
- `createBulkWriteJob(bulkWriteDetails)` — generic write
- `createInsertBulkWriteJob(tableId, objectDetails)` — insert only
- `createUpdateBulkWriteJob(tableId, objectDetails, columnId)` — update only
- `createUpsertBulkWriteJob(tableId, objectDetails, columnId)` — upsert
- `getBulkWriteJobStatus(jobId)` — check status

```java
ZCBulkWriteServices bulkWrite = ZCDataStoreBulk.getInstance().getBulkWriteInstance();
ZCBulkWriteDetails bulkWriteDetails = ZCBulkWriteDetails.getInstance();
bulkWriteDetails.setTableIdentifier(12096000000642178L);
bulkWriteDetails.setObjectDetails(objectDetails);
ZCBulkResult bulkWriteResult = bulkWrite.createBulkWriteJob(bulkWriteDetails);

// Convenience methods:
bulkWrite.createInsertBulkWriteJob(12096000000642178L, objectDetails);
bulkWrite.createUpdateBulkWriteJob(12096000000642178L, objectDetails, 12096000000642900L);
bulkWrite.createUpsertBulkWriteJob(12096000000642178L, objectDetails, 12096000000642900L);
bulkWrite.getBulkWriteJobStatus(bulkWriteResult.getJobId());
```

---

## 10. Bulk Delete Rows

Delete up to 200 rows in a single operation by passing an ArrayList of ROWIDs.

```java
ArrayList rowIdList = new ArrayList<>();
rowIdList.add(1028000000171815L);
rowIdList.add(1028000000171810L);
rowIdList.add(1028000000171805L);
rowIdList.add(1028000000171617L);
rowIdList.add(1028000000171098L);
List<ZCRowObject> deletedRowList = ZCObject.getInstance().getTableInstance("EmpDetails").deleteRows(rowIdList);
```

**Min:** 1 ROWID, **Max:** 200 ROWIDs per operation.
