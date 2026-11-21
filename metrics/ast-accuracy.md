---
id: evaluation-metric/ast-accuracy
type: evaluation-metric
slug: ast-accuracy
schema_version: 3.0
title: "AST Accuracy (Function Calling)"
name: "AST evaluation accuracy"
category: agent
measures: "A high score means the model's function calls name the right function with the right parameters, types and values."
needs: [function documentation, user prompt, model function call, possible correct answers]
judged_by: code
direction: higher-is-better
defined_in: { title: "Berkeley Function-Calling Leaderboard (Gorilla blog, Yan et al.)", url: "https://gorilla.cs.berkeley.edu/blogs/8_berkeley_function_calling_leaderboard.html" }
tags: [agent, tool-use, function-calling]
updated: 2026-11-21
sources:
  - { url: "https://gorilla.cs.berkeley.edu/blogs/8_berkeley_function_calling_leaderboard.html", title: "Berkeley Function-Calling Leaderboard (Gorilla blog)", trust: 0.9 }
related: [benchmark/bfcl-v4]
---

## What It Measures
Abstract syntax tree (AST) evaluation is how the Berkeley Function-Calling Leaderboard (BFCL) scores **tool-call correctness** without running the call. The share of test cases that pass is reported as accuracy.

## How It Is Computed
- **Parse:** the generated call is parsed into an abstract syntax tree.
- **Function name:** must match a name in the possible answers.
- **Parameters:** all required parameters must be present, and only parameters in the function documentation may appear; extras are flagged as hallucinated.
- **Types and values:** strict type checks; values must match a possible answer. Strings are compared case-insensitively with whitespace and some punctuation removed; lists are order-sensitive; dictionary key order is ignored.
- **Multiple calls:** if any expected call finds no match, the test case fails.
- **Companion method:** executable evaluation runs the call and compares its result with the expected one.

## Limits
The leaderboard's authors state:

- **AST checking complements execution,** used because not every function is easily executable (for example Java functions).
- **REST API tests skip AST checking,** since the possible answers for complex APIs cannot all be listed.
