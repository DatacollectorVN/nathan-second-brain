---
title: Uninformed vs Informed Search — Strategy and Algorithm Comparison
type: critical-review
status: reviewed
date_added: 2026-07-08
reviewer: AI-RMIT-Teacher (Critical Reviewer)
covers:
  - "[[AI Lecture 02 — Solving Problems by Searching]]"
  - "[[Breadth-First Search]]"
  - "[[Uniform-Cost Search]]"
  - "[[Depth-First Search]]"
  - "[[Depth-Limited Search]]"
  - "[[Iterative Deepening Search]]"
  - "[[Greedy Best-First Search]]"
  - "[[A* Search]]"
tags:
  - critical-review
  - course/ai
---

## Question / Thesis

When should you reach for an uninformed (blind) search strategy versus an informed (heuristic) one — and within each family, which algorithm wins under which conditions? This is the core exam-relevant comparison from Week 2 of the AI course.

## Compared Items

**Families:**
- Uninformed search: [[Breadth-First Search]], [[Uniform-Cost Search]], [[Depth-First Search]], [[Depth-Limited Search]], [[Iterative Deepening Search]] (+ bidirectional search, covered only in the lecture note)
- Informed search: [[Greedy Best-First Search]], [[A* Search]] (both built on the [[Heuristic Function]] concept)

## Executive Judgment

The uninformed/informed split is really a split over *what the algorithm knows*: uninformed strategies rank the frontier only by properties of the path already travelled (depth or g(n)); informed strategies add an estimate h(n) of the cost still to come. Because every algorithm in the lecture is literally the same generic search procedure with a different queuing function, the whole comparison reduces to "what orders the frontier, and what guarantees does that ordering buy?" Within uninformed search, [[Iterative Deepening Search]] is the lecture's stated default (BFS-grade completeness/optimality at DFS-grade O(bd) space); within informed search, [[A* Search]] dominates [[Greedy Best-First Search]] whenever an admissible heuristic exists, since it keeps greedy's goal-directedness while restoring uniform-cost's optimality guarantee. Informed search is strictly the better family *when a decent heuristic is available* — its only real cost is having to design one.

## Evidence Base
- Source notes reviewed:
  - [[AI Lecture 02 — Solving Problems by Searching]] (course: AI) — **status: reviewed** (2026-07-08, verified against the 73-slide source PDF; Drive archive pending)
  - [[Breadth-First Search]], [[Uniform-Cost Search]], [[Depth-First Search]], [[Depth-Limited Search]], [[Iterative Deepening Search]], [[Greedy Best-First Search]], [[A* Search]] (all 03-Architectures/, course: AI) — **all status: reviewed** (2026-07-08, slide-by-slide verification)
  - [[Heuristic Function]] (02-Concepts/, shared) — **status: reviewed** (2026-07-08)
- Key documents / lectures: AI Lecture 02 (COSC2129/1476, Week 2), Romania route-finding, Missionaries & Cannibals, 8-Puzzle worked examples
- Labs / implementation evidence: none yet in 04-Experiments/

> ✅ 2026-07-08: all sources in this evidence base passed the Reviewer's faithfulness gate against `AI-Lec02 Search_.pdf`, so this review is promoted from draft to reviewed. Two minor advisory flags from that review (slide 71's ambiguous relaxation labelling for h1; DLS "diameter" example being an R&N addition) do not affect the comparisons made here.

## The Structural Difference (both families share one skeleton)

Per the lecture, every algorithm is the generic search procedure — initialise frontier, pop a leaf per strategy, goal-test, expand — and "search algorithms differ only in their queuing function":

| | Uninformed | Informed |
|---|---|---|
| Frontier ordered by | depth (FIFO/LIFO) or path cost g(n) | heuristic h(n), alone or as f(n) = g(n) + h(n) |
| Knowledge used | how far from the *start* | + estimate of how far to the *goal* |
| Extra design burden | none | must design an admissible/consistent [[Heuristic Function]] |
| Scaling behaviour | exponential in d or m, unavoidably | exponential worst case, but a good h prunes dramatically |

## Within Uninformed Search — algorithm vs algorithm

Source table (from [[AI Lecture 02 — Solving Problems by Searching]]; b = branching factor, d = shallowest goal depth, m = max path length, l = depth limit, C* = optimal cost, ε = min step cost):

| Criteria | [[Breadth-First Search\|BFS]] | [[Uniform-Cost Search\|UCS]] | [[Depth-First Search\|DFS]] | [[Depth-Limited Search\|DLS]] | [[Iterative Deepening Search\|IDS]] |
|---|---|---|---|---|---|
| Frontier | FIFO queue | priority queue by g(n) | stack (LIFO) | stack + bound l | DLS with l = 0,1,2,… |
| Complete | Yes (b finite) | Yes | No | No | Yes |
| Optimal | Yes (equal step costs) | Yes | No | No | Yes (equal step costs) |
| Time | O(b^d) | O(b^⌈C*/ε⌉) | O(b^m) | O(b^l) | O(b^d) |
| Space | O(b^d) | O(b^⌈C*/ε⌉) | O(bm) | O(bl) | O(bd) |

Head-to-head reading of that table:

- **BFS vs DFS** — the classic guarantee-vs-memory trade. BFS buys completeness and (equal-cost) optimality at O(b^d) *space*, which is what actually kills it in practice; DFS's O(bm) space is its only virtue — it can dive down an m ≫ d branch, loop on cycles, and return a non-optimal goal.
- **UCS vs BFS** — UCS is BFS generalised to unequal step costs (it reduces to BFS exactly when g(n) = depth). It keeps optimality where BFS loses it, at the price of complexity governed by C*/ε rather than d. Its goal-test-on-pop (not on generation) is precisely what guarantees optimality — an exam-worthy detail.
- **DLS vs DFS** — DLS just bounds DFS at depth l to guarantee termination, introducing the distinct "cutoff" failure (solution may exist deeper) vs genuine failure. The lecture gives no concrete method for choosing l beyond problem knowledge — a flagged gap.
- **IDS vs everyone** — IDS reruns DLS with l = 0,1,2,… and gets BFS's completeness/optimality with DFS's O(bd) space. The re-expansion of shallow levels looks wasteful but isn't (lecture's worked figures, b=10, d=5: 123,456 IDS expansions vs 111,111 for BFS — most nodes live at the bottom level). The lecture's verdict, quoted: "in general, the preferred uninformed search method when the search space is large and the solution depth is unknown."
- **Bidirectional search** — potentially very fast (meet in the middle), but per the lecture problematic with many goal states and needs an efficient frontier-intersection check. No standalone architecture note exists for it yet.

## Within Informed Search — Greedy vs A*

Both rank a priority-queue frontier using a [[Heuristic Function]]; they differ in one term:

| Feature | [[Greedy Best-First Search]] | [[A* Search]] |
|---|---|---|
| Ranking | h(n) only — ignores g(n) entirely | f(n) = g(n) + h(n) |
| Analogy in uninformed family | heuristic DFS (chases what looks closest) | UCS + heuristic term |
| Complete | Only if repeated states removed (can loop) | Yes, if h consistent |
| Optimal | No | Yes — admissible h (tree-search) / consistent h (graph-search) |
| Romania (Arad→Bucharest) result | Arad–Sibiu–Fagaras–Bucharest, cost **450** | Arad–Sibiu–Rimnicu Vilcea–Pitesti–Bucharest, cost **418** (true optimum) |

The Romania example is the whole argument in one trace: greedy is lured toward Fagaras because it *looks* closest, having spent no thought on cost already incurred; A* folds g(n) back in and correctly prefers Rimnicu Vilcea. Heuristic quality then determines efficiency: on the 8-Puzzle, Manhattan-distance h2 dominates misplaced-tiles h1 (h2 ≥ h1, both admissible from problem relaxation), so A* with h2 expands fewer nodes.

## What Each Does Well

- **Uninformed family**: zero design burden — works on any well-formulated [[Search Problem]] with no domain knowledge; IDS in particular is a safe default.
- **Informed family**: converts domain knowledge into pruning; A* is the only algorithm in the lecture that is simultaneously complete, optimal *and* goal-directed.

## Failure Modes / Risks

- DFS/DLS: incomplete and non-optimal; DFS loops on cycles without repeated-state checking; DLS wastes the whole search if l < d.
- BFS/UCS/A*: all keep the frontier (and explored set) in memory — space, not time, is usually the binding constraint.
- Greedy: can loop (incomplete without repeated-state removal) and confidently returns suboptimal paths.
- A*: guarantees are *conditional* — an inadmissible h silently voids optimality; a weak h degrades A* toward UCS behaviour/cost.
- Whole lecture scope: single-agent, fully observable, deterministic, finite spaces only — none of this transfers directly to chance, hidden state, infinite spaces, or adversaries.

## Tradeoff Matrix

| Item | Strength | Weakness | Best Fit | Avoid When |
|------|----------|----------|----------|------------|
| [[Breadth-First Search]] | Complete; optimal (equal costs); simple | O(b^d) space | Shallow solutions, small b | Deep solutions / large spaces (memory) |
| [[Uniform-Cost Search]] | Optimal with any step costs | O(b^⌈C*/ε⌉) time+space; blind | Weighted graphs, no heuristic available | Good heuristic exists (use A*) |
| [[Depth-First Search]] | O(bm) space | Incomplete, non-optimal, O(b^m) time | Memory-bound, solutions known deep, all-solutions traversal | Cycles/infinite branches; optimality needed |
| [[Depth-Limited Search]] | Terminates; O(bl) space | Cutoff failure if l < d; no recipe for picking l | Known depth bound (e.g. state-space diameter) | Solution depth unknown (use IDS) |
| [[Iterative Deepening Search]] | Complete + optimal + O(bd) space | Still O(b^d) time; re-expands shallow nodes | Large space, unknown solution depth — the lecture's default | Step costs unequal (use UCS/A*) |
| [[Greedy Best-First Search]] | Fast to a goal with good h | Incomplete (loops), never guaranteed optimal | Quick feasible path, optimality irrelevant | Path cost matters at all |
| [[A* Search]] | Complete + optimal + heuristic pruning | Exponential space; guarantees hinge on h | Optimal paths with a known admissible h | No decent heuristic; memory-bound |

## Engineering Takeaways

- The frontier data structure *is* the algorithm: FIFO → BFS, LIFO → DFS, priority-by-g → UCS, priority-by-h → Greedy, priority-by-f → A*. Implement one generic search loop and swap the queue.
- Always add the explored-set / repeated-state check (hash table) — it's what keeps DFS and Greedy from looping and is cheap insurance everywhere.
- Goal-test on pop, not on generation, for UCS and A* — testing early breaks optimality.
- Heuristic design recipe: relax the problem, use the relaxed solution cost as h. Prefer the dominating admissible heuristic (higher h everywhere) — it strictly reduces node expansions.
- Memory is the practical bottleneck for BFS/UCS/A* — a judgment worth carrying into any real implementation.

## Exam / Assignment Implications

- Reproduce the five-column performance table (Complete/Optimal/Time/Space for BFS, UCS, DFS, DLS, IDS) from memory, with b/d/m/l/C*/ε notation.
- State admissible vs consistent precisely, and *why* graph-search A* needs consistency while tree-search A* only needs admissibility.
- Hand-trace Greedy vs A* on the Romania map (450 vs 418) — the lecture's canonical worked example and the likeliest tutorial/exam question.
- Be ready to justify "IDS is the preferred uninformed method" and "h2 dominates h1 on the 8-Puzzle" with the reasoning, not just the claim.

## My View

*(Judgment, not lecture content.)* The cleanest mental model is a two-axis map: what the ranking function knows (g only / h only / g+h) × what guarantee you need (any solution / optimal solution). UCS and Greedy are each half of A* — UCS is f = g + 0, Greedy is f = 0 + h — which makes A* less a third algorithm than the completion of the other two, and explains both tables at once. I'd also push back slightly on reading "informed > uninformed" as absolute: the informed family's superiority is entirely contingent on heuristic quality, and the lecture's own framing (A* degrades toward UCS as h weakens) means uninformed search is really the h = 0 limiting case of informed search, not a separate species. Finally, the shared weakness the lecture underplays: *every* complete-and-optimal method shown (BFS, UCS, IDS aside, A*) pays exponential space — memory-bounded variants exist beyond this week's scope, and I'd expect them to matter for any real assignment.

## Open Questions

- How is the depth limit l for [[Depth-Limited Search]] chosen in practice? The lecture says only "based on knowledge of the problem" — no method given.
- The slides assert "almost any" admissible heuristic is also consistent without proof — under what conditions does admissible-but-inconsistent actually occur, and what breaks in graph-search A* when it does?
- When are the flagged out-of-scope settings (chance, hidden state, infinite spaces, adversarial games) covered, and which of these algorithms survive the transition? [not yet in the vault]
- Bidirectional search has no architecture note — worth creating one if it recurs in tutorials or the exam.

## Related

- [[AI Lecture 02 — Solving Problems by Searching]]
- [[Heuristic Function]]
- [[Search Problem]]
- [[State Space Search]]
- [[Admissible Heuristic]]
- [[Consistent Heuristic]]
- [[Evaluation Function]]
- [[Frontier]]
- [[Explored Set]]
