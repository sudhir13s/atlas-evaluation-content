---
id: benchmark/humaneval
type: benchmark
slug: humaneval
schema_version: 1.0
title: "HumanEval"
name: HumanEval
category: Code Generation
description: "164 hand-written Python function-completion problems scored by executing unit tests."
what_is_it: "OpenAI's 164 hand-authored programming problems: each gives a function signature and docstring, and the model must write the body. Correctness is decided by running hidden unit tests, not by matching a reference solution. It introduced the pass@k metric."
why_people_use_it: "It was the first widely adopted execution-based code benchmark and remains the default headline number for code generation, reported by nearly every model. Execution-based scoring means a solution is judged on whether it works, not whether it looks right."
examples:
  - "def second_largest(nums: list[int]) -> int: '''Return the second-largest element; the list has >= 2 distinct elements.''' (model writes a correct O(n) body that handles duplicates)"
method: "0-shot; the model completes each function, generated solutions are executed against hidden unit tests in a sandbox. pass@1 (single sample) is the standard headline; pass@10/pass@100 estimate from many samples."
metric: "pass@k — probability that at least one of k sampled solutions passes all tests (pass@1 is standard)."
format: "0-shot function completion; unit-test execution."
saturation: high
limitations: "Only 164 problems, all small self-contained Python functions (avg ~7 lines) — too narrow to generalize to real engineering (no debugging, refactoring, multi-file context, or other languages). Saturated at 90%+ for frontier models. The stricter HumanEval+ adds many more tests and exposes solutions that pass the original but are subtly wrong."
citation: "https://arxiv.org/abs/2107.03374"
worked_examples:
  - task_type: "Array manipulation"
    input: "def second_largest(nums: list[int]) -> int: return the second-largest distinct value (the list has at least two distinct elements)."
    success:
      answer: "A single pass tracking the largest and second-largest values while skipping any value equal to the current max, so second_largest([10, 10, 5]) returns 5."
      why: "Handles duplicates of the maximum, satisfying the distinct-value contract on every hidden test."
    failure:
      answer: "return sorted(nums)[-2]"
      why: "On [10, 10, 5] the second element from the end is 10, not 5 — duplicate maxima break the naive sort-and-index."
    checks: "Translating a spec into code that survives the duplicate-max edge case, not just the happy path."
    scoring: "pass@1 on hidden unit tests (e.g. 8/8 vs 6/8)."
  - task_type: "String normalization"
    input: "def is_palindrome(s: str) -> bool: return True if s reads the same forwards and backwards, ignoring case and non-alphanumeric characters."
    success:
      answer: "cleaned = [c.lower() for c in s if c.isalnum()]; return cleaned == cleaned[::-1]"
      why: "Strips punctuation and normalizes case first, so 'A man, a plan, a canal: Panama' correctly returns True."
    failure:
      answer: "return s == s[::-1]"
      why: "Passes bare lowercase inputs like 'racecar' but fails the mixed-case and punctuation edge cases ('RaceCar', 'A man, a plan...') the spec explicitly requires."
    checks: "Honoring the full spec (case- and punctuation-insensitivity), not only the simplest input shape."
    scoring: "pass@1 on hidden unit tests."
  - task_type: "Numeric computation"
    input: "def factorial(n: int) -> int: return n! for n >= 0, with factorial(0) == 1."
    success:
      answer: "result = 1; for i in range(1, n + 1): result *= i; return result"
      why: "range(1, n + 1) includes n itself, so factorial(5) correctly returns 120 and factorial(0) returns 1."
    failure:
      answer: "result = 1; for i in range(1, n): result *= i; return result"
      why: "Passes n = 0 and n = 1 but range(1, n) omits the final factor n (off-by-one), so factorial(5) returns 24 instead of 120."
    checks: "Getting the loop bound right so the final multiplicand is not dropped."
    scoring: "pass@1 on hidden unit tests."
  - task_type: "Search algorithm"
    input: "def binary_search(arr: list[int], target: int) -> int: return the index of target in an ascending-sorted list, or -1 if absent."
    success:
      answer: "lo, hi = 0, len(arr) - 1; while lo <= hi: mid = (lo + hi) // 2; ... ; return -1"
      why: "The lo <= hi condition still checks the final single-element window, so binary_search([5], 5) returns 0."
    failure:
      answer: "Same loop but with the condition `while lo < hi`."
      why: "Passes multi-element cases but exits before inspecting the last remaining element when lo == hi, so binary_search([5], 5) wrongly returns -1."
    checks: "Using the loop condition that still examines the single-element boundary window."
    scoring: "pass@1 on hidden unit tests."
  - task_type: "Aggregation with empty input"
    input: "def average(nums: list[float]) -> float: return the arithmetic mean of nums, or 0.0 for an empty list."
    success:
      answer: "return sum(nums) / len(nums) if nums else 0.0"
      why: "The guard returns 0.0 for the empty list, avoiding division by zero while computing the mean otherwise."
    failure:
      answer: "return sum(nums) / len(nums)"
      why: "Works for non-empty lists but raises ZeroDivisionError on the empty-list edge case the spec calls out."
    checks: "Guarding the empty-input case before dividing by the length."
    scoring: "pass@1 on hidden unit tests."
tags: [code, python, execution, pass-at-k]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2107.03374", title: "Evaluating Large Language Models Trained on Code (Chen et al., 2021)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2305.01210", title: "Is Your Code Generated by ChatGPT Really Correct? (EvalPlus / HumanEval+, Liu et al., 2023)", trust: 0.85 }
related: [benchmark/mbpp, benchmark/swe-bench]
---

## What It Tests
The ability to translate a natural-language spec (docstring) plus a signature into a correct,
self-contained Python function. It measures functional correctness on small algorithmic problems.

## How It Scores
**pass@k**: the model samples k solutions per problem, and a problem counts solved if any sample
passes all hidden unit tests. pass@1 (one sample) is the headline; pass@10/pass@100 are estimated
from a larger sample pool using an unbiased estimator. Because scoring runs the code, a plausible-but-
wrong solution simply fails.

## Format
0-shot: no worked examples, just the signature and docstring. Generated code runs in a sandbox
against the tests.

## Limitations
**Narrow and saturated.** 164 tiny Python functions cannot represent real software work — no
debugging, no cross-file context, no non-Python languages. Frontier models exceed 90% pass@1.
HumanEval+ (EvalPlus) adds ~80× more tests and drops many models several points, showing the original
test suites are too thin to catch edge-case bugs.

## Example
`def second_largest(nums: list[int]) -> int:` with a docstring promising ≥2 distinct elements. A
correct single-pass solution tracks the largest and second-largest and skips duplicates of the max;
the lazy `sorted(nums)[-2]` fails on `[10, 10, 5]` (returns 10, not 5) — an edge case the unit tests
are designed to expose.
