---
title: Reproduce Efficient Long CoT on Llama-3.1-8B
tags: [experiment, distillation, SLM, planned]
status: planned
hypothesis: Binary cutting + on-policy validation can reduce CoT length by 50%+ while preserving >95% of MATH accuracy on a 7-8B SLM
date: 2025-06-13
---

## Goal
Reproduce the headline result of [[Efficient Long CoT Reasoning in Small Language Models]] on Llama-3.1-8B-Instruct using a small subset of OpenR1-Math-220k. Validate that the approach generalizes to my hardware/setup before exploring extensions.

## Setup
- **Model**: [[Llama-3.1-8B-Instruct]]
- **Teacher data**: OpenR1-Math-220k (use 5-10k subset to start)
- **Hardware**: TBD — likely Lambda Labs or RunPod A100 80GB
- **Framework**: HuggingFace TRL for SFT + DPO, vLLM for on-policy validation
- **Eval datasets**: GSM8K, MATH (500-sample subset), AIME 2024

## Method

### Phase 1 — Data pipeline
1. Load OpenR1-Math-220k, sample 5k math problems
2. For each problem with R1's full CoT:
   - Split thinking part into steps (use Qwen2.5-14B for segmentation as paper does)
   - Run **binary cutting** with backtracking on each sample
   - Validate each candidate prefix via **on-policy** prompt to Llama-3.1-8B
3. Output: pruned dataset ~1-2k samples

### Phase 2 — Training
1. **SFT** Llama-3.1-8B on pruned dataset (3 epochs, lr=1e-6)
2. **DPO** with chosen=pruned, rejected=original full CoT (1 epoch, β=0.1, SFT loss weight=0.3)

### Phase 3 — Evaluation
- Generate answers with `<think>` tags on each benchmark
- Measure: accuracy + average tokens inside `<think>` block
- Compare to: base model, full-CoT SFT baseline

## Expected Results (from paper Table 1)
| Metric | Base | Full CoT SFT | Ours (target) |
|--------|------|--------------|---------------|
| GSM8K Acc | 76.80% | 89.01% | ~87% |
| GSM8K Tokens | 232 | 1051 | ~340 |
| MATH Acc | 37.80% | 54.80% | ~52% |
| MATH Tokens | 2454 | 3274 | ~1320 |

## Risks & Open Questions
- On-policy validation requires many SLM inference calls — cost?
- Step segmentation quality affects everything downstream
- DPO might collapse without careful SFT-loss weighting
- Smaller training subset may hurt results

## Results
> TBD — fill in after running

## Observations
> TBD

## Conclusion
> TBD

## Next Steps
1. Reproduce baseline → confirm pipeline works
2. **Extension idea**: apply same approach to **tool-calling traces** instead of math reasoning — does pruning verbose tool-use chains help SLM agentic performance?
3. **Extension idea**: combine with [[LoRA]] for parameter-efficient distillation

## Related
- [[Efficient Long CoT Reasoning in Small Language Models]]
- [[Knowledge Distillation]]
- [[DPO]]
- [[On-Policy Learning]]
