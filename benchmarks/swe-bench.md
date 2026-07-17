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
tags: [code, agentic, real-world, patch, execution]
updated: 2026-07-17
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
