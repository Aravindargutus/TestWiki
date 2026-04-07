# Catalyst Java SDK — NoSQL, Security Rules & API Gateway

## Source URLs
- NoSQL SDK: https://docs-ea.catalyst.zoho.com/en/sdk/java/v1/cloud-scale/nosql/get-table-metadata/
- Security Rules: https://docs-ea.catalyst.zoho.com/en/serverless/help/security-rules/introduction/
- API Gateway: https://docs-ea.catalyst.zoho.com/en/cloud-scale/help/api-gateway/introduction/

---

## NoSQL SDK (10 pages)

### Overview
Catalyst NoSQL is a fully managed non-relational, NoSQL data storage feature. Supports document-type data storage in key-value pair based JSON format. The Java SDK package enables CRUD data operations on NoSQL tables. Entry point: `ZCNoSQL.getInstance()`.

### Page 1: Get Table Metadata
- `ZCNoSQL.getInstance().getTable(tableId)` — get single table metadata by ID
- `ZCNoSQL.getInstance().getTable(tableName)` — get single table metadata by name
- `ZCNoSQL.getInstance().getAllTables()` — get metadata of all tables
- Response contains table configuration: partition key, sort key, TTL attribute, etc.

### Page 2: Create Table Instance
- `ZCNoSQL.getInstance().getTableInstance(tableId)` — get empty table instance by ID
- `ZCNoSQL.getInstance().getTableInstance(tableName)` — get empty table instance by name
- Returns empty instance (no server call) used to refer to table for operations
- Similar to the `getInstance()` pattern used across the SDK

### Page 3: Construct NoSQL Item
- `new ZCNoSQLItem()` — create new empty item
- `ZCNoSQLItem.fromJSON(jsonString)` — create item from JSON
- Builder-style methods for different data types:
  - `item.withString()`, `item.withNumber()`, `item.withInt()`, `item.withBigInteger()`
  - `item.withShort()`, `item.withFloat()`, `item.withDouble()`, `item.withLong()`
  - `item.withBinary()`, `item.withBoolean()`, `item.withNull()`
  - `item.withStringSet()`, `item.withBigDecimalSet()`, `item.withNumberSet()`, `item.withBinarySet()`, `item.withByteBufferSet()`
  - `item.withList()`, `item.withMap()`, `item.withJSON()`
  - `item.with(attrName, value)` — generic
- Partition key attribute value is mandatory in every item

### Page 4: NoSQL Item Operations
- `item.removeAttribute(attrName)` — remove attribute
- `item.attributes()` — get all keys (returns Iterable<Map.Entry<String, Object>>)
- `item.hasAttribute(attrName)` — check if attribute exists
- `item.asMap()` / `item.getAllAttributesAsMap()` — get item as map
- `item.toJSON()` — get item as JSON string
- `item.numberOfAttributes()` — get attribute count
- **ZCNoSQLAttribute**: Used to indicate attributes for operations. Supports nested access via ',' separator and list index via "[<index>]".
  - `ZCNoSQLAttribute.getInstance(pathElements...)` or `new ZCNoSQLAttribute(pathElements)`
  - Data types: S (String), N (Numeric), B (Binary), BOOL (Boolean), SS (Set<String>), SN (Set<Number>), SB (Set<Binary>), L (List), M (Map), NuLL (Null)
- **ZCNoSQLValue**: Indicates value with data type.
  - `new ZCNoSQLValue(DataType, value)` or `ZCNoSQLValue.getInstance(DataType, value)`
- **ZCNoSQLResponseBean**: Response of SDK calls.
  - `getSize()` — size of data read/written
  - `getStartKey()` — pagination start key for next set
  - `getResponseDataList()` — actual data, with `getNew_item()` / `getOld_item()` / `setStatus()`

### Page 5: Insert Items in Table
- Max 25 items per bulk insert operation
- Simple insert: `table.insert(item)` or `table.getInsertHelper(item).insert()`
- **Conditional insert**: `table.getInsertHelper(item).withCondition(condition).insert()`
  - Evaluates existing data against condition; inserts only if true
  - If no existing data, conditions are ignored
- **ZCNoSQLCondition** — 3 ways to construct:
  1. Using functions: `ZCNoSQLCondition.getInstance(NoSQLConditionFunction)`
     - `ZCNoSQLAttributeTypeFunction` — check data type match
     - `ZCNoSQLAttributeExistFunction` — check if attribute exists
  2. Using operator/operand/value: `ZCNoSQLCondition.getInstance(attribute, operator, value)`
     - NOSQL_OPERATOR values: contains, not_contains, begins_with, ends_with, in, not_in, between, not_between, equals, not_equals, greater_than, less_than, greater_equal, less_equal
  3. Group of conditions: `ZCNoSQLCondition.getInstance(List<conditions>, groupOperator)`
     - NOSQL_CONDITION_GROUP_OPERATOR: AND, OR
- **NOSQL_RETURN_VALUE**: NEW, OLD, NULL
- Full chain: `table.getInsertHelper(item).withCondition(cond).withReturnValue(rv).insert()`

### Page 6: Update Items
- Max 25 items per bulk update operation
- Identify items by primary keys (partition key, or partition + sort key)
- Simple update: `table.update(item, updateOp)`
- Helper: `table.getUpdateHelper(item, updateOp).update()`
- **ZCNoSQLUpdateAttributeOperation**:
  - Put attribute: `ZCNoSQLUpdateAttributeOperation.getPutAttributeInstance(attr, value)`
  - Delete attribute: `ZCNoSQLUpdateAttributeOperation.getDeleteAttributeInstance(attr)`
- **NoSQLUpdateAttributeFunction** (4 prebuilt):
  1. `ZCNoSQLIfNotExistFunction` — update with value of another attribute, fallback to given value
  2. `ZCNoSQLAppendListFunction` — append elements to list
  3. `ZCNoSQLAdditionFunction` — add to set or add numeric
  4. `ZCNoSQLReductionFunction` — remove from set or subtract numeric
- Conditions and NOSQL_RETURN_VALUE reusable from insert
- Full chain: `table.getUpdateHelper(item, updateOp).withCondition(cond).withReturnValue(rv).update()`

### Page 7: Fetch Items from NoSQL Table
- Max 100 items per fetch operation
- Identify items by primary keys
- Simple fetch: `table.fetch(key)` (key is a ZCNoSQLItem with primary key values)
- Helper: `table.getFetchHelper(key).fetch()`
  - `withRequiredAttributes(List<ZCNoSQLAttribute>)` — filter specific attributes
  - `withConsistency(true/false)` — true = read from master, false = read from slave (cheaper, slight delay)
- Full chain: `table.getFetchHelper(key).withRequiredAttributes(attrs).withConsistency(true/false).fetch()`

### Page 8: Query NoSQL Table
- Max 100 items with pagination
- Identify by partition key (required) + optional sort key
- Simple query: `table.query(partitionKeyCondition, forwardScan, limit)`
- Helper: `table.getQueryHelper(partitionKeyCondition, forwardScan, limit).queryTable()`
- **ZCNoSQLPartitionKeyCondition**: `getInstance(attribute, value)` — required for all queries
- **ZCNoSQLSecondaryKeyCondition**: Optional sort key criteria
  - SECONDARY_KEY_CONDITION_OPERATOR: begins_with, between, equals, greater_than, less_than, greater_equal, less_equal
  - `withSecondaryKeyCondition(condition, isAdditionalSortKey)`
- Other options:
  - `withOtherCondition(ZCNoSQLCondition)` — filter after key retrieval (may return 0 results even if data exists)
  - `withStartKey(ZCNoSQLItem)` — pagination start key from previous response
  - `withRequiredAttributes(List)` — filter attributes
  - `withConsistency(true/false)` — master vs slave read

### Page 9: Query NoSQL Index
- Max 100 items with pagination
- Same as Query Table but targets an index instead of main table
- Simple: `table.queryIndex(indexID, partitionKeyCondition, forwardScan, limit)`
- Helper: `table.getQueryHelper(partitionKeyCondition, forwardScan, limit).queryIndex(indexID)`
- All query options reusable (secondary key, other conditions, required attributes, consistency, start key)
- **Exception**: additionalSortKey cannot be used for index queries
- Full chain: `table.getQueryHelper(...).withSecondaryKeyCondition(cond, false).withOtherCondition(cond).withRequiredAttributes(attrs).withConsistency(true/false).withStartKey(startKey).queryIndex(indexID)`

### Page 10: Delete Items from Table
- Max 25 items per bulk delete operation
- Identify by primary keys (partition key, or partition + sort key)
- Simple delete: `table.delete(key)`
- Helper: `table.getDeleteHelper(key).delete()`
- Conditions and NOSQL_RETURN_VALUE reusable
- Full chain: `table.getDeleteHelper(key).withCondition(cond).withReturnValue(rv).delete()`

---

## Security Rules (3 pages)

### Page 1: Introduction
- Security Rules is a Serverless component for defining invocation and access rules of Basic I/O and Advanced I/O functions
- JSON file that configures: HTTP methods allowed, authentication required/optional
- Default security configuration — created automatically when function is deployed
- Does NOT apply to Cron and Event functions (not directly user-accessible)
- Same for Java, Node.js, and Python functions
- **Relationship with API Gateway**: API Gateway is an enhancement to Security Rules
  - When API Gateway enabled → Security Rules disabled automatically
  - When API Gateway disabled → Security Rules re-enabled
  - Security Rules definitions can be migrated to API Gateway via auto-create

### Page 2: Key Concepts
- Referenced from sidebar but content primarily in introduction

### Page 3: Implementation
- Referenced from sidebar but content primarily in introduction

---

## API Gateway (5 pages)

### Page 1: Introduction
- Intermediate layer between client and server (reverse proxy)
- Routes client requests to individual services
- Targets: Basic I/O Functions, Advanced I/O Functions, Web Client
- Handles: Routing, Authentication (optional), Throttling (optional)
- Optional, paid component — enhancement to Security Rules
- When enabled, all function/web client URLs become inaccessible until APIs are created
- Can be enabled/disabled at any time
- CLI support available

### Page 2: Key Concepts
- **Security Rules vs API Gateway comparison**:
  - Security Rules: same request/target URL, 2 auth types, no throttling, no web client rules
  - API Gateway: custom request/target URLs, 3 auth types (adds API Key), throttling, web client support, ANY method aggregation

- **Routing**:
  - Request Methods: GET, PUT, POST, DELETE, PATCH + ANY (aggregates all)
  - Request URL: custom path appended to project domain
  - Target URL: Basic I/O, Advanced I/O, or Web Client URLs
  - Regex support in URLs with pattern matching and dynamic values
  - Web client APIs only support GET

- **Authentication Request Processor** (3 methods, any or all):
  1. **API Key**: Auto-generated by Catalyst. Different keys for dev/prod. Pass via header (`ZCFKEY`) or query parameter
  2. **Catalyst Users Authentication**: For app users added in Authentication component
  3. **OAuth-Based Authentication**: Via OAuth access token in Authorization header
  - Not available for web client APIs

- **Throttling** (2 types, either or both):
  1. **General Throttling**: Max hits per time unit for all users. Sliding window algorithm.
  2. **IP-Based Throttling**: Max hits per time unit from specific IP address
  - Exceeding limits returns HTTP 429 Too Many Requests

- **API types**:
  - Auto-created: Migrates from Security Rules, follows default protocols
  - Custom: Fully configurable
  - Default (Login Redirect): Created for web client, based on client-package.json login_redirect, cannot be deleted

### Page 3: Architecture
- Flow sequence: Read request method/URL → Search for matching API → Initiate or deny → Check throttling → Check authentication → Redirect to target URL

### Page 4: Benefits
1. Efficient API Management — single endpoint, ANY method aggregation
2. Enhanced Security — decouples clients from services, prevents backend exposure
3. Offloading — auth/throttling handled centrally, not per-service
4. High Performance — throttling prevents resource exhaustion, sliding window rate limiting

### Page 5: Implementation
- **Enable**: Navigate to API Gateway in console → Click Enable Now → Confirm
  - Warning: all URLs become inaccessible until APIs created
- **Create API**:
  - Auto-create: Only available first time, migrates Security Rules definitions
  - Custom: Select method, request URL, target, auth, throttling
  - Hard limit: 1000 APIs per project in dev, unlimited in prod
- **Edit API**: Modify definitions at any time
- **Delete API**: Renders target URL inaccessible
- **Disable**: Type "DISABLE" to confirm; Security Rules re-enabled; APIs preserved for re-enable
