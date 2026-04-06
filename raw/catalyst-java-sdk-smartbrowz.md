# Catalyst Java SDK — SmartBrowz

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/smartbrowz/
> Pages fetched: Browser Grid (6: Overview, Get Instance, Get All, Get Specific, Get Node, Stop) + PDF & Screenshot (1)
> Total: 7 pages
> Date fetched: 2026-04-06

---

## 1. Browser Grid

Browser Grid is SmartBrowz's auto-scaling component for configuring and managing multiple headless browsers. Configure nodes and browsers per grid.

### Overview — SDK Methods

| Category | Operations | Scope |
|----------|-----------|-------|
| General | Get Browser Grid Instance | Admin |
| Browser Grid | Get all grids, Get specific grid (by ID/name), Get nodes (by ID/name), Stop grid (by ID/name) | Admin |

All operations require **Admin scope**.

### 1.1 Get Browser Grid Instance
```java
import com.zc.component.smartbrowz.*;
ZCBrowserGrid grid = ZCBrowserGrid.getInstance(); // no server call
```

### 1.2 Get All Browser Grid Details
```java
List<ZCGrid> gridList = grid.getGrid();
// Returns list of all grids with: id, name, memory, browser_version (chrome/firefox),
// created/modified time, project_details, endpoint_type, max_session/nodes/concurrent_count, config_type
```

### 1.3 Get Specific Browser Grid
```java
// By ID:
ZCBrowserGrid gridDetails = grid.getGrid(3970000000005013L);
// By Name:
ZCBrowserGrid gridDetails = grid.getGrid("Selenium_Grid");
```

### 1.4 Get Details of a Node
```java
// By Grid ID:
ZCBrowserGrid nodeDetails = grid.getGridNodes(3970000000005013L);
// By Grid Name:
ZCBrowserGrid nodeDetails = grid.getGridNodes("Selenium_Grid");
```

### 1.5 Stop Browser Grid
Terminates all executions and stops the grid.
```java
// By Grid ID:
ZCBrowserGrid gridTerminate = grid.stopGrid(3970000000005013L);
// By Grid Name:
ZCBrowserGrid gridTerminate = grid.stopGrid("Selenium_Grid");
// Response: { "status": "success", "data": true }
```

Grid response includes: id, name, memory (1024 default), browser_version (chrome + firefox), endpoint_type, max_session_count, max_nodes_count, max_concurrent_count, config_type.

---

## 2. PDF & Screenshot

Generate visual documents from HTML, URL, or predefined templates. Entry point: `ZCSmartBrowz.getInstance()`.

### 2.1 Generate from Template
```java
ZCSmartBrowz smartBrowz = ZCSmartBrowz.getInstance();
ZCSmartBrowzTemplateOptions templateOptions = ZCSmartBrowzTemplateOptions.getInstance();
templateOptions.setTemplateId(templateId);
templateOptions.setTemplateInput(templateData); // Jackson JsonNode
templateOptions.setOutputType(ZC_CONVERT_OUTPUT_TYPE.PDF);
templateOptions.setPdfDetails(pdfOptions);
templateOptions.setNavigationDetails(navOptions);
templateOptions.setPageDetails(pageOptions);
InputStream output = smartBrowz.generateFromTemplate(templateOptions);
```

### 2.2 Convert to PDF from HTML
```java
ZCSmartBrowzConvertDetails convertDetails = ZCSmartBrowzConvertDetails.getInstance();
convertDetails.setHtml("<html>Hello</html>");
convertDetails.setPdfDetails(pdfOptions);
convertDetails.setNavigationDetails(navOptions);
convertDetails.setPageDetails(pageOptions);
InputStream output = smartBrowz.convertToPdf(convertDetails);
```

### 2.3 Take Screenshot from URL
```java
ZCSmartBrowzConvertDetails convertDetails = ZCSmartBrowzConvertDetails.getInstance();
convertDetails.setUrl("http://www.example.com");
convertDetails.setPdfDetails(pdfOptions);
InputStream output = smartBrowz.convertToPdf(convertDetails);
```

### PDF Options
- `setFormat("A4")`, `setLandscape(true)`, `setScale(1.0)`
- `setDisplayHeaderFooter(true)`, `setPageRanges("1-2")`
- `setPrintBackground(true)`, `setPassword("...")` (template password)
- `setMargin(marginDetails)` with top/right/left/bottom
- `setWidth/Height("100")`

### Page Options
- `setCss(contentDetails)` for custom HTML content
- `setDevice("Blackberry PlayBook")` for device emulation
- `setJavaScriptEnabled(true)`
- `setViewport(viewportDetails)` with width/height

### Navigation Options
- `setWaitUntil("domcontentloaded")`
- `setTimeout(30000)`

Key classes: `ZCSmartBrowz`, `ZCSmartBrowzConvertDetails`, `ZCSmartBrowzTemplateOptions`, `ZCSmartBrowzPDFOptions`, `ZCSmartBrowzNavigationOptions`, `ZCSmartBrowzPageOptions`

Note: Browser operations via SmartBrowz are at user's own risk. Use on domains that permit the actions.
