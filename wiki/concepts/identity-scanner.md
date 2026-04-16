---
title: Identity Scanner
type: concept
created: 2026-04-06
updated: 2026-04-16
sources: [catalyst-java-sdk-zia-services.md, catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]
tags: [zia, identity, ekyc, document-processing]
---

# Identity Scanner

## Definition

Identity Scanner is a [[zia-services]] component for secure identity checks on individuals and documents. It has two categories: E-KYC (Facial Comparison — globally available) and Document Processing (Aadhaar, PAN, Passbook, Cheque — India DC only). [Source: catalyst-java-sdk-zia-services.md]

## Key Aspects

- **E-KYC (Facial Comparison)**: Compare two face images → boolean match + confidence score. Globally available. 10MB limit.
- **Document Processing** (IN DC only):
  - **Aadhaar**: Front + back images, extracts name/address/gender/number. 15MB. Auto-language detection coming.
  - **PAN**: Front image, extracts name/PAN number/DOB. English only. 15MB.
  - **Passbook**: Front page image, extracts bank/account/branch details + RTGS/NEFT/IMPS flags. 15MB.
  - **Cheque**: CTS-2010 format only, extracts account/IFSC/bank/amount/date. English only. 15MB.
- **Shared OCR foundation**: PAN, Passbook, Cheque all use `getContent(file, options)` with different `ZCOCRModelType` values. Aadhaar has its own method `getContentForAadhaar()`.
- **Confidence scores**: All responses include accuracy scores (0-1 range)

## Node.js SDK Access Pattern

```js
const zia = app.zia();

// Facial Comparison (global)
await zia.compareFaces(fs.createReadStream('./face1.jpg'), fs.createReadStream('./face2.jpg'));

// Document Processing (IN DC only)
await zia.extractOpticalCharacters(fs.createReadStream('./doc.jpg'), { modelType: 'AADHAAR' });
await zia.extractOpticalCharacters(fs.createReadStream('./pan.jpg'), { modelType: 'PAN' });
await zia.extractOpticalCharacters(fs.createReadStream('./passbook.jpg'), { modelType: 'PASSBOOK' });
await zia.extractOpticalCharacters(fs.createReadStream('./cheque.webp'), { modelType: 'CHEQUE' });
```

**Key difference**: Java has a dedicated `getContentForAadhaar(front, back, lang)` method. In Node.js, all document types (including Aadhaar) use the same `extractOpticalCharacters()` with a `modelType` option. Cheque scanner supports `.webp`, `.jpeg`, `.png` (max 15MB), CTS-2010 format, English only. [Source: catalyst-nodejs-sdk-zia-smartbrowz-jobs.md]

## Sources

- [[catalyst-java-sdk-zia-services]]
- [[catalyst-nodejs-sdk-zia-smartbrowz-jobs]] — Node.js Identity Scanner (5 pages)

## Related Concepts

- [[zia-services]] — Parent category
- [[text-analytics]] — Another Zia sub-suite (NLP vs identity)

## Evolution

- Facial Comparison was initially IN DC-only for console testing; SDK/API access was later opened globally.
- Language code parameter for Aadhaar is being deprecated in favor of auto-detection.
