---
id: benchmark/polymath
type: benchmark
slug: polymath
schema_version: 1.0
title: "PolyMATH"
name: PolyMATH
category: Multilingual
description: "Parallel mathematical reasoning across 18 languages and four difficulty tiers."
what_is_it: "PolyMATH is a multilingual math-reasoning benchmark of 500 problems per language in 18 languages, organised into four difficulty levels from grade-school word problems to Olympiad and frontier mathematics."
why_people_use_it: "It separates two things most math benchmarks conflate — reasoning ability and the language it happens in — by giving the exact same problems in 18 languages, revealing that even top reasoning models lose several points simply by switching language."
examples:
  - "An Olympiad-level algebra or number-theory problem presented identically in English, Chinese, Swahili, Telugu, and 14 other languages, requiring a worked derivation and a final answer."
method: "Free-response math solving over 125 problems per language at each of four difficulty levels (500 per language, 18 languages); the final answer is extracted and checked for correctness."
metric: "Accuracy (% correct), reported per language and per difficulty level."
format: "Open-ended generation with chain-of-thought; answer extracted and matched; 0-shot."
saturation: low
limitations: "The hardest tier is far from solved — leading reasoning models sit around 40% at the top level — but that also means most of the discriminating signal is concentrated in a few hard problems per language. As a parallel translated set it measures reasoning-in-language, not culturally-native math, and answer extraction on free-form solutions adds some grading noise."
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
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2504.18428", title: "PolyMath: Evaluating Mathematical Reasoning in Multilingual Contexts (Qwen Team, 2025)", trust: 0.95 }
  - { url: "https://github.com/QwenLM/PolyMath", title: "QwenLM/PolyMath — NeurIPS 2025 D&B evaluation repo", trust: 0.9 }
related: [benchmark/math, benchmark/aime, benchmark/gsm8k]
---

## What It Tests
Mathematical reasoning across languages. The same problems appear in 18 languages — English,
Chinese, Spanish, Arabic, French, Bengali, Portuguese, Russian, Indonesian, German, Japanese,
Swahili, Vietnamese, Italian, Telugu, Korean, Thai, and Malay — spanning four difficulty tiers
that mix *thought depth* and *knowledge breadth*, from elementary word problems up to frontier math.

## How It Scores
Accuracy per language and per difficulty level, over 125 problems at each level (500 problems per
language). The point is the cross-language spread: even Qwen3-235B-A22B-Thinking and Gemini-2.5-Pro
reach only ~54.6 and ~52.2 overall, about 40% at the hardest level, with per-language differences
of up to 10 points at low accuracy — evidence that language alone moves math scores.

## Format
Open-ended generation: the model produces a chain-of-thought derivation, and the final answer is
extracted and checked, run 0-shot. Because the 18 language versions are parallel translations of
the same problems, a per-language gap isolates reasoning-in-that-language from problem difficulty.

## Limitations
The top tier remains largely unsolved, so much of the discriminative signal rides on a small set
of hard problems per language. It is a parallel translated benchmark, hence it measures reasoning
in a language rather than culturally-native mathematics, and grading free-form solutions by answer
extraction introduces some measurement noise on borderline outputs.

## Example
An Olympiad-grade problem — say, proving a divisibility property or solving a functional
equation — is posed identically in all 18 languages. A model that solves it in English but fails
the Telugu or Swahili version, despite identical mathematics, exposes a language-dependent
reasoning gap that a monolingual benchmark like MATH or AIME cannot see.
