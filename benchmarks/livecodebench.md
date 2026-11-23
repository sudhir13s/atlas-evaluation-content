---
id: benchmark/livecodebench
type: benchmark
slug: livecodebench
schema_version: 4.0
title: LiveCodeBench v6
name: LiveCodeBench v6
category: Code Generation
description: A coding benchmark that keeps collecting new contest problems from LeetCode, AtCoder and Codeforces, each with a release date; release v6 has 1,055 problems from May 2023 to April 2025.
what_is_it: LiveCodeBench (Jain et al., 2024) continuously collects new problems from contests on LeetCode, AtCoder and Codeforces. Because each problem has a release date, a model can be scored only on problems published after its training cutoff. Besides code generation it tests self-repair, code execution and test-output prediction. Release v6 covers May 2023 to April 2025 with 1,055 problems.
why_people_use_it: The authors write that existing benchmarks such as HumanEval and MBPP are no longer sufficient for assessing new models; collecting new problems over time with release dates lets evaluators avoid contamination (2024 paper).
examples:
- 'A Codeforces-style problem published after a model''s cutoff: given a natural-language statement and constraints, generate a program that passes all hidden tests within the time limit (code-generation scenario).'
- 'Test-output prediction: given a function and an input, predict the exact output without executing the code.'
method: For code generation, the model reads a problem statement and writes a program, which is run against the problem's tests with a modified version of the APPS checker. Evaluation can be limited to problems released in a date window, such as after a model's training cutoff.
metric: 'pass@1 and pass@5: the share of problems for which a sampled program passes all of the problem''s tests, estimated from several samples per problem (the repository''s default run draws 10 samples at temperature 0.2).'
format: 'Code generation from a natural-language problem statement, plus self-repair, code-execution and test-output-prediction scenarios. Releases: v1 400, v2 511, v3 612, v4 713, v5 880 and v6 1,055 problems, all starting May 2023 (v6 ends April 2025).'
limitations: A score depends on the release version and date window, so both must be stated. Only problems released after a model's training cutoff avoid contamination for that model. The problems are contest problems from three platforms. The default lite dataset uses pruned and sampled tests, so it can differ from runs on the original, larger test suites.
citation: https://arxiv.org/abs/2403.07974
openness: open
openness_note: 'The problems, tests and evaluation code are public: the repository is MIT-licensed and the datasets are on Hugging Face, whose card lists a Creative Commons licence without naming the variant.'
worked_examples:
- task_type: Array counting under a time budget
  input: Given an array of up to 100,000 integers and a target k, count the pairs (i, j) with i < j and a[i] + a[j] = k. Time limit 1 second.
  success:
    answer: 'One pass with a hash map of complements: `for x in a: total += seen[k-x]; seen[x] += 1` in O(n).'
    why: Passes every hidden test within the time limit, including the maximal n = 100,000 case.
  failure:
    answer: 'A nested double loop checking every pair: `for i: for j>i: if a[i]+a[j]==k: total+=1`, O(n^2).'
    why: Correct on the small sample cases but Time Limit Exceeded on the n = 100,000 hidden test (about 10^10 operations).
  checks: Choosing an algorithm that meets the complexity budget, not merely one that is logically correct.
  scoring: 'pass@1 over hidden tests: passes all vs Time Limit Exceeded on the large case.'
- task_type: Sliding window on a string
  input: Return the length of the longest substring without repeating characters. The string can be up to 100,000 characters.
  success:
    answer: 'Two-pointer window with a last-seen index map: advance `right`, and on a repeat set `left = max(left, last[c]+1)`, tracking `right-left+1`.'
    why: The `max(left, ...)` never moves `left` backward, so it handles repeats that occurred before the current window and passes all hidden tests.
  failure:
    answer: Same window but resetting `left = last[c]+1` unconditionally on any repeat.
    why: For an input like 'abba', the second 'a' pulls `left` backward, over-counting; it fails the hidden test on strings where a repeated char sits left of the window (off-by-one on window shrink).
  checks: Correctly bounding the window's left edge when a duplicate lies outside the current window.
  scoring: 'pass@1 over hidden tests: passes all vs Wrong Answer on repeated-prefix inputs.'
- task_type: Binary search on the answer
  input: Given sorted array positions of length up to 1e5 (values up to 1e9) and an integer m, place m items so the minimum gap between any two is maximized; return that gap.
  success:
    answer: Binary-search the gap; feasibility greedily places items. Compute the midpoint as `mid = lo + (hi-lo)//2` to avoid overflow.
    why: The feasibility check is monotonic and the overflow-safe midpoint keeps arithmetic correct across the full 1e9 range, passing every hidden test.
  failure:
    answer: Same search but writing `mid = (lo+hi)//2` and, in a C++/Java submission, summing positions into a 32-bit int.
    why: In a fixed-width integer language `lo+hi` overflows 32-bit range near 1e9, corrupting `mid`; Wrong Answer / Runtime Error on the large-value hidden tests (integer overflow).
  checks: Guarding intermediate arithmetic against overflow in bounded-width integer types.
  scoring: 'pass@1 over hidden tests: passes all vs Wrong Answer on max-value cases.'
- task_type: Dynamic programming (coin change)
  input: Given coin denominations and an amount up to 10,000, return the fewest coins that sum to the amount, or -1 if impossible.
  success:
    answer: 'Unbounded-knapsack DP: `dp[0]=0`; for each coin, `dp[x]=min(dp[x], dp[x-coin]+1)`; return `dp[amount]` or -1.'
    why: Considers every combination, so it returns the true minimum for denominations where greedy fails (e.g. coins [1,3,4], amount 6 → 2, not 3).
  failure:
    answer: 'Greedy: repeatedly take the largest coin ≤ remaining amount.'
    why: Greedy is not optimal for arbitrary denominations — on coins [1,3,4], amount 6 it returns 3 (4+1+1) instead of 2 (3+3), so it fails the hidden non-canonical-coin test (wrong algorithm).
  checks: Recognizing when a greedy shortcut is not optimal and a DP is required.
  scoring: 'pass@1 over hidden tests: passes all vs Wrong Answer on non-canonical coin sets.'
- task_type: Modular combinatorics
  input: Compute C(n, r) mod 1_000_000_007 for n up to 1e6, answering up to 1e5 queries.
  success:
    answer: Precompute factorials and modular inverses via Fermat's little theorem; each query is `fact[n]*inv[r]%MOD*inv[n-r]%MOD`.
    why: All arithmetic stays under the modulus and inverse factorials give exact division mod p, passing every hidden test within the time limit.
  failure:
    answer: Compute `factorial(n) // (factorial(r)*factorial(n-r))` with plain integer division, then take `% MOD` at the end.
    why: Taking the modulus only at the end after integer division is mathematically invalid mod p (and overflows in fixed-width languages); Wrong Answer on every non-trivial hidden case (missing modular inverse).
  checks: Doing division under a modulus with modular inverses rather than integer division.
  scoring: 'pass@1 over hidden tests: passes all vs Wrong Answer from invalid modular division.'
tags:
- code-generation
- competitive-programming
- contamination-free
- pass-at-1
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2403.07974
  title: 'LiveCodeBench: Holistic and Contamination Free Evaluation of Large Language Models for Code (Jain et al., 2024)'
  trust: 0.95
- url: https://livecodebench.github.io/
  title: LiveCodeBench project site
  trust: 0.9
- url: https://github.com/LiveCodeBench/LiveCodeBench
  title: LiveCodeBench GitHub (release versions, MIT licence)
  trust: 0.9
- url: https://huggingface.co/datasets/livecodebench/code_generation_lite
  title: livecodebench/code_generation_lite — Hugging Face dataset card
  trust: 0.9
related:
- benchmark/ojbench
- benchmark/humaneval
---

## What It Tests

**Coding on contest problems released over time**, from LeetCode, AtCoder and Codeforces.

- **Code generation** — write a program from a problem statement.
- **Self-repair** — fix a failing solution.
- **Code execution** and **test-output prediction** — reason about what code outputs.

Introduced by Jain et al. (2024); the paper's first snapshot had 400 problems.

## How It Scores

**pass@1** (and pass@5): the share of problems whose generated program passes all the tests.

- **Checker** — a modified version of the APPS benchmark checker runs the program.
- **Sampling** — the repository's default run draws 10 samples per problem at temperature 0.2.
- **Date window** — each problem's release date lets evaluators score only problems published after a model's training cutoff.

## Format

The model gets a natural-language problem statement and returns a program. Release versions grow over time:

| Release | Problems | Window |
|---|---|---|
| v1 | 400 | May 2023 to Mar 2024 |
| v2 | 511 | to May 2024 |
| v3 | 612 | to Jul 2024 |
| v4 | 713 | to Sep 2024 |
| v5 | 880 | to Jan 2025 |
| v6 | 1,055 | to Apr 2025 |

- **Data** — Hugging Face datasets `livecodebench/code_generation_lite`, `execution` and `test_generation`.
- **Code** — GitHub `LiveCodeBench/LiveCodeBench`, **MIT licence**; the dataset card lists a Creative Commons licence without naming the variant.

## Limitations

- **Window-dependent.** State the release version and date window with any score.
- **Contamination per model.** Only problems after a model's cutoff are unseen by it.
- **Contest problems.** Three competition platforms only.
- **Lite tests.** The default dataset prunes and samples tests; the original suites are larger.

## Example

No single item is quoted here: the dataset card shows no item preview, and its viewer is disabled. As the paper and card describe it, a problem from one of the three platforms gives a statement, input and output format, examples, constraints and a release date. The model returns a program, which runs against the hidden tests; it counts as solved only if it passes them all.
