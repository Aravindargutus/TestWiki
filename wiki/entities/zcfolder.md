---
title: ZCFolder
type: entity
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [sdk, java, file-store, class]
---

# ZCFolder

## Overview

`ZCFolder` is the folder class in the [[file-store]] component. All file operations (upload, download, delete) are performed through a folder instance. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Facts

- Package: `com.zc.component.files.ZCFolder`
- Obtained from [[zcfile]] via `getFolderInstance()` or `getFolder()`
- All file operations are scoped to a folder

## Methods

| Method | Description |
|--------|-------------|
| `uploadFile(File f)` | Upload a file (max 100MB) |
| `downloadFile(long fileId)` | Download a file, returns `InputStream` |
| `deleteFile(long fileId)` | Permanently delete a file (irreversible) |

## Appearances

- [[catalyst-java-sdk-cloud-scale-remaining]] — File Store SDK operations

## Relationships

- Accessed via: [[zcfile]]
- Part of: [[file-store]] component

## Notes

- Dev environment: 1GB storage per project. Production: unlimited.
- Deleted files cannot be restored.
