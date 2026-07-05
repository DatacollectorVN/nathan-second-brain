---
title: Random Variable
tags: [concept]
status: draft
related_documents:
  - "[[Statistics and Probability Foundation]]"
date_added: 2026-07-05
---

## Definition
> A random variable X measures the value of a randomly occurring event, where the value can be a count (discrete) or a fractional measurement (continuous); its values are mutually exclusive.

## Intuition
> An extension of the algebra "unknown X": instead of one fixed unknown amount, X ranges over outcomes of a random event — cones bought per customer, defects per motherboard, minutes to complete a procedure. Collecting many observations of X and tabulating frequencies reveals its distribution.

## How It Works
Discrete X's (counts) give a Probability Mass Function; continuous X's (measurements) give a Probability Density Function, usually after binning the raw data. Relative frequency of each value = its probability = the expectation of seeing it in future observations. See [[Probability Distribution]].

## Variants & Depth
Discrete vs continuous; whether a measured quantity is treated as discrete or continuous depends on the precision needed (bracketing/binning converts between views). Central to [[Monte Carlo Simulation]] input modeling.

## Key Documents
- [[Statistics and Probability Foundation]]

## Related Concepts
- [[Probability Distribution]]
- [[Normal Distribution]]
- [[Poisson Distribution]]

## My Notes
