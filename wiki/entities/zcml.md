---
title: ZCML
type: entity
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-zia-services.md]
tags: [sdk, java, zia, ml, class]
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

## Appearances

- [[catalyst-java-sdk-zia-services]] — All Zia Services SDK operations

## Relationships

- Part of: [[zoho-catalyst]] Zia Services component
- Follows: [[instance-objects]] pattern (singleton)

## Notes

- Unlike [[zcfile]], [[zccache]], [[zcstratus]] etc. which are per-component entry points, ZCML is a unified entry point for ALL Zia services.
