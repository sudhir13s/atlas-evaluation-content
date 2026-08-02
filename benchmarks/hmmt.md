---
id: benchmark/hmmt
type: benchmark
slug: hmmt
schema_version: 1.0
title: "HMMT"
name: HMMT
category: Math & Logic
description: "Harvard-MIT Math Tournament problems used as an uncontaminated competition-math eval — final-answer accuracy over the fresh Feb/Nov 2025 problem sets."
what_is_it: "HMMT (Harvard-MIT Mathematics Tournament) is a prestigious high-school math contest held twice a year (November and February). As an LLM eval — popularized by MathArena — the short-answer problems from a fresh HMMT sitting are used as an uncontaminated competition-math test: each problem has a single numeric or closed-form final answer that is auto-graded, similar in spirit to AIME."
why_people_use_it: "Because each HMMT sitting is brand-new, its problems are unlikely to appear in training data, so it measures genuine mathematical reasoning rather than memorization. It sits alongside AIME as a go-to frontier math benchmark; the February sitting is notably harder (national-olympiad level), making it a stronger discriminator as AIME scores climb into the mid-90s."
examples:
  - "HMMT February 2025, algebra: compute a specific value defined by a functional or number-theoretic condition — the model must output the exact integer/closed-form answer, which is checked against the gold answer."
  - "HMMT February 2025, combinatorics: count configurations satisfying stated constraints; the single numeric answer is auto-graded."
method: "Each model is prompted with one contest problem and must produce a final numeric/closed-form answer, which is extracted and compared to the gold answer. On MathArena, every model is run 4 times per problem and scores are averaged. The Feb 2025 set has 30 problems; a separate Nov 2025 set is also tracked."
metric: "Accuracy (% of problems solved), averaged over 4 runs per problem."
format: "0-shot final-answer math; single numeric or closed-form answer per problem, auto-graded."
saturation: high
limitations: "Small problem sets (~30 per sitting) mean high variance — one problem is ~3 percentage points. Only the final answer is graded, so a right answer from flawed reasoning still counts and proof-based rounds are excluded. Contamination is reduced but not zero: some problems resemble prior contest items, and once a sitting is public it can leak into future training data, so freshness must be tracked by date."
citation: "https://matharena.ai/"
tags: [math, competition-math, reasoning, final-answer]
updated: 2026-07-20
sources:
  - { url: "https://matharena.ai/", title: "MathArena: Evaluating LLMs on Uncontaminated Math Competitions", trust: 0.9 }
  - { url: "https://arxiv.org/abs/2505.23281", title: "MathArena: Evaluating LLMs on Uncontaminated Math Competitions (Balunović et al., 2025)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/MathArena/hmmt_feb_2025", title: "MathArena/hmmt_feb_2025 dataset (Hugging Face)", trust: 0.95 }
related: [benchmark/aime]
---

## What It Tests
Competition-level mathematical reasoning on fresh problems. HMMT is a twice-yearly high-school
tournament; its short-answer problems — spanning algebra, combinatorics, number theory, and geometry
— are repurposed as an eval. Each problem has one exact answer, so the test rewards correct
multi-step derivation, not recall. The February sitting is deliberately harder than AIME (closer to
national-olympiad difficulty), while the November sitting is nearer AMC/AIME level.

## How It Scores
Accuracy — the percentage of problems whose final answer matches the gold answer. Because a single
sitting is small (the MathArena Feb 2025 set is 30 problems), results are averaged over multiple runs
per problem (MathArena uses 4) to cut variance. Top frontier models now reach the mid-90s on HMMT
February 2025, which is why it is used as a harder companion to AIME rather than a beginner test.

## Format
Zero-shot final-answer math: the model receives one problem statement (LaTeX) and must emit a single
numeric or closed-form answer. The answer is parsed and string/value-matched against the reference —
no partial credit and no grading of the intermediate work.

## Limitations
The sets are tiny (~30 problems per sitting), so one problem shifts the score by several points and
small model gaps are noise. Only the final answer is checked, so a correct answer reached by faulty
reasoning still scores, and the proof-heavy Team Round is excluded. Freshness reduces but does not
eliminate contamination — occasional problems echo earlier contests, and each sitting can leak into
later training corpora once public, so the evaluation date matters.

## Example
An HMMT February 2025 number-theory problem asks for a specific integer defined by a divisibility
condition. The model must carry out the full argument and output just that integer; MathArena runs it
four times and averages. A model that sets up the problem correctly but makes an arithmetic slip
produces the wrong final value and scores zero on that item — final-answer grading gives no credit
for a nearly-correct derivation.
