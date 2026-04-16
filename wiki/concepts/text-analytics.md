---
title: Text Analytics
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-zia-services.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [zia, nlp, sentiment, ner, keywords]
---

# Text Analytics

## Definition

Text Analytics is a [[zia-services]] NLP component that processes textual content for three functions: Sentiment Analysis (tone detection), Named Entity Recognition (entity categorization), and Keyword Extraction (highlights/summary). All three can be run individually or combined in a single `getTextAnalytics()` call. [Source: catalyst-java-sdk-zia-services.md]

## Key Aspects

- **3 individual operations + 1 combined**:
  - `getSentimentAnalysis(text, keywords)` — positive/negative/neutral per sentence + overall
  - `getNERPrediction(text)` — extract entities with categories, locations, confidence
  - `getKeywordExtraction(text)` — extract keywords + keyphrases
  - `getTextAnalytics(text, keywords)` — all three in one call
- **Input limit**: 1500 characters per request
- **Optional keywords**: For Sentiment Analysis, filter to sentences containing specific keywords
- **Confidence scores**: All analyses include accuracy scores
- **No data center restrictions**: Available in all DCs

## Node.js SDK Access Pattern

```js
const zia = app.zia();

await zia.getSentimentAnalysis(['Text to analyze...'], ['optional', 'keywords']);
await zia.getNamedEntityRecognition(['Text with entities...']);
await zia.getKeywordExtraction(['Text to extract from...']);
await zia.getAllTextAnalytics(['Text...']);  // combined
```

Max 1500 characters. Sentiment response includes: `document_sentiment`, `sentence_analytics` (per-sentence sentiment + confidence), `overall_score`. [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

**Key difference**: Node.js text analytics methods accept **arrays** of strings, enabling batch processing. Java methods accept single string inputs.

## Sources

- [[catalyst-java-sdk-zia-services]]
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js Text Analytics (4 pages)

## Related Concepts

- [[zia-services]] — Parent category
- [[identity-scanner]] — Another Zia sub-suite (identity vs NLP)
