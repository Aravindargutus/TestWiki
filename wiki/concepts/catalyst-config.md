---
title: catalyst-config.json
type: concept
created: 2025-07-10
updated: 2025-07-10
sources: [catalyst-serverless-functions.md]
tags: [configuration, functions, serverless, catalyst]
---

# catalyst-config.json

## Definition

`catalyst-config.json` is the universal configuration file for all [[serverless-functions]] in [[zoho-catalyst]], present in every function directory regardless of runtime (Java, Node.js, Python). It contains function metadata, type designation, and runtime settings. [Source: catalyst-serverless-functions.md]

## Key Aspects

- Present in every function's root directory
- Required for all 7 function types
- Contains function type, runtime version, and other metadata
- Part of the standard directory structure alongside language-specific files:
  - **Java**: main `.java` + `lib/` + `catalyst-config.json`
  - **Node.js**: main `.js` + `package.json` + `node_modules/` + `catalyst-config.json`
  - **Python**: `main.py` + `requirements.txt` + `catalyst-config.json`

## Sources

- [[catalyst-serverless-functions]] — Help documentation

## Related Concepts

- [[serverless-functions]] — Functions that use this configuration
