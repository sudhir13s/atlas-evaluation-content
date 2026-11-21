---
id: benchmark/polymath
type: benchmark
slug: polymath
schema_version: 1.0
title: "PolyMath"
name: PolyMath
category: Multilingual
description: "A multilingual mathematical reasoning benchmark: the same problems in 18 languages, at four difficulty levels."
what_is_it: "PolyMath (Wang et al., 2025, Qwen team) has four difficulty levels (low, medium, high, top) spanning K-12 to Olympiad and advanced frontier mathematics. Each level has 125 problems, and every problem is available in 18 language versions, so each language has 500 problems. Translations are calibrated by language experts rather than taken directly from LLM output."
why_people_use_it: "Because the problems are parallel across languages, differences between languages show directly. The authors report that reasoning performance varies widely across languages and that input-output language consistency is low in reasoning LLMs (paper, 2025)."
examples:
  - "A problem from one of the four levels, given in each of the 18 languages; the model writes a solution and its final answer is checked."
method: "The model solves each problem and its final answer is checked. The benchmark score is Difficulty-Weighted Accuracy (DW-ACC): problems at the low, medium, high and top levels carry weights 1, 2, 4 and 8."
metric: "Difficulty-Weighted Accuracy (DW-ACC), per language and overall; accuracy per level is also reported."
format: "Free-response mathematics with a final answer; 125 problems per level per language."
limitations: "The language versions are translations of the same problems, so they test reasoning in each language rather than mathematics native to it. Under DW-ACC, a top-level problem counts eight times as much as a low-level one, so a few hard problems carry much of the score."
citation: "https://arxiv.org/abs/2504.18428"
worked_examples:
  - task_type: "Multilingual math — counting (Swahili)"
    input: "Ni nambari ngapi kamili chanya chini ya 100 zinazogawanywa na 3 au 5? (How many positive integers less than 100 are divisible by 3 or 5?)"
    success:
      answer: "Correct reasoning: multiples of 3 = 33, of 5 = 19, of 15 = 6, so 33 + 19 − 6 = 46 by inclusion–exclusion."
      why: "Applies inclusion–exclusion and subtracts the double-counted multiples of 15 — the same steps that succeed in English."
    failure:
      answer: "Wrong step: computes 33 + 19 = 52, omitting the overlap."
      why: "Skips the inclusion–exclusion correction and double-counts the six multiples of 15; the identical slip should not appear just because the language changed."
    checks: "Reasoning consistency across languages — the derivation must hold identically whatever language poses the problem."
    scoring: "Accuracy (% correct)."
  - task_type: "Multilingual math — number theory (Telugu)"
    input: "100! లో చివర ఎన్ని సున్నాలు ఉంటాయి? (How many trailing zeros does 100! have?)"
    success:
      answer: "Correct reasoning: count factors of 5 — ⌊100/5⌋ + ⌊100/25⌋ = 20 + 4 = 24 trailing zeros."
      why: "Trailing zeros come from factors of 5 (rarer than 2s); summing ⌊100/5⌋ and ⌊100/25⌋ counts them — the method is language-independent."
    failure:
      answer: "Wrong step: computes only ⌊100/5⌋ = 20 trailing zeros."
      why: "Forgets that multiples of 25 contribute a second factor of 5, undercounting by 4 — a reasoning slip, not a translation error."
    checks: "Applying Legendre's factor-counting fully, not a partial count, regardless of language."
    scoring: "Accuracy (% correct)."
  - task_type: "Multilingual math — combinatorics (Chinese)"
    input: "5个人围圆桌就座，有多少种不同的坐法？ (In how many distinct ways can 5 people sit around a round table?)"
    success:
      answer: "Correct reasoning: fix one person to remove rotations, giving (5−1)! = 24 arrangements."
      why: "Circular permutations divide out the 5 rotations, so (5−1)! = 24 — the reasoning holds in any language."
    failure:
      answer: "Wrong step: computes 5! = 120, treating the seats as a straight line."
      why: "Counts rotations of the same seating as distinct, overcounting by a factor of 5 — a reasoning error independent of the Chinese text."
    checks: "Recognizing rotational symmetry in circular arrangements across languages."
    scoring: "Accuracy (% correct)."
  - task_type: "Multilingual math — modular arithmetic (Arabic)"
    input: "ما باقي قسمة 3¹⁰⁰ على 5؟ (What is the remainder when 3^100 is divided by 5?)"
    success:
      answer: "Correct reasoning: powers of 3 mod 5 cycle as 3, 4, 2, 1 with period 4; since 100 ≡ 0 (mod 4), the value is the last in the cycle, 1."
      why: "Uses the length-4 cycle of 3 mod 5 and lands on 1 for an exponent divisible by the period."
    failure:
      answer: "Wrong step: reads 100 ≡ 0 (mod 4) as the start of the cycle and answers 3."
      why: "Misindexes the cycle — a multiple of the period maps to the last residue (1), not the first (3); the same slip whatever the language."
    checks: "Correctly indexing a cyclic residue pattern, not misreading the exponent's position."
    scoring: "Accuracy (% correct)."
  - task_type: "Multilingual math — geometry (Japanese)"
    input: "凸十角形の対角線は何本ありますか？ (How many diagonals does a convex decagon have?)"
    success:
      answer: "Correct reasoning: n(n−3)/2 = 10 × 7 / 2 = 35 diagonals."
      why: "The formula subtracts each vertex's 3 non-diagonal links (itself and two neighbours) before halving — the same derivation in any language."
    failure:
      answer: "Wrong step: computes n(n−1)/2 = 45."
      why: "Counts all vertex-pairs, including the 10 sides, instead of only diagonals — overcounts by the number of edges."
    checks: "Distinguishing diagonals from all vertex-pairs when applying the polygon formula, across languages."
    scoring: "Accuracy (% correct)."
tags: [multilingual, math, reasoning, generation]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2504.18428", title: "PolyMath: Evaluating Mathematical Reasoning in Multilingual Contexts (Wang et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2504.18428", title: "PolyMath, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://github.com/QwenLM/PolyMath", title: "QwenLM/PolyMath — NeurIPS 2025 D&B evaluation repo", trust: 0.9 }
related: [benchmark/math, benchmark/aime, benchmark/gsm8k]
---

## What It Tests
Mathematical reasoning across languages. The same problems appear in 18 languages — English,
Chinese, Spanish, Arabic, French, Bengali, Portuguese, Russian, Indonesian, German, Japanese,
Swahili, Vietnamese, Italian, Telugu, Korean, Thai, and Malay — spanning four difficulty tiers
that mix *thought depth* and *knowledge breadth*, from elementary word problems up to frontier math.

## How It Scores
**Difficulty-Weighted Accuracy (DW-ACC)**: correct answers weighted 1, 2, 4 and 8 for the low, medium, high and top levels. Reported per language and overall, with accuracy per level alongside.

## Format
Free-response problems with a final answer: 125 per level, four levels, so 500 per language. The 18 language versions are expert-calibrated translations of the same problems.

## Limitations
- **Translated set.** Reasoning in a language, not mathematics native to it.
- **Weighted score.** Top-level problems count eight times as much as low-level ones.

## Example
A problem from one level is posed in each of the 18 languages; the model's final answer in each language is checked, and the per-language DW-ACC scores are compared.
