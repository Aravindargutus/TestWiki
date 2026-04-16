---
title: ZCFile
type: entity
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-cloud-scale-remaining.md]
tags: [sdk, java, nodejs, file-store, class]
---

# ZCFile

## Overview

`ZCFile` is the entry point class for [[file-store]] operations in the Catalyst Java SDK. It provides access to folder instances for file upload, download, and delete operations. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Facts

- Package: `com.zc.component.files.ZCFile`
- Singleton via `ZCFile.getInstance()`
- Returns [[zcfolder]] instances by ID or name
- Being superseded by [[zcstratus]] (Stratus is the recommended upgrade)

## Methods

| Method | Description |
|--------|-------------|
| `getInstance()` | Get ZCFile singleton |
| `getFolderInstance(long id)` | Get folder by ID (no API call) |
| `getFolderInstance(String name)` | Get folder by name (no API call) |
| `getFolder(long id)` | Get folder details by ID (API call) |
| `getFolder(String name)` | Get folder details by name (API call) |
| `getFolder()` | List all folders (API call) |

## Node.js SDK Equivalent

In Node.js, File Store is accessed via `app.filestore()` → `filestore.folder(folderId)`. [Source: catalyst-nodejs-sdk-cloud-scale-remaining.md]

| Java SDK | Node.js SDK |
|----------|-------------|
| `ZCFile.getInstance()` | `app.filestore()` |
| `getFolderInstance(id)` / `getFolder(id)` | `filestore.folder(folderId)` |
| `folder.uploadFile(file)` | `folder.uploadFile({ code: stream, name: 'file.ext' })` |
| `folder.downloadFile(fileId)` | `folder.downloadFile(fileId)` |
| `folder.deleteFile(fileId)` | `folder.deleteFile(fileId)` |
| `getFolder()` (list all) | `folder.getDetails()` |

**Key difference**: Java `getFolderInstance` accepts both ID and name; Node.js `folder()` accepts only folderId. Node.js upload uses `{ code, name }` object instead of a File.

## Appearances

- [[catalyst-java-sdk-cloud-scale-remaining]] — File Store SDK operations
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js File Store SDK operations

## Relationships

- Returns: [[zcfolder]]
- Superseded by: [[zcstratus]]
- Part of: [[zoho-catalyst]] Cloud Scale components

## Notes

- Follows the same [[instance-objects]] pattern as other SDK entry points.
- File Store has a 100MB upload limit vs Stratus which supports multipart uploads for much larger files.
