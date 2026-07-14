---
title: Strong Convexity
tags: [concept, certified]
status: draft
related_papers:
  - "[[Certified Data Removal from Machine Learning Models]]"
  - "[[Understanding Black-box Predictions via Influence Functions]]"
date_added: 2026-07-13
---

## Definition
> A function $f$ is $\mu$-strongly convex if it is lower-bounded everywhere by a quadratic: curvature of at least $\mu > 0$ in every direction.

## Intuition
Ordinary convexity only guarantees the function sits above its tangent lines (which can be nearly flat). Strong convexity adds a quadratic bowl underneath: there is always a restoring force pulling toward the minimizer, growing linearly with distance. Consequences: the minimizer is unique, gradients far from it are provably large, and a bound on the gradient norm converts into a bound on distance from the optimum — which is exactly what certified unlearning needs.

## How It Works
Three equivalent characterizations (for twice-differentiable $f$):

$$f(y) \ge f(x) + \nabla f(x)^\top (y-x) + \frac{\mu}{2}\lVert y-x \rVert_2^2 \quad \forall x,y$$

**What each symbol means:** $\mu > 0$ is the strong-convexity constant; the last term is the quadratic lower bound missing from plain convexity.
**Logic:** the function grows at least quadratically away from any tangent point — no flat valleys.

$$\nabla^2 f(x) \succeq \mu I \quad \forall x$$

**What each symbol means:** every eigenvalue of the Hessian is at least $\mu$; $I$ is the identity.
**Logic:** minimum curvature $\mu$ in every direction. This is the form used in unlearning proofs: an $L_2$ regularizer $\frac{\lambda n}{2}\lVert w\rVert^2$ adds $\lambda n I$ to the Hessian, *manufacturing* strong convexity even if the data loss alone is only convex.

$$f(x) - f(x^*) \le \frac{1}{2\mu}\lVert \nabla f(x) \rVert_2^2 \qquad \text{and} \qquad \lVert x - x^* \rVert_2 \le \frac{1}{\mu}\lVert \nabla f(x) \rVert_2$$

**What each symbol means:** $x^*$ is the unique minimizer (the first is the Polyak–Łojasiewicz inequality).
**Logic:** a small gradient certifies you are close to the optimum — this is the bridge certified removal crosses: bound the post-Newton-step gradient residual, and strong convexity converts it into "the unlearned model is provably near the true retrained model," a residual small enough for noise to mask.

Paired with $M$-smoothness ($\nabla^2 f \preceq M I$), the condition number $\kappa = M/\mu$ controls optimization difficulty ("the quadratic sandwich"); gradient descent converges linearly, in $O(\kappa \log(1/\epsilon))$ iterations.

## Variants & Evolution
- Plain convexity ($\mu = 0$): minimizers may be non-unique; certified-removal bounds degrade.
- PL inequality: weaker than strong convexity but preserves linear convergence; a candidate relaxation for non-convex unlearning analyses.
- Deep nets: $H$ is indefinite (negative eigenvalues), $\hat\theta$ non-unique — the certificate breaks; damping $H + \lambda I$ ([[Understanding Black-box Predictions via Influence Functions]], §4.2) fakes local strong convexity without a guarantee.

## Key Papers
- [[Certified Data Removal from Machine Learning Models]] — strong convexity via $L_2$ regularization drives every bound
- [[Understanding Black-box Predictions via Influence Functions]] — PD Hessian assumption; damping when it fails

## Related Concepts
- [[Influence Functions]]
- [[Certified Removal]]
- [[Sensitivity]]

## My Notes
### Reading list (curated 2026-07-13)
Intuition first: [The Quadratic Sandwich](https://fedemagnani.github.io/math/2026/04/08/the-quadratic-sandwich.html) (strong convexity + smoothness + condition number), then [Xingyu Zhou — Strong convexity](https://xingyuzhou.org/blog/notes/strong-convexity) (six equivalent definitions with proofs), and [Bubeck — ORF523: Strong convexity](https://blogs.princeton.edu/imabandit/2013/04/04/orf523-strong-convexity/).
Rigor: Boyd & Vandenberghe, *Convex Optimization*, §9.1.2 ([free PDF](https://web.stanford.edu/~boyd/cvxbook/)); [Cambridge L23-III OPT Lecture 3](https://www.damtp.cam.ac.uk/user/hf323/L23-III-OPT/lecture3.pdf); [IFT 6085 Lecture 3 (Mila)](https://mitliagkas.github.io/ift6085-2020/ift-6085-lecture-3-notes.pdf); [CMU 10-725](https://www.cs.cmu.edu/~ggordon/10725-F12/scribes/10725_Lecture5.pdf); [UBC CPSC 540](https://www.cs.ubc.ca/~schmidtm/Courses/540-W19/L5.pdf).
Bridge to unlearning: [Guo et al. 2020, §3](http://proceedings.mlr.press/v119/guo20c/guo20c.pdf); contrast with [Rewind-to-Delete (nonconvex)](https://arxiv.org/html/2409.09778) and [Certified Unlearning via Noisy SGD](https://arxiv.org/html/2403.17105v2).
