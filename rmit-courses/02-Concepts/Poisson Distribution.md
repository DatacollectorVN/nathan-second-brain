---
title: Poisson Distribution
tags: [concept]
status: draft
related_documents:
  - "[[Statistics and Probability Foundation]]"
date_added: 2026-07-05
---

## Definition
> A one-parameter discrete distribution over the non-negative integers:
> $$P(X = x) = \frac{e^{-\theta}\,\theta^{x}}{x!}, \quad x = 0, 1, 2, \dots$$
> with mean $\mu = \theta$ and variance $\sigma^{2} = \theta$ (the parameter is often written $\lambda$).

## Intuition
> The distribution of counts of "successes" occurring at random in time or space — arrivals per period, accidents, goals, particles hitting a target. Counting historical arrivals per period yields the rate that parameterizes the whole distribution.

## How It Works
Under the Poisson Process assumptions (independent counts in disjoint intervals, distribution depending only on interval length, no simultaneous successes), the count in a window of length $w$ is Poisson with $\theta = \lambda w$. It approximates the binomial when $n$ is large and $p$ small ($\theta = np$), and sums of independent Poissons are Poisson: $\sum_i X_i \sim \text{Poisson}\!\left(\sum_i \theta_i\right)$. For large $\theta$ ($\theta \ge 25$) it is approximately bell-shaped and a Normal approximation with continuity correction applies: $z = \frac{x - 0.5 - \theta}{\sqrt{\theta}}$.

## Variants & Depth
Extends to 2-D/3-D ($\theta$ = rate × area or volume). Mean = variance ($\mu = \sigma^{2} = \theta$) is the classic overdispersion check before assuming Poisson. Used in the MCS course for arrival modeling (e.g. petrol-station vehicles at rate 3.0/period).

## Key Documents
- [[Statistics and Probability Foundation]]

## Related Concepts
- [[Probability Distribution]]
- [[Random Variable]]
- [[Normal Distribution]]

## My Notes
