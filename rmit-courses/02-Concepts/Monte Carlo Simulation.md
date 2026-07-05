---
title: Monte Carlo Simulation
tags: [concept]
status: draft
related_documents:
  - "[[Introduction to Monte Carlo Simulation]]"
date_added: 2026-07-05
---

## Definition
> A dynamic modeling method that replaces fixed input estimates with thousands of random values drawn from probability distributions, runs a formulation once per drawn scenario, and analyzes the resulting thousands of outputs statistically.

## Intuition
> Instead of asking "what is the answer if my inputs are exactly right?", MCS asks "what is the spread of answers across all plausible inputs?" — turning a single fragile point estimate into a distribution with measured confidence and margin of error.

## How It Works
Generic 4-step process (per the course intro): (1) build a static formulation and identify input variables, constants, and output variables; (2) identify a [[Probability Distribution]] and its parameters for each input variable; (3) generate 1000s of scenarios with randomly drawn input values; (4) analyze the 1000s of outputs statistically. A detailed 8-step process follows in Section 4 of the course.

## Variants & Depth
Stub — expand as the course progresses (distribution fitting, run counts, confidence quantification, sensitivity analysis). Contrast with [[Discrete Event Simulation]] for workflows/event networks.

## Key Documents
- [[Introduction to Monte Carlo Simulation]]

## Related Concepts
- [[Probability Distribution]]
- [[Discrete Event Simulation]]

## My Notes
