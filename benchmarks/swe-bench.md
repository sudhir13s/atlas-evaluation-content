---
id: benchmark/swe-bench
type: benchmark
slug: swe-bench
schema_version: 1.0
title: "SWE-bench"
name: SWE-bench
category: Code Generation
description: "Real GitHub issues from popular Python repos — the model must produce a patch that passes the repo's tests."
what_is_it: "A benchmark built from ~2,300 real resolved issues across 12 popular Python repositories (Django, scikit-learn, matplotlib, sympy, …). The model is given the repo and the issue text and must generate a code patch; success means the repo's own hidden tests (that the human fix made pass) now pass. SWE-bench Verified is a ~500-issue human-validated subset."
why_people_use_it: "It is the leading measure of realistic, agentic software engineering — multi-file understanding, code navigation, and producing a working patch — not toy function completion. It is the headline benchmark for coding agents."
examples:
  - "Fix a Django issue where QuerySet.bulk_create() silently ignores update_conflicts on unsupported backends; the patch must raise NotSupportedError and pass the added regression test."
method: "Agentic: the system is given the repo + issue, explores the codebase, and emits a patch (diff). The patch is applied and the repo's fail-to-pass and pass-to-pass test sets are run. Reported on SWE-bench Verified (~500) most often."
metric: "% of issues resolved (patch makes the target tests pass without breaking existing ones)."
format: "Agentic patch generation; test-suite execution."
saturation: low
limitations: "Expensive and complex to run (per-issue Docker environments). Requires an agent scaffold, so scores reflect the whole system (model + tools + prompt), not the model alone. Python-only. Some issues have under-specified tests; the Verified subset addresses part of this. Contamination is possible for issues predating training cutoffs."
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
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2310.06770", title: "SWE-bench: Can Language Models Resolve Real-World GitHub Issues? (Jimenez et al., 2023)", trust: 0.95 }
related: [benchmark/humaneval, benchmark/mbpp]
---

## What It Tests
End-to-end software engineering on real code: read an issue, locate the relevant files in a large
repository, write a correct multi-line patch, and not break anything else. It is the closest common
benchmark to a working developer's job.

## How It Scores
An issue is **resolved** if the applied patch turns the target failing tests green (fail-to-pass)
while keeping the previously passing tests green (pass-to-pass). The headline is the percentage of
resolved issues, usually on **SWE-bench Verified** (~500 human-checked issues). Top agent systems
resolve a substantial minority — meaningful, because these are bugs that required human engineers to
diagnose.

## Format
Agentic: the model, wrapped in a scaffold that can read files and run commands, produces a unified
diff. The harness applies it and runs the repo's tests in an isolated environment.

## Limitations
Scores measure the **whole system** (model + agent scaffold + tools + prompt), so a number is not a
property of the base model alone. Running it is costly (per-issue containers). It is Python-only, some
test suites are under-specified (Verified mitigates this), and issues older than the training cutoff
risk contamination.

## Example
A Django issue: `QuerySet.bulk_create()` silently ignores `update_conflicts=True` on backends that do
not support it. A resolving patch adds a guard in `django/db/models/query.py` that raises
`NotSupportedError` when the backend lacks `supports_update_conflicts`, plus a regression test. Adding
a mere warning `print` does not make the fail-to-pass test pass, so it scores as unresolved.
