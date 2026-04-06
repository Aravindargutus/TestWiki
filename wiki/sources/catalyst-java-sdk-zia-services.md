---
title: Catalyst Java SDK — Zia Services
type: source
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-zia-services.md]
tags: [sdk, java, zia, ai, ml, ocr, face-analytics, identity-scanner, text-analytics, image-moderation, object-recognition, barcode]
---

# Catalyst Java SDK — Zia Services

## Summary

This source covers all **15 Zia Services SDK pages**: OCR (1), AutoML (1), Face Analytics (1), Identity Scanner (5: Facial Comparison, Aadhaar, PAN, Passbook, Cheque), Text Analytics (4: Sentiment Analysis, NER, Keyword Extraction, All), Image Moderation (1), Object Recognition (1), Barcode Scanner (1). All Zia Services share the single entry point `ZCML.getInstance()`.

[Source: catalyst-java-sdk-zia-services.md]

## Key Takeaways

1. **Single entry point**: All Zia operations go through `ZCML.getInstance()` — no separate component classes per service.
2. **OCR** is the most versatile: 9 international + 10 Indian languages, supports .pdf, 20MB limit, reused by Identity Scanner (PAN, Passbook, Cheque use `getContent()` with model type).
3. **Identity Scanner** has two halves: E-KYC (Facial Comparison — globally available) and Document Processing (Aadhaar, PAN, Passbook, Cheque — **IN DC only**).
4. **AutoML** is US-only (not available in EU, AU, IN, JP, SA, CA).
5. **Text Analytics** offers three NLP features (Sentiment, NER, Keyword Extraction) individually or combined via `getTextAnalytics()`. Input limit: 1500 chars.
6. **Image Moderation** returns safe_to_use/unsafe_to_use prediction with confidence scores for racy, nudity, violence, gore, weapons, drugs.
7. **Object Recognition** identifies 80 object types with coordinates and confidence.
8. **Barcode Scanner** auto-detects format via `ZCBarcodeFormat.ALL`.
9. **Privacy-first**: Catalyst does not store uploaded files; one-time processing only, no ML training.
10. **Analysis modes**: Face Analytics and Image Moderation support BASIC/MODERATE/ADVANCED modes (ADVANCED default).

## SDK Method Reference

| Service | Method | Key Classes | Data Center Restrictions |
|---------|--------|-------------|------------------------|
| OCR | `getContent(file, options)` | ZCML, ZCContent, ZCOCROptions, ZCOCRModelType | None |
| AutoML | `predictData(modelId, json)` | ZCML, ZCAutomlPredictionData | US only |
| Face Analytics | `analyzeFace(file, options)` | ZCML, ZCFaceAnalysisData, ZCFaceAnalyticsOptions | None |
| Facial Comparison | `compareFace(source, query)` | ZCML, ZCFaceComparisonData | Global (console: IN only) |
| Aadhaar | `getContentForAadhaar(front, back, lang)` | ZCML, ZCContent | IN only |
| PAN | `getContent(file, options)` [PAN model] | ZCML, ZCPanData | IN only |
| Passbook | `getContent(file, options)` [PASSBOOK model] | ZCML, ZCContent | IN only |
| Cheque | `getContent(file, options)` [CHEQUE model] | ZCML, ZCChequeData | IN only |
| Sentiment Analysis | `getSentimentAnalysis(text, keywords)` | ZCML, ZCSentimentAnalysisData | None |
| NER | `getNERPrediction(text)` | ZCML, ZCNERData | None |
| Keyword Extraction | `getKeywordExtraction(text)` | ZCML, ZCKeywordExtractionData | None |
| All Text Analytics | `getTextAnalytics(text, keywords)` | ZCML, ZCTextAnalyticsData | None |
| Image Moderation | `moderateImage(file, options)` | ZCML, ZCImageModerateData | None |
| Object Recognition | `detectObjects(file)` | ZCML, ZCObjectDetectionData | None |
| Barcode Scanner | `scanBarcode(file, options)` | ZCML, ZCBarcodeData | None |

## Entities Mentioned

- [[zoho-catalyst]] — Platform
- [[zcml]] — ZCML singleton entry point for all Zia Services (new)

## Concepts Introduced

- [[zia-services]] — Umbrella concept for all Zia AI/ML features (new)
- [[identity-scanner]] — E-KYC + Document Processing suite (new)
- [[text-analytics]] — NLP: Sentiment + NER + Keyword Extraction (new)

## Open Questions

1. What NER categories are supported? (References help page list)
2. What are the exact confidence score thresholds for Image Moderation's safe/unsafe prediction?
3. How many languages does OCR support specifically? (Says 9 international + 10 Indian)
4. Does AutoML support custom model deployment or only Catalyst-trained models?
