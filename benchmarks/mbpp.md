---
id: benchmark/mbpp
type: benchmark
slug: mbpp
schema_version: 1.0
title: "MBPP"
name: MBPP
category: Code Generation
description: "Mostly Basic Python Problems — ~1,000 entry-level programming tasks with tests."
what_is_it: "Google's Mostly Basic Python Problems: ~974 short, entry-level tasks, each with a natural-language description, a reference solution, and three assert-based test cases. A hand-verified subset (~427 problems) is the cleaner reporting slice."
why_people_use_it: "It complements HumanEval with more problems at a similar (basic) difficulty, giving a larger, less-noisy minimum-competence signal for Python code generation. Widely reported alongside HumanEval."
examples:
  - "Write a function to count the number of ways to tile a 2xn board with 2x1 tiles. (Fibonacci; tile_ways(4) == 5)"
method: "Few-shot (commonly 3-shot) or 0-shot; generated solutions are executed against the provided assert tests. pass@1 is standard."
metric: "pass@1 (% of tasks whose generated solution passes all asserts)."
format: "3-shot or 0-shot; unit-test execution."
saturation: high
limitations: "Problems are basic and self-contained, so MBPP works better as a competence floor than a discriminator between strong models (frontier models score 85%+). The raw split has some noisy descriptions and weak tests; use the sanitized subset. Python-only and small-function-only, like HumanEval."
citation: "https://arxiv.org/abs/2108.07732"
example:
  task_type: "Basic Python function synthesis"
  input: "Write a function tile_ways(n) that counts the ways to tile a 2xn board with 2x1 tiles. asserts: tile_ways(3) == 3, tile_ways(4) == 5, tile_ways(5) == 8."
  success:
    answer: "An iterative Fibonacci recurrence with base cases tile_ways(0) = 1 and tile_ways(1) = 1."
    why: "Produces 1, 1, 2, 3, 5, 8 for n = 0..5, passing all three asserts."
  failure:
    answer: "The same recurrence but with base case tile_ways(0) = 0."
    why: "Shifts the whole sequence, so tile_ways(4) returns 3 instead of 5 (and tile_ways(3) returns 2), failing the asserts."
  checks: "Getting the recurrence base case right, where an off-by-one seed corrupts every later value."
  scoring: "pass@1 on the provided asserts (e.g. 3/3 vs 1/3)."
tags: [code, python, execution, pass-at-k]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2108.07732", title: "Program Synthesis with Large Language Models (Austin et al., 2021)", trust: 0.95 }
related: [benchmark/humaneval, benchmark/swe-bench]
---

## What It Tests
Basic Python program synthesis from a short description — string manipulation, small algorithms,
list/dict operations. It measures whether a model reliably writes correct simple functions.

## How It Scores
pass@1: execute the generated solution against the three provided assert statements; the task is
solved if all pass. Because the problems are easy and numerous (~974, or ~427 sanitized), the score
is stable but ceilinged — most capable models cluster high.

## Format
Commonly 3-shot (three worked examples prepended) or 0-shot. Generated code is executed against the
asserts in a sandbox.

## Limitations
**Basic and saturated**: frontier models routinely exceed 85% pass@1, so MBPP separates weak from
competent models but not good from great. The unsanitized split has noisy prompts and thin tests —
prefer the hand-verified subset. Like HumanEval, it is Python-only and isolated-function-only.

## Example
"Write a function to count the number of ways to tile a 2×n board with 2×1 tiles." The count follows
the Fibonacci recurrence: `tile_ways(3) == 3`, `tile_ways(4) == 5`. An efficient iterative solution
passes; a naive recursion with a wrong base case (`tile_ways(0) == 0` instead of 1) fails the
asserts.
