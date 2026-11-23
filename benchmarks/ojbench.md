---
id: benchmark/ojbench
type: benchmark
slug: ojbench
schema_version: 3.1
title: "OJBench"
name: OJBench
category: Code Generation
description: "232 competition programming problems from NOI and ICPC, judged by whether a generated program passes all of the organisers' test cases."
what_is_it: "OJBench (Wang et al., 2025; Beijing University of Posts and Telecommunications, Tsinghua University, University of Chinese Academy of Sciences, Peking University and Moonshot AI) is a competition-level code benchmark of 232 problems: 159 from China's National Olympiad in Informatics (NOI) and 73 from the International Collegiate Programming Contest (ICPC). Each problem comes with the test cases released by the competition organisers (31.81 per problem on average), and problems are labelled Easy (36), Medium (79) or Hard (117)."
why_people_use_it: "It assesses competition-level code reasoning with the organisers' own test cases, in both Python and C++. The authors evaluated 37 models in the June 2025 paper."
examples:
  - "An NOI or ICPC problem statement in Markdown; the model writes a program in Python or C++, and it counts as correct only if it passes every test case."
method: "The model generates a program for each problem statement, in Python or C++, and it is run on the problem's test cases. A solution is considered correct only if it passes all test cases. The paper reports pass@1 and pass@8 in each language, and pass@1 by difficulty."
metric: "pass@k: the share of problems where at least one of k generated programs passes every test case (the paper generates eight candidates per problem and reports pass@1 and pass@8), per language."
format: "Competition code generation in Python or C++, judged on the organisers' full test cases."
limitations: "With 232 problems, a few problems move the score. Difficulty labels for NOI come from votes by contestants who solved the problem and for ICPC from team attempt ratios, so they are approximate. The problems come from past published contests, so they can appear in training data."
citation: "https://arxiv.org/abs/2506.16395"
openness: open
openness_note: "The problems and organisers' test data are public on Hugging Face (CC BY 4.0) and the judging code is on GitHub (AGPL-3.0), so anyone can rerun the benchmark."
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
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2506.16395", title: "OJBench: A Competition Level Code Benchmark For Large Language Models (Wang et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2506.16395", title: "OJBench, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://github.com/He-Ren/OJBench", title: "OJBench GitHub repository (judging code, AGPL-3.0)", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/He-Ren/OJBench_testdata", title: "He-Ren/OJBench_testdata — Hugging Face dataset (CC BY 4.0)", trust: 0.9 }
related: [benchmark/livecodebench]
---

## What It Tests
Competition-level **algorithmic code reasoning** on NOI and ICPC problems.

- **Sources** — 159 NOI and 73 ICPC problems.
- **Difficulty** — 36 Easy, 79 Medium, 117 Hard.
- **Two languages** — Python and C++.
- **Data** — test data on Hugging Face (`He-Ren/OJBench_testdata`, **CC BY 4.0**); judging code on GitHub (`He-Ren/OJBench`, **AGPL-3.0**).

## How It Scores
A solution is correct only if it **passes all test cases**; the test cases are those released by the competition organisers and validated against contestants' correct submissions.

- **pass@k** — the share of problems where at least one of k programs passes every test; the paper samples eight per problem.
- **pass@1 and pass@8** — reported per language.
- **By difficulty** — pass@1 for Easy, Medium and Hard.
- **Judge** — the repository runs programs in the DMOJ judge server and returns a verdict such as AC (accepted), WA (wrong answer) or RE (runtime error).

## Format
The model receives a problem statement in Markdown and returns a complete program in Python or C++, which runs on the problem's test cases: 31.81 per problem on average (NOI 17.21, ICPC 63.60).

## Limitations
- **Small set.** 232 problems, so a few problems move the score.
- **Approximate difficulty.** Labels come from contestant votes (NOI) and team attempt ratios (ICPC).
- **Past contests.** The problems are from published competitions.

## Example
No problem statement is quoted here: neither the paper nor the repository shows one as text, and the dataset has no preview. As the paper describes it, the model reads an NOI or ICPC statement in Markdown, writes a program in Python or C++, and the judge runs it on the organisers' test cases; it counts as solved only if every case passes.
