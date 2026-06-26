---
title: BLEU
tags: [glossary]
---

## Definition
> BLEU (Bilingual Evaluation Understudy) is an automatic metric for machine translation quality that scores a candidate translation by n-gram overlap with one or more human reference translations, with a brevity penalty for over-short output. Higher is better; reported on a 0–100 scale.

## Context
The standard headline metric in [[Attention Is All You Need]] and the NMT literature of the era. On WMT 2014 the Transformer big model reached 28.4 BLEU EN-DE and 41.8 BLEU EN-FR. A few BLEU points was a meaningful margin at the time, but BLEU correlates only loosely with human judgment and has since been complemented by metrics like COMET and chrF.

## See Also
- [[Attention Is All You Need]]
- [[Transformer]]
