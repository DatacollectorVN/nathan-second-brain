---
title: Normal Distribution
tags: [concept]
status: draft
related_documents:
  - "[[Statistics and Probability Foundation]]"
date_added: 2026-07-05
---

## Definition
> The Gauss ("bell curve") distribution: a continuous probability distribution completely defined by its mean and standard deviation, symmetric about the mean, extending to ± infinity, with mean = median = mode.

## Intuition
> Widely found in nature and business (heights, manufacturing tolerances, demand, scores). Spread is captured by the standard deviation: ±1/2/3 StDev around the mean contain 68.26% / 95.46% / 99.73% of the population.

## How It Works
Standardization Z = (X − Mean) / StDev maps any Normal population onto the standard Normal (mean 0, StDev 1), enabling cross-population comparison; reverse with X = Mean + Z·StDev. Cumulative areas under the curve answer P(X ≤ A); the inverse maps a probability back to X — the mechanism used to generate normally distributed random values in [[Monte Carlo Simulation]].

## Variants & Depth
Standard Normal (Z) vs general Normal; population vs sample standard deviation (divide by n vs n−1). Some ML methods assume normality (testable); underpins Six Sigma computations.

## Key Documents
- [[Statistics and Probability Foundation]]

## Related Concepts
- [[Probability Distribution]]
- [[Random Variable]]
- [[Poisson Distribution]]

## My Notes
