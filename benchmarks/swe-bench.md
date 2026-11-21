---
id: benchmark/swe-bench
type: benchmark
slug: swe-bench
schema_version: 1.0
title: "SWE-bench"
name: SWE-bench
category: Code Generation
description: "2,294 software engineering problems from real GitHub issues in 12 popular Python repositories; the model must edit the codebase so the issue's tests pass."
what_is_it: "SWE-bench was introduced by Jimenez et al. (2023). Its 2,294 task instances come from merged pull requests that resolve a GitHub issue and add tests, across 12 popular Python repositories. Given the codebase and the issue text, the model must edit the code to resolve the issue. SWE-bench Verified, released by OpenAI with the SWE-bench authors, is a subset of 500 test samples that professional developers screened for well-specified issues and appropriate tests."
why_people_use_it: "Resolving an issue often needs changes across several functions, classes and files, interaction with an execution environment and long contexts, which the authors argue goes far beyond traditional code generation."
examples:
  - "Fix a Django issue where QuerySet.bulk_create() silently ignores update_conflicts on unsupported backends; the patch must raise NotSupportedError and pass the added regression test."
method: "The system produces a patch for the repository; the harness applies it and runs the instance's tests. Every instance has at least one fail-to-pass test from the reference fix."
metric: "% resolved: the share of instances whose patch applies and makes all of the instance's tests pass."
format: "Patch generation over a full repository; test execution."
limitations: "Scores depend on the agent scaffold, tools and retrieval setup as well as the model, so a number describes the whole system. SWE-bench Verified exists because OpenAI's annotators found test-set samples with underspecified issues or unsuitable tests. Results on the full set, Verified (500) and Lite (300) are not comparable. Repositories are Python only."
citation: "https://arxiv.org/abs/2310.06770"
worked_examples:
  - task_type: "Django — raise instead of silently ignore"
    input: "Django issue: QuerySet.bulk_create() silently ignores update_conflicts=True on backends that do not support it. Produce a patch that makes it fail loudly, and make the added regression test pass."
    success:
      answer: "A patch in django/db/models/query.py that raises `NotSupportedError` when the backend lacks `supports_update_conflicts`, plus the accompanying regression test."
      why: "Turns the fail-to-pass test green while keeping every pass-to-pass test green."
    failure:
      answer: "A patch that emits `warnings.warn(...)` instead of raising."
      why: "The fail-to-pass test asserts NotSupportedError is raised; a warning does not raise, so that test still fails and the issue counts unresolved (matched the intent, not the tested behavior)."
    checks: "Navigating a large repo to the right fix and matching the exact tested behavior, not an approximation of it."
    scoring: "Resolve rate on hidden tests: fail-to-pass and pass-to-pass all green vs fail-to-pass still failing."
  - task_type: "scikit-learn — fix without regressing (pass-to-pass)"
    input: "scikit-learn issue: `LabelEncoder.transform([])` raises on an empty array instead of returning an empty array. Patch `transform` so an empty input returns an empty array."
    success:
      answer: "Guard the empty case at the top of `transform`: `if len(y) == 0: return np.array([], dtype=self.classes_.dtype)` before the normal `np.searchsorted` path."
      why: "The early return handles the empty input while leaving the non-empty code path untouched, so the fail-to-pass test passes and all pass-to-pass tests stay green."
    failure:
      answer: "Replace the internal `np.searchsorted` mapping with a Python list comprehension over `self.classes_.tolist()` to sidestep the empty-array error."
      why: "The empty case now passes, but the rewrite changes the return dtype and raises on unseen labels differently, breaking existing pass-to-pass tests — a regression, so the issue counts unresolved (broke previously-passing tests)."
    checks: "Fixing the reported bug with a minimal change that does not disturb the tests already passing."
    scoring: "Resolve rate on hidden tests: fail-to-pass green AND pass-to-pass all green vs a pass-to-pass regression."
  - task_type: "sympy — root cause vs symptom"
    input: "sympy issue: simplifying an expression drops a term when a coefficient is exactly zero, giving a wrong result for a specific symbolic input. Patch so the term is preserved."
    success:
      answer: "Fix the collection routine in the simplification pass so a zero coefficient is handled by the general term-combination logic, not special-cased away, and add the reported input as a regression test."
      why: "Correcting the underlying term-combination logic fixes the reported case and the whole family of zero-coefficient inputs, so the fail-to-pass test and neighboring symbolic tests pass."
    failure:
      answer: "Special-case the exact failing expression: `if expr == reported_case: return known_answer` near the entry point."
      why: "The one hidden fail-to-pass test may use a different but equivalent expression; the hard-coded guard misses it, so the test still fails — patching the symptom for one literal input, not the root cause."
    checks: "Diagnosing the underlying defect rather than pattern-matching the single reported reproduction."
    scoring: "Resolve rate on hidden tests: fail-to-pass green vs still failing on an equivalent-but-different input."
  - task_type: "matplotlib — off-by-one in a boundary computation"
    input: "matplotlib issue: a histogram with `bins=N` occasionally places the maximum data value into an out-of-range bin index, raising IndexError on the largest sample. Patch the binning so the max value lands in the last bin."
    success:
      answer: "Clamp the computed index at the upper edge: `idx = min(int((x - lo) / width), N - 1)` so a value equal to the right edge maps to bin N-1 rather than N."
      why: "The clamp handles the exact-max floating-point edge, so the fail-to-pass IndexError test passes and existing binning tests remain green."
    failure:
      answer: "Wrap the bin assignment in `try/except IndexError: pass` to swallow the error."
      why: "The exception no longer propagates, but the max sample is silently dropped from the histogram, so the fail-to-pass test — which asserts the value lands in the last bin — still fails (off-by-one hidden, not fixed)."
    checks: "Correcting the boundary index rather than suppressing the symptom it produces."
    scoring: "Resolve rate on hidden tests: fail-to-pass green vs still failing / count assertion wrong."
  - task_type: "requests — preserve public API contract"
    input: "requests issue: `Session.merge_environment_settings` mutates the passed-in proxies dict, leaking state across requests. Patch so the caller's dict is not modified, and pass the added regression test."
    success:
      answer: "Build a new merged dict from a copy: `merged = {**environ_proxies, **(proxies or {})}` and use that, leaving the caller's `proxies` argument untouched."
      why: "Copying before merge fixes the mutation while preserving the merge semantics every other test depends on, so fail-to-pass and pass-to-pass both stay green."
    failure:
      answer: "Fix the leak by having the method return early and skip the environment merge entirely when `proxies` is provided."
      why: "No more mutation, but skipping the environment merge changes documented behavior (env proxies no longer applied), breaking pass-to-pass tests that expect the merge — unresolved from a contract regression."
    checks: "Fixing a side-effect bug without altering the method's documented merge contract."
    scoring: "Resolve rate on hidden tests: fail-to-pass green AND pass-to-pass all green vs a behavior regression."
tags: [code, agentic, real-world, patch, execution]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2310.06770", title: "SWE-bench: Can Language Models Resolve Real-World GitHub Issues? (Jimenez et al., 2023)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/2310.06770", title: "SWE-bench paper, full text (ar5iv)", trust: 0.95 }
  - { url: "https://openai.com/index/introducing-swe-bench-verified/", title: "Introducing SWE-bench Verified (OpenAI)", trust: 0.9 }
  - { url: "https://www.swebench.com/", title: "SWE-bench leaderboards (Verified 500, Lite 300 instances)", trust: 0.9 }
related: [benchmark/humaneval, benchmark/mbpp, benchmark/deepswe]
---

## What It Tests
**Software engineering on real code**: read an issue, find the relevant code in a large repository, and write a patch that resolves it.

## How It Scores
An instance is **resolved** when the patch applies and all of its tests pass.

- **Fail-to-pass tests** — at least one per instance, from the reference fix.
- **Headline** — % of instances resolved.

## Format
The model, usually inside an agent scaffold, outputs a patch; the harness applies it and runs the repository's tests.

## Limitations
- **System, not model.** Scaffold and tools shape the score.
- **Split matters.** Full set, Verified (500) and Lite (300) differ.
- **Python only.** Twelve Python repositories.

## Example
A Django issue: `QuerySet.bulk_create()` silently ignores `update_conflicts=True` on backends that do
not support it. A resolving patch adds a guard in `django/db/models/query.py` that raises
`NotSupportedError` when the backend lacks `supports_update_conflicts`, plus a regression test. Adding
a mere warning `print` does not make the fail-to-pass test pass, so it scores as unresolved.
