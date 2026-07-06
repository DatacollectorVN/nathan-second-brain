---
title: Differential Privacy
tags: [concept, privacy, certified]
status: reviewed
related_papers:
  - 
date_added: 2026-07-06
---

## Definition
> A mechanism is **differentially private** if its output distribution is almost unchanged when any single record is added to or removed from the input dataset — so an adversary observing the output cannot confidently tell whether any given individual was in the data.

Formally, for neighboring datasets $D \sim D'$ (differing in exactly one record) and any output set $S$, a randomized mechanism $\mathcal{A}$ is $\varepsilon$-DP if:

$$P[\mathcal{A}(D)\in S] \le e^{\varepsilon}\cdot P[\mathcal{A}(D')\in S].$$

The relaxation $(\varepsilon,\delta)$-DP adds a slack term:

$$P[\mathcal{A}(D)\in S] \le e^{\varepsilon}\cdot P[\mathcal{A}(D')\in S] + \delta.$$

## Intuition
Whatever the algorithm outputs, it's *nearly as likely* with your record in the data as without it. If the presence or absence of any one person barely moves the output distribution, that person's participation is hidden — no matter what side information an attacker holds. DP is a property of the **mechanism**, not of any particular output or dataset.

## How It Works

**Term-by-term breakdown of the formula:**

- **Neighboring datasets $D \sim D'$** — two datasets identical except for one record (one individual's data added, removed, or changed). DP reasons about exactly this single-record change, because that individual is who it protects.
- **Mechanism $\mathcal{A}$** — any *randomized* algorithm mapping a dataset to an output (a statistic, query answer, or trained model). Randomness is mandatory: a deterministic algorithm can't be DP, since its output would directly reveal which dataset was used.
- **Output set $S$** — because $\mathcal{A}(D)$ is a random variable, we bound the *probability the output lands in a set* $S$. The guarantee must hold for **every** subset $S$ of possible outputs.
- **The inequality** — the probability of any output is within a multiplicative factor of itself whether or not one person's record is present. By symmetry it also holds with $D$ and $D'$ swapped, so the two distributions are pinned within $e^\varepsilon$ of each other → outputs are (nearly) indistinguishable.
- **$\varepsilon$ (epsilon), the privacy budget** — the knob for how much the two probabilities may differ. Small $\varepsilon$ (e.g. 0.1 → $e^\varepsilon\approx 1.1$) = strong privacy, more noise, less accuracy. Large $\varepsilon$ (e.g. 5 → $e^5\approx 148$) = weak guarantee. Called a "budget" because it is spent as more queries are released.
- **Why $e^{\varepsilon}$ and not $\varepsilon$** — the exponential makes the guarantee multiplicative and additive in log-space: composing budgets $\varepsilon_1,\varepsilon_2$ gives $\varepsilon_1+\varepsilon_2$. That clean addition is why the bound is written with $e^\varepsilon$.
- **$\delta$ (the slack in $(\varepsilon,\delta)$-DP)** — a tiny probability (typically $\delta \ll 1/N$) that the clean $\varepsilon$-guarantee is allowed to fail entirely. Read it as: "with probability $\ge 1-\delta$ you get the $\varepsilon$ guarantee; with probability $\delta$ all bets are off." $\delta$ must be tiny because privacy can be fully broken in that failure event.

**Noise mechanisms**
- **Laplace noise** — exponential tails match the $e^\varepsilon$ ratio exactly → achieves *pure* $\varepsilon$-DP.
- **Gaussian noise** — lighter (faster-decaying) tails can't satisfy the pure multiplicative bound for every output; the rare extreme event is absorbed by $\delta$. So **Gaussian noise requires $(\varepsilon,\delta)$-DP**, and is preferred in ML (e.g. DP-SGD) because it composes better over many steps.

**Sensitivity — how noise is calibrated**
$$\Delta f = \max_{D\sim D'} \lVert f(D)-f(D')\rVert$$
The maximum a query's output can move when one record changes. Noise is scaled to it: Laplace $\propto \Delta f/\varepsilon$; Gaussian $\propto \Delta f/\varepsilon$ (times a $\delta$-dependent factor). High-sensitivity queries (max, sum) need more noise; averaging over many records keeps sensitivity — and thus noise — low.

## Variants & Evolution
- **Pure $\varepsilon$-DP** — strongest form, Laplace mechanism.
- **$(\varepsilon,\delta)$-DP** — approximate DP, enables Gaussian noise.
- **Rényi DP / zCDP** — tighter composition accounting for many-step training; the modern basis for DP-SGD privacy accounting.
- **Local DP** — noise added at the data source before collection (no trusted curator).

**Key properties**
- **Composition** — privacy loss accumulates predictably; total budget across mechanisms is (at most) $\sum_i \varepsilon_i$ (advanced composition gives tighter-than-linear bounds). This is what lets you budget privacy across many queries or training steps.
- **Post-processing immunity** — any function applied to a DP output stays DP with the same $\varepsilon$; you cannot degrade privacy by further analyzing or combining a released result with public data.
- **Group privacy** — if $k$ correlated records change, the guarantee degrades gracefully to $\approx k\varepsilon$, extending protection from individuals to groups at proportional cost.

## Key Papers
- Dwork & Roth, *The Algorithmic Foundations of Differential Privacy* (2014) — the canonical reference (Ch. 1–3 for the definitions above).
- Abadi et al., *Deep Learning with Differential Privacy* (2016) — DP-SGD and the moments accountant.

## Related Concepts
- [[Certified Removal]]
- [[Influence Functions]]
- [[Membership Inference]]
- [[Deletion Capacity]]

## My Notes
**Why this matters for unlearning.** Certified unlearning borrows DP's indistinguishability idea directly. DP asks: "is the output nearly identical whether or not record $x$ was in the training set?" Certified unlearning asks: "is the *unlearned* model nearly indistinguishable from a model *retrained without* $x$?" — the same $(\varepsilon,\delta)$ bound, applied to the distribution over **model parameters** after deletion instead of over query outputs. Guo et al.'s certified-removal framework is essentially DP's definition transplanted onto the deletion problem.
