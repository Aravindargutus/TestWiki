---
title: Function Cost & Performance Optimization
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [catalyst-serverless-functions.md]
tags: [functions, pricing, credits, memory, optimization]
---

# Function Cost & Performance Optimization

## Definition

Catalyst [[serverless-functions]] are billed in **credits** computed from execution time and provisioned memory. Tuning memory can reduce credits for compute-heavy functions even though higher memory costs more per second. [Source: [Cost and Performance Optimization](https://docs.catalyst.zoho.com/en/serverless/help/functions/optimization/)]

## Pricing Model

### Credit Formula
```
Credits per execution = Time-credits × Memory-credits
Total cost = Number of invocations × Time-credits × Memory-credits
```

### Execution Time Tiers

| Execution time (seconds) | Time-credits |
|---|---|
| < 1 | 0.5 |
| 1 – 5 | 1 |
| 5 – 10 | 5 |
| 10 – 15 | 10 |
| 15 – 30 | 15 |

### Memory Tiers

| Memory (MB) | Memory-credits |
|---|---|
| 128 | 1 |
| 256 | 2 |
| 512 | 4 |

### Worked Examples (from docs)
- 4s @ 128 MB → `1 × 1 × 1` = **1 credit**
- 8s @ 256 MB → `1 × 5 × 2` = **10 credits**
- 0.5s @ 512 MB → `1 × 0.5 × 4` = **2 credits**

## Key Insights

### Compute-heavy functions: higher memory saves credits
Experiment from docs: a bubble sort of 1–10,000 took **6,994 ms @ 128 MB** (5 credits) vs **3,391 ms @ 512 MB** (4 credits). At 10,000 invocations/month that's **10,000 credits/month saved** by bumping memory.

### Lightweight functions: higher memory just wastes credits
For I/O-bound or trivial functions, memory-credits scale linearly with no execution-time benefit — credit cost becomes directly proportional to memory.

## Optimization Strategy

1. Start at the **lowest memory tier (128 MB)**
2. Use [Catalyst Logs] and [APM] to measure execution time
3. If the function is compute-intensive and hitting upper time tiers, try 256 MB and 512 MB
4. Pick the tier where `Time-credits × Memory-credits` is minimized for your workload
5. Re-tune when code or data volumes change significantly

## Related Constraints

- **Job Functions** (see [[job-functions]]): function memory must be **less than** the job pool memory to avoid dispatch delays
- **Execution time caps**: 30s for HTTP-facing functions; 15 min for background functions — see [[serverless-functions]]

## Sources

- [[catalyst-serverless-functions]] — Cost and Performance Optimization sub-page

## Related Concepts

- [[serverless-functions]] — Parent concept
- [[runtime-support-policy]] — Runtime choice also affects performance characteristics
- [[job-functions]] — Has an additional memory-vs-pool constraint
