---
title: File Store
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, storage, file-store]
---

# File Store

## Definition

File Store is Catalyst's original cloud-scale file storage component, organized into folders and files. It provides simple upload, download, and delete operations with a 100MB file size limit. It is being superseded by [[stratus]], which offers multipart uploads, versioning, presigned URLs, and much larger file support. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Aspects

- **Folder-based organization**: Files are stored inside named folders, accessed via [[zcfile]] → [[zcfolder]]
- **5 SDK operations**: Create folder instance, get folder details, upload file, download file, delete file
- **100MB upload limit** per file
- **Dev limits**: 1GB storage per project; unlimited in production
- **Irreversible deletes**: Files cannot be restored once deleted
- **Download returns InputStream**: Caller handles the stream

## SDK Entry Point

```java
ZCFile fileStore = ZCFile.getInstance();
ZCFolder folder = fileStore.getFolderInstance(folderId);
folder.uploadFile(new File("data.csv"));
InputStream is = folder.downloadFile(fileId);
folder.deleteFile(fileId);
```

## Node.js SDK Access Pattern

```js
const filestore = app.filestore();
const folder = filestore.folder(folderId);
await folder.uploadFile({ code: fileStream, name: 'data.csv' });
const stream = await folder.downloadFile(fileId);
await folder.deleteFile(fileId);
const details = await folder.getDetails();
```

Node.js SDK also notes that Stratus is a "significant upgrade to File Store, available in Early Access." [Source: catalyst-nodejs-sdk-cloud-scale-remaining.md]

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js File Store (6 pages)

## Related Concepts

- [[stratus]] — The newer, more capable replacement for File Store
- [[data-store]] — Relational data counterpart to file-based storage
- [[sdk-scopes]] — File Store is one of the scope-controlled components
- [[bulk-operations]] — Bulk write uses Stratus CSV, not File Store

## Evolution

- **Initial**: File Store was the only file storage option in Catalyst
- **2024+**: Stratus introduced as a major upgrade with multipart uploads, versioning, presigned URLs
- **Current**: Catalyst docs note Stratus as the recommended path; File Store deprecation timeline unknown
