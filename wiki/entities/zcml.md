---
title: ZCML
type: entity
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-zia-services.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [sdk, java, nodejs, zia, ml, class]
---

# ZCML

## Overview

`ZCML` is the singleton entry point class for all [[zia-services]] operations in the Catalyst Java SDK. Unlike other SDK components which have separate entry point classes per service, all Zia AI/ML features are accessed through `ZCML.getInstance()`. [Source: catalyst-java-sdk-zia-services.md]

## Key Facts

- Package: `com.zc.component.ml.ZCML`
- Singleton via `ZCML.getInstance()`
- Handles 15 different operations across 8 Zia service categories
- File inputs are used for one-time processing only — never stored

## Methods

| Method | Service |
|--------|---------|
| `getContent(file, options)` | OCR, PAN, Passbook, Cheque |
| `getContentForAadhaar(front, back, lang)` | Aadhaar |
| `predictData(modelId, json)` | AutoML |
| `analyzeFace(file, options)` | Face Analytics |
| `compareFace(source, query)` | Facial Comparison |
| `getSentimentAnalysis(text, keywords)` | Sentiment Analysis |
| `getNERPrediction(text)` | NER |
| `getKeywordExtraction(text)` | Keyword Extraction |
| `getTextAnalytics(text, keywords)` | All Text Analytics |
| `moderateImage(file, options)` | Image Moderation |
| `detectObjects(file)` | Object Recognition |
| `scanBarcode(file, options)` | Barcode Scanner |

## Node.js SDK Equivalent

In Node.js, all Zia services are accessed via `app.zia()`. Methods accept ReadStreams for file inputs and return promises. [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

| Java SDK (`ZCML.getInstance()`) | Node.js SDK (`app.zia()`) |
|------|------|
| `getContent(file, options)` | `zia.extractOpticalCharacters(readStream, options)` |
| `getContentForAadhaar(front, back, lang)` | `zia.extractOpticalCharacters(stream, { modelType: 'AADHAAR' })` |
| `predictData(modelId, json)` | `zia.getRegression(modelId, inputData)` |
| `analyzeFace(file, options)` | `zia.detectFaces(readStream)` |
| `compareFace(source, query)` | `zia.compareFaces(stream1, stream2)` |
| `getSentimentAnalysis(text, keywords)` | `zia.getSentimentAnalysis(texts, keywords)` |
| `getNERPrediction(text)` | `zia.getNamedEntityRecognition(texts)` |
| `getKeywordExtraction(text)` | `zia.getKeywordExtraction(texts)` |
| `getTextAnalytics(text, keywords)` | `zia.getAllTextAnalytics(texts)` |
| `moderateImage(file, options)` | `zia.moderateImage(readStream)` |
| `detectObjects(file)` | `zia.detectObject(readStream)` |
| `scanBarcode(file, options)` | `zia.scanBarcode(readStream, { format })` |

**Key differences**: Node.js uses `fs.createReadStream()` for file inputs. Aadhaar scanner in Node.js uses the same `extractOpticalCharacters()` with `modelType` option instead of a separate method. Node.js text analytics methods accept arrays of strings.

## Appearances

- [[catalyst-java-sdk-zia-services]] — All Zia Services SDK operations
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js Zia Services via `app.zia()`

## Relationships

- Part of: [[zoho-catalyst]] Zia Services component
- Follows: [[instance-objects]] pattern (singleton)

## Notes

- Unlike [[zcfile]], [[zccache]], [[zcstratus]] etc. which are per-component entry points, ZCML is a unified entry point for ALL Zia services.
