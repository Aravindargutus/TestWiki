---
title: Text Analytics
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-zia-services.md]
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

## Sources

- [[catalyst-java-sdk-zia-services]]

## Related Concepts

- [[zia-services]] — Parent category
- [[identity-scanner]] — Another Zia sub-suite (identity vs NLP)
