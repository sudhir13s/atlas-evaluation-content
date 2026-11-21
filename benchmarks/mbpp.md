---
id: benchmark/mbpp
type: benchmark
slug: mbpp
schema_version: 1.0
title: "MBPP"
name: MBPP
category: Code Generation
description: "Mostly Basic Programming Problems: 974 short Python programming tasks for entry-level programmers, each with three test cases."
what_is_it: "MBPP was introduced by Austin et al. (2021). It has 974 short Python tasks written by crowdworkers with basic Python knowledge; each has a short problem statement, a self-contained reference function and three test cases written as assert statements. The authors also inspected, edited and pruned a subset into 426 hand-verified questions (the edited dataset)."
why_people_use_it: "It measures whether a model can write short Python programs from a natural-language description, checked by running the tests."
examples:
  - "Write a function to count the number of ways to tile a 2xn board with 2x1 tiles. (Fibonacci; tile_ways(4) == 5)"
method: "The model writes a function from the problem statement (often with a few examples in the prompt); it is executed against the three assert-based test cases."
metric: "Share of tasks whose generated solution passes all test cases (pass@1 is the most reported)."
format: "Few-shot or zero-shot function synthesis; unit-test execution."
limitations: "The tasks are short, self-contained Python functions. Each task has only three test cases. In the paper, few-shot performance on 100 problems found in both versions rose from 63% on the original wording to 79% on the edited wording, so results on the full and edited sets are not comparable."
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2108.07732", title: "Program Synthesis with Large Language Models (Austin et al., 2021)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/2108.07732", title: "MBPP paper, full text (ar5iv)", trust: 0.95 }
related: [benchmark/humaneval, benchmark/swe-bench]
---

## What It Tests
**Basic Python program synthesis** from a short description: small algorithms and string, list and dictionary operations.

## How It Scores
Run the generated function against the task's **three assert statements**; the task counts as solved if all pass.

## Format
Few-shot or zero-shot prompts; code executes against the asserts.

## Limitations
- **Basic scope.** Short single functions in Python only.
- **Thin tests.** Three asserts per task.
- **Two versions.** The full 974-task set and the 426-task edited set score differently.

## Example
"Write a function to count the number of ways to tile a 2×n board with 2×1 tiles." The count follows
the Fibonacci recurrence: `tile_ways(3) == 3`, `tile_ways(4) == 5`. An efficient iterative solution
passes; a naive recursion with a wrong base case (`tile_ways(0) == 0` instead of 1) fails the
asserts.
