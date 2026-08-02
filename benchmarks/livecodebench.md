---
id: benchmark/livecodebench
type: benchmark
slug: livecodebench
schema_version: 1.0
title: "LiveCodeBench v6"
name: LiveCodeBench v6
category: Code Generation
description: "Contamination-free coding benchmark of competitive-programming problems, date-stamped so models are tested only on problems published after their training cutoff; v6 spans May 2023-Apr 2025 (1,055 problems)."
what_is_it: "LiveCodeBench is a continuously updated coding benchmark built from problems on LeetCode, AtCoder, and Codeforces. Every problem carries a release date, so evaluators can score a model only on problems published after its training cutoff — making the test contamination-free by construction. Release v6 covers May 2023 through April 2025 and contains 1,055 problems."
why_people_use_it: "Static coding benchmarks leak into training data and inflate scores. LiveCodeBench's date-windowing lets you measure a model on genuinely unseen problems, so it is the standard live, contamination-controlled coding leaderboard. It also goes beyond generation to test self-repair, code execution, and test-output prediction."
examples:
  - "A Codeforces-style problem published after a model's cutoff: given a natural-language statement and constraints, generate a program that passes all hidden tests within the time limit (code-generation scenario)."
  - "Test-output prediction: given a function and an input, predict the exact output without executing the code."
method: "For code generation, the model reads a problem statement and produces a full solution, which is run against a hidden test suite; a problem counts as solved only if all tests pass. Evaluated on a date-filtered window (v6: May 2023-Apr 2025, 1,055 problems). Contamination is avoided by scoring only problems released after the model's training cutoff."
metric: "pass@1 (primary; fraction of problems solved on the first sample); pass@5 also reported."
format: "0-shot code generation from a natural-language problem statement, judged by hidden tests; plus self-repair, test-output-prediction, and code-execution scenarios."
saturation: medium
limitations: "Scores depend heavily on which date window is used — comparing models on different cutoffs is invalid, and only problems after each model's cutoff are truly contamination-free. Competitive-programming problems skew toward algorithmic puzzles, so results do not transfer directly to real-world software engineering. Hidden-test judging can miss edge cases, and self-consistency/sampling settings (pass@1 vs avg@k, temperature) shift reported numbers."
citation: "https://arxiv.org/abs/2403.07974"
worked_examples:
  - task_type: "Array counting under a time budget"
    input: "Given an array of up to 100,000 integers and a target k, count the pairs (i, j) with i < j and a[i] + a[j] = k. Time limit 1 second."
    success:
      answer: "One pass with a hash map of complements: `for x in a: total += seen[k-x]; seen[x] += 1` in O(n)."
      why: "Passes every hidden test within the time limit, including the maximal n = 100,000 case."
    failure:
      answer: "A nested double loop checking every pair: `for i: for j>i: if a[i]+a[j]==k: total+=1`, O(n^2)."
      why: "Correct on the small sample cases but Time Limit Exceeded on the n = 100,000 hidden test (about 10^10 operations)."
    checks: "Choosing an algorithm that meets the complexity budget, not merely one that is logically correct."
    scoring: "pass@1 over hidden tests: passes all vs Time Limit Exceeded on the large case."
  - task_type: "Sliding window on a string"
    input: "Return the length of the longest substring without repeating characters. The string can be up to 100,000 characters."
    success:
      answer: "Two-pointer window with a last-seen index map: advance `right`, and on a repeat set `left = max(left, last[c]+1)`, tracking `right-left+1`."
      why: "The `max(left, ...)` never moves `left` backward, so it handles repeats that occurred before the current window and passes all hidden tests."
    failure:
      answer: "Same window but resetting `left = last[c]+1` unconditionally on any repeat."
      why: "For an input like 'abba', the second 'a' pulls `left` backward, over-counting; it fails the hidden test on strings where a repeated char sits left of the window (off-by-one on window shrink)."
    checks: "Correctly bounding the window's left edge when a duplicate lies outside the current window."
    scoring: "pass@1 over hidden tests: passes all vs Wrong Answer on repeated-prefix inputs."
  - task_type: "Binary search on the answer"
    input: "Given sorted array positions of length up to 1e5 (values up to 1e9) and an integer m, place m items so the minimum gap between any two is maximized; return that gap."
    success:
      answer: "Binary-search the gap; feasibility greedily places items. Compute the midpoint as `mid = lo + (hi-lo)//2` to avoid overflow."
      why: "The feasibility check is monotonic and the overflow-safe midpoint keeps arithmetic correct across the full 1e9 range, passing every hidden test."
    failure:
      answer: "Same search but writing `mid = (lo+hi)//2` and, in a C++/Java submission, summing positions into a 32-bit int."
      why: "In a fixed-width integer language `lo+hi` overflows 32-bit range near 1e9, corrupting `mid`; Wrong Answer / Runtime Error on the large-value hidden tests (integer overflow)."
    checks: "Guarding intermediate arithmetic against overflow in bounded-width integer types."
    scoring: "pass@1 over hidden tests: passes all vs Wrong Answer on max-value cases."
  - task_type: "Dynamic programming (coin change)"
    input: "Given coin denominations and an amount up to 10,000, return the fewest coins that sum to the amount, or -1 if impossible."
    success:
      answer: "Unbounded-knapsack DP: `dp[0]=0`; for each coin, `dp[x]=min(dp[x], dp[x-coin]+1)`; return `dp[amount]` or -1."
      why: "Considers every combination, so it returns the true minimum for denominations where greedy fails (e.g. coins [1,3,4], amount 6 → 2, not 3)."
    failure:
      answer: "Greedy: repeatedly take the largest coin ≤ remaining amount."
      why: "Greedy is not optimal for arbitrary denominations — on coins [1,3,4], amount 6 it returns 3 (4+1+1) instead of 2 (3+3), so it fails the hidden non-canonical-coin test (wrong algorithm)."
    checks: "Recognizing when a greedy shortcut is not optimal and a DP is required."
    scoring: "pass@1 over hidden tests: passes all vs Wrong Answer on non-canonical coin sets."
  - task_type: "Modular combinatorics"
    input: "Compute C(n, r) mod 1_000_000_007 for n up to 1e6, answering up to 1e5 queries."
    success:
      answer: "Precompute factorials and modular inverses via Fermat's little theorem; each query is `fact[n]*inv[r]%MOD*inv[n-r]%MOD`."
      why: "All arithmetic stays under the modulus and inverse factorials give exact division mod p, passing every hidden test within the time limit."
    failure:
      answer: "Compute `factorial(n) // (factorial(r)*factorial(n-r))` with plain integer division, then take `% MOD` at the end."
      why: "Taking the modulus only at the end after integer division is mathematically invalid mod p (and overflows in fixed-width languages); Wrong Answer on every non-trivial hidden case (missing modular inverse)."
    checks: "Doing division under a modulus with modular inverses rather than integer division."
    scoring: "pass@1 over hidden tests: passes all vs Wrong Answer from invalid modular division."
tags: [code-generation, competitive-programming, contamination-free, pass-at-1]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2403.07974", title: "LiveCodeBench: Holistic and Contamination Free Evaluation of Large Language Models for Code (Jain et al., 2024)", trust: 0.95 }
  - { url: "https://livecodebench.github.io/", title: "LiveCodeBench project site", trust: 0.9 }
  - { url: "https://github.com/LiveCodeBench/LiveCodeBench", title: "LiveCodeBench GitHub (release version details)", trust: 0.95 }
related: [benchmark/ojbench, benchmark/humaneval]
---

## What It Tests
Real coding ability on fresh competitive-programming problems, free of training-data contamination.
Problems come from periodic contests on LeetCode, AtCoder, and Codeforces, each tagged with a release
date. The headline code-generation scenario asks the model to write a complete, correct program from
a natural-language statement; three further scenarios — self-repair, code execution, and test-output
prediction — probe related code-reasoning skills.

## How It Scores
pass@1 is the primary metric: the fraction of problems the model solves on its first attempt, where
"solved" means the generated program passes the entire hidden test suite. pass@5 is also reported.
The decisive design choice is the date window — release v6 spans May 2023 to April 2025 (1,055
problems), and a model is scored only on problems published after its training cutoff, so a high score
cannot come from memorized solutions. Earlier releases (v1-v5) cover progressively shorter windows
(v1: 400 problems through March 2024; v5: 880 through January 2025).

## Format
Zero-shot code generation: the model receives the problem statement and constraints and returns a
full program, executed against hidden tests under time/memory limits. The auxiliary scenarios reuse
the same problems to test self-repair (fix a failing solution), code execution (predict runtime
behavior), and test-output prediction (predict an output without running code).

## Limitations
Because the score is defined over a date window, numbers are only comparable when the window and each
model's cutoff match; only post-cutoff problems are genuinely uncontaminated. The problems are
algorithmic contest puzzles, so strong scores do not guarantee real-world software-engineering skill.
Hidden-test judging can still admit solutions that miss untested edge cases, and the sampling protocol
(pass@1 vs avg@k, temperature, number of samples) materially changes reported results.

## Example
A Codeforces problem released after a model's training cutoff gives a statement, input/output format,
and constraints. The model must output a complete program that reads input, computes the answer within
the time limit, and passes every hidden test. Partial correctness earns nothing — a solution that is
right on samples but times out on a large hidden case is not counted, which is why pass@1 on the
post-cutoff window is a demanding signal.
