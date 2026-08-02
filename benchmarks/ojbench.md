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
worked_examples:
  - task_type: "Tree path query (LCA)"
    input: "ICPC-level: given a weighted tree of up to 200,000 nodes, answer queries for the maximum edge weight on the path between two nodes. Earn Accepted only if every hidden test passes within the time and memory limits."
    success:
      answer: "Binary-lifting (LCA) precomputation carrying a per-jump max, combined correctly at the node where the two paths meet."
      why: "Handles every case, including when one query node is an ancestor of the other, earning an Accepted verdict."
    failure:
      answer: "The same binary-lifting approach but stopping one level short at the LCA, excluding the topmost edge."
      why: "Off-by-one on the ancestor boundary: when u is the LCA of v, the path's top edge is skipped, giving Wrong Answer on the ancestor-pair hidden tests."
    checks: "Precise boundary handling inside an otherwise-correct algorithm, since online-judge grading gives zero credit for a single wrong case."
    scoring: "Solve rate / pass@1: Accepted on all hidden tests vs Wrong Answer."
  - task_type: "Range-query data structure"
    input: "NOI-level: process up to 300,000 mixed operations on an array of 300,000 integers — point updates and range-sum queries — within a 1-second limit."
    success:
      answer: "A Fenwick (binary indexed) tree giving O(log n) update and prefix-sum query, so range sum is `query(r) - query(l-1)`."
      why: "Each of the 300,000 operations is O(log n), well inside the time limit, earning Accepted on the full hidden suite."
    failure:
      answer: "Recompute each range sum by iterating the subarray, `sum(a[l:r+1])`, with O(1) updates."
      why: "The right answer, but O(n) per query is ~9x10^10 operations at the limits — Time Limit Exceeded on the large hidden tests (wrong data structure for the query load)."
    checks: "Selecting a logarithmic data structure when linear scanning cannot meet the operation budget."
    scoring: "Solve rate / pass@1: Accepted vs Time Limit Exceeded on the maximal case."
  - task_type: "Language-runtime ceiling (Python vs C++)"
    input: "NOI-level dynamic programming: an O(n·m) table with n, m up to 5,000 (2.5x10^7 states, each a few arithmetic ops), 1-second limit. Submittable in Python or C++."
    success:
      answer: "The same DP in C++ with a flat `vector<int>` / array, iterating the 2.5x10^7 states directly."
      why: "C++'s per-operation constant clears 2.5x10^7 state transitions inside 1 second, earning Accepted."
    failure:
      answer: "The identical, algorithmically-correct DP written in idiomatic Python with nested lists."
      why: "Correct algorithm, but Python's interpreter overhead on 2.5x10^7 inner iterations blows the 1-second limit — Time Limit Exceeded (language-dependent runtime, not a reasoning error)."
    checks: "Whether the implementation's constant factor — including language choice — fits the time limit for a correct algorithm."
    scoring: "Solve rate / pass@1: Accepted (C++) vs Time Limit Exceeded (Python)."
  - task_type: "Weighted-graph shortest path"
    input: "ICPC-level: on a graph of up to 100,000 nodes and 500,000 weighted edges (weights up to 10^9), find the shortest distance from a source to every node."
    success:
      answer: "Dijkstra with a binary heap, distances stored in 64-bit integers: pop the nearest unsettled node, relax its edges, O((V+E) log V)."
      why: "Handles arbitrary non-negative weights and the 10^9·10^5 path magnitude without overflow, earning Accepted."
    failure:
      answer: "Plain breadth-first search counting edges (or Dijkstra accumulating distances in 32-bit ints)."
      why: "BFS finds fewest-edges, not least-weight, so it is Wrong Answer on weighted graphs; the 32-bit variant overflows on long paths near 10^9·10^5 — either way a failing verdict (wrong algorithm / overflow)."
    checks: "Matching the algorithm to weighted-graph semantics and sizing the accumulator to the value range."
    scoring: "Solve rate / pass@1: Accepted vs Wrong Answer on weighted / large-distance tests."
  - task_type: "Combinatorics under a modulus"
    input: "NOI-level: count labeled structures and report the total modulo 998_244_353; intermediate counts exceed 10^18 for n up to 2x10^5."
    success:
      answer: "Carry the modulus through every step — precomputed factorials and modular inverses (Fermat) — so each multiply is `(a*b) % MOD`."
      why: "No intermediate value leaves the modular range and division is done via modular inverse, so all hidden tests are Accepted."
    failure:
      answer: "Compute the exact counts and take `% MOD` only on the final result (in C++ with 64-bit `long long`)."
      why: "The true counts exceed 10^18 and overflow `long long` long before the final modulus, corrupting the result — Wrong Answer on the large-n hidden tests (integer overflow from late modular reduction)."
    checks: "Reducing modulo at every operation rather than once at the end, to stay within the integer range."
    scoring: "Solve rate / pass@1: Accepted vs Wrong Answer from overflow."
tags: [code-generation, competitive-programming, olympiad, pass-at-1]
updated: 2026-08-02
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
