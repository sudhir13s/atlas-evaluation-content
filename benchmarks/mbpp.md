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
worked_examples:
  - task_type: "Recurrence with a base case"
    input: "Write a function tile_ways(n) that counts the ways to tile a 2xn board with 2x1 tiles. asserts: tile_ways(3) == 3, tile_ways(4) == 5, tile_ways(5) == 8."
    success:
      answer: "Iterative Fibonacci recurrence seeded `a, b = 1, 1`; loop n times doing `a, b = b, a+b`; return a. Base cases tile_ways(0)=1, tile_ways(1)=1."
      why: "Produces 1, 1, 2, 3, 5, 8 for n = 0..5, passing all three asserts."
    failure:
      answer: "The same recurrence but seeded with base case tile_ways(0) = 0."
      why: "Shifts the whole sequence, so tile_ways(4) returns 3 instead of 5 (and tile_ways(3) returns 2), failing the asserts (off-by-one seed)."
    checks: "Getting the recurrence base case right, where an off-by-one seed corrupts every later value."
    scoring: "pass@1 on the provided asserts: 3/3 vs 1/3."
  - task_type: "List aggregation"
    input: "Write max_of_list(nums) returning the largest element. asserts: max_of_list([3,1,2]) == 3, max_of_list([-5]) == -5, max_of_list([]) == None."
    success:
      answer: "`return max(nums) if nums else None` — guard the empty case explicitly before calling max."
      why: "Handles the empty-list assert by returning None, and returns the true maximum otherwise, passing all three."
    failure:
      answer: "`return max(nums)` with no guard, or seeding `best = 0` then looping."
      why: "`max([])` raises ValueError (and a `best = 0` seed returns 0 for [-5]); either way the empty-list assert fails (empty-input edge case)."
    checks: "Handling the empty collection rather than assuming at least one element."
    scoring: "pass@1 on the provided asserts: 3/3 vs 2/3 (ValueError on the empty case)."
  - task_type: "String predicate"
    input: "Write is_palindrome(s) that ignores case and non-alphanumeric characters. asserts: is_palindrome('') == True, is_palindrome('A man, a plan, a canal: Panama') == True, is_palindrome('ab') == False."
    success:
      answer: "`t = [c.lower() for c in s if c.isalnum()]; return t == t[::-1]` — normalize first, then compare to the reverse."
      why: "The empty string normalizes to [] which equals its reverse (True), and the punctuated phrase normalizes to a true palindrome, passing all asserts."
    failure:
      answer: "Compare `s.lower() == s.lower()[::-1]` without stripping non-alphanumerics."
      why: "The spaces, comma, and colon break the comparison, so the 'A man, a plan…' assert returns False (missing normalization step)."
    checks: "Applying the specified normalization before the core comparison."
    scoring: "pass@1 on the provided asserts: 3/3 vs 1/3."
  - task_type: "Order-preserving dedup"
    input: "Write dedup(seq) returning the elements with duplicates removed, keeping first-seen order. asserts: dedup([1,1,2,3,2]) == [1,2,3], dedup([]) == [], dedup(['a','a']) == ['a']."
    success:
      answer: "`seen=set(); out=[]; for x in seq: if x not in seen: seen.add(x); out.append(x); return out`."
      why: "The seen-set preserves first-occurrence order while dropping later repeats, matching all asserts."
    failure:
      answer: "`return list(set(seq))`."
      why: "A set is unordered, so `list(set([1,1,2,3,2]))` may come back as [1,2,3] or [2,3,1]; the order-sensitive assert fails nondeterministically (duplicate handling loses order)."
    checks: "Removing duplicates while preserving insertion order, not just uniqueness."
    scoring: "pass@1 on the provided asserts: 3/3 vs frequently 1/3 (order mismatch)."
  - task_type: "Integer arithmetic edge cases"
    input: "Write factorial(n) for n >= 0. asserts: factorial(0) == 1, factorial(5) == 120, factorial(1) == 1."
    success:
      answer: "`r = 1; for i in range(2, n+1): r *= i; return r` — an empty range for n in {0,1} leaves r at 1."
      why: "The loop from 2..n is empty for 0 and 1, correctly returning 1, and gives 120 for 5, passing all asserts."
    failure:
      answer: "`r = n; for i in range(1, n): r *= i; return r` (seed with n, multiply down)."
      why: "For n = 0 this returns 0 instead of 1, failing the factorial(0) assert — the classic zero/empty base-case slip."
    checks: "Handling the 0! = 1 boundary that a naive downward multiply misses."
    scoring: "pass@1 on the provided asserts: 3/3 vs 2/3 (fails factorial(0))."
tags: [code, python, execution, pass-at-k]
updated: 2026-08-02
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
