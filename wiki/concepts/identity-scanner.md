---
title: Identity Scanner
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-zia-services.md]
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

## Sources

- [[catalyst-java-sdk-zia-services]]

## Related Concepts

- [[zia-services]] — Parent category
- [[text-analytics]] — Another Zia sub-suite (NLP vs identity)

## Evolution

- Facial Comparison was initially IN DC-only for console testing; SDK/API access was later opened globally.
- Language code parameter for Aadhaar is being deprecated in favor of auto-detection.
