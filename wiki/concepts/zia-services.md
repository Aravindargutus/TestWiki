---
title: Zia Services
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-zia-services.md]
tags: [zia, ai, ml, cloud-scale]
---

# Zia Services

## Definition

Zia Services is Catalyst's AI/ML component suite powered by Zoho's Zia AI engine. It provides pre-built ML models for vision (OCR, face analytics, object recognition, image moderation, barcode scanning), NLP (sentiment analysis, NER, keyword extraction), identity verification (facial comparison, document processing), and custom ML (AutoML). All services are accessed via the single [[zcml]] entry point. [Source: catalyst-java-sdk-zia-services.md]

## Key Aspects

- **8 service categories, 15 SDK operations** via `ZCML.getInstance()`
- **Vision services**: OCR, Face Analytics, Image Moderation, Object Recognition, Barcode Scanner
- **NLP services**: Sentiment Analysis, NER, Keyword Extraction (+ combined All Text Analytics)
- **Identity services**: Facial Comparison (E-KYC), Aadhaar, PAN, Passbook, Cheque (Document Processing)
- **Custom ML**: AutoML — train and predict with classification/regression models
- **Privacy**: No file storage, one-time processing, no ML training on user data
- **Analysis modes**: BASIC/MODERATE/ADVANCED for Face Analytics and Image Moderation
- **Data center restrictions**: AutoML (US only), Document Processing (IN only), Facial Comparison (global)

## Sources

- [[catalyst-java-sdk-zia-services]]

## Related Concepts

- [[identity-scanner]] — E-KYC + Document Processing subset
- [[text-analytics]] — NLP subset (Sentiment + NER + Keywords)
- [[serverless-functions]] — Zia Services are typically called from within functions

## Evolution

- Zia is Zoho's AI engine used across the Zoho ecosystem. In Catalyst, it's exposed as pre-built services.
- Document Processing started India-only; Facial Comparison was later made globally available.
