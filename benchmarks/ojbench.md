---
id: benchmark/ojbench
type: benchmark
slug: ojbench
schema_version: 1.0
title: "OJBench"
name: OJBench
category: Code Generation
description: "232 olympiad/ICPC-level programming problems from NOI and ICPC, judged with full hidden test suites in Python and C++ — a hard test of competition-grade code reasoning."
what_is_it: "OJBench is a competition-level code benchmark of 232 rigorously selected problems drawn from China's National Olympiad in Informatics (NOI) and the International Collegiate Programming Contest (ICPC). Problems are labeled Easy/Medium/Hard from real contestant data and are solvable in both Python and C++. Each submission is judged online-judge style: it counts as solved only if it is Accepted on the full hidden test suite."
why_people_use_it: "Most coding benchmarks top out well below olympiad difficulty. OJBench targets the hardest tier — NOI and ICPC problems — where even strong reasoning models like o4-mini and Gemini 2.5 Pro struggle, giving real headroom to measure competition-grade algorithmic reasoning that easier benchmarks can no longer discriminate."
examples:
  - "An ICPC-level graph problem: design an efficient algorithm from a natural-language statement and submit a program that must pass every hidden test within the time limit to earn Accepted."
  - "An NOI-level dynamic-programming problem solved in both Python and C++, with C++ typically clearing the time limits that the Python solution cannot."
method: "The model reads a competition problem statement and generates a complete program (Python or C++), which is run against the problem's full hidden test suite. A problem is solved only on verdict Accepted (all test cases pass); Wrong Answer, Runtime Error, or Time Limit Exceeded count as failures. 232 problems across three difficulty tiers; 37 models evaluated in the paper."
metric: "Solve rate / pass@1 (fraction of problems reaching an Accepted verdict on all hidden tests)."
format: "0-shot competition code generation, judged by full hidden test suites; bilingual Python and C++."
saturation: low
limitations: "The 232-problem set is small and skewed toward hard olympiad/ICPC problems, so scores are low and noisy at the top. Full-testcase Accepted judging is strict — a correct algorithm that is too slow in one language (often Python) scores zero — which couples the metric to implementation/runtime, not just reasoning. Being drawn from published contests, older problems risk contamination, so the benchmark leans on difficulty rather than novelty for its signal."
citation: "https://arxiv.org/abs/2506.16395"
example:
  task_type: "Olympiad/ICPC code generation (Python or C++)"
  input: "ICPC-level: given a weighted tree of up to 200,000 nodes, answer queries for the maximum edge weight on the path between two nodes. Earn Accepted only if every hidden test passes within the time and memory limits."
  success:
    answer: "Binary-lifting (LCA) precomputation carrying a per-jump max, combined correctly at the node where the two paths meet."
    why: "Handles every case, including when one query node is an ancestor of the other, earning an Accepted verdict."
  failure:
    answer: "The same binary-lifting approach but stopping one level short at the LCA, excluding the topmost edge."
    why: "Off-by-one on the ancestor boundary: when u is the LCA of v, the path's top edge is skipped, giving Wrong Answer on the ancestor-pair hidden tests."
  checks: "Precise boundary handling inside an otherwise-correct algorithm, since online-judge grading gives zero credit for a single wrong case."
  scoring: "Solve rate / pass@1 - Accepted on all hidden tests vs Wrong Answer."
tags: [code-generation, competitive-programming, olympiad, pass-at-1]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2506.16395", title: "OJBench: A Competition Level Code Benchmark For Large Language Models (2025)", trust: 0.95 }
  - { url: "https://github.com/He-Ren/OJBench", title: "OJBench GitHub repository", trust: 0.9 }
related: [benchmark/livecodebench]
---

## What It Tests
Competition-grade algorithmic reasoning at the olympiad and ICPC level. The 232 problems are pulled
from China's National Olympiad in Informatics (NOI) and the International Collegiate Programming
Contest (ICPC) — a tier far above typical coding benchmarks — and are stratified into Easy, Medium,
and Hard using real contestant votes and submission data. Each problem is solvable in both Python and
C++, so the benchmark also exposes language-dependent performance gaps.

## How It Scores
Solve rate (pass@1): the fraction of problems for which the model's program earns an Accepted verdict
on the full hidden test suite. Judging is strict online-judge style — the verdict is Accepted only if
every test case passes; any Wrong Answer, Runtime Error, or Time Limit Exceeded counts as a failure.
Across 37 models evaluated in the paper, even state-of-the-art reasoning models (o4-mini,
Gemini 2.5 Pro) solved only a modest share of the hard problems, and reasoning-oriented models clearly
outperformed non-reasoning ones, with C++ generally scoring higher than Python.

## Format
Zero-shot competition code generation: the model receives a contest problem statement and returns a
complete program in Python or C++, which is executed against the problem's hidden tests under time and
memory limits. There is no partial credit — the unit of success is the Accepted verdict.

## Limitations
The set is small (232 problems) and deliberately hard, so top scores are low and sensitive to a few
problems. Full-testcase Accepted judging ties the score to implementation efficiency, not reasoning
alone: a correct-but-slow solution (common in Python) is marked failed on Time Limit Exceeded. Because
the problems come from published contests, older items carry contamination risk; OJBench relies on
sheer difficulty, rather than a live date-window, for its discriminating power.

## Example
An ICPC-level problem gives a graph and a query about optimal routing. The model must devise an
efficient algorithm (a naive approach will time out), implement it correctly, and submit; only a
program that returns Accepted across every hidden test scores. A solution with the right idea but an
off-by-one on a boundary case gets Wrong Answer and zero credit — the strict verdict is what makes
OJBench a demanding test of both reasoning and precise implementation.
