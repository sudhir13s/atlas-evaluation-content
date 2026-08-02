---
id: benchmark/bfcl-v4
type: benchmark
slug: bfcl-v4
schema_version: 1.0
title: "BFCL-V4"
name: BFCL-V4 (Berkeley Function-Calling Leaderboard v4)
category: Agent & Tool Use
description: "The de facto function-calling leaderboard, extended in v4 with agentic web search, memory, and format-sensitivity tracks."
what_is_it: "The Berkeley Function-Calling Leaderboard (BFCL) measures how well a model turns a natural-language request plus a set of API/function schemas into correct function calls. Version 4 keeps the earlier single-turn and multi-turn tracks and adds three agentic tracks — web search, memory management, and format sensitivity — so the leaderboard now scores holistic tool-use behavior, not just one-shot call formatting."
why_people_use_it: "It is the most-cited, most-reproducible function-calling benchmark, run by the Berkeley Gorilla team. Its abstract-syntax-tree (AST) matcher avoids the cost and variance of an LLM judge, so labs use it as the standard yardstick for tool-calling ability across proprietary and open models."
examples:
  - "Given schemas for get_weather(location, unit) and book_flight(origin, dest, date), and the request 'what's the weather in Paris in celsius and book me a flight there from London next Friday', emit the correct sequence of calls with correctly typed arguments."
  - "Agentic web search: answer a 3-hop multihop question by issuing search-tool calls and returning a normalized answer string."
method: "Multiple tracks. Single-turn (non-live and live/user-contributed) and multi-turn function calling are scored with a deterministic AST matcher (and executable checks where functions can be run). v4 adds agentic web search (exact-match on the normalized answer field), memory management, and format sensitivity. The headline number is an unweighted composite across categories."
metric: "Overall accuracy — an unweighted average of per-category accuracies; v4 composite weights are Agentic 40% + Multi-Turn 30% + Live 10% + Non-Live 10% + Hallucination 10%. Cost (USD) and latency are reported alongside."
format: "0-shot: the model receives the user query plus JSON function schemas and must emit calls; agentic tracks run multi-step tool loops."
saturation: medium
limitations: "AST matching rewards calls that match a reference signature, so it can miss semantically-correct-but-differently-structured calls and cannot fully judge open-ended agentic reasoning. The web-search track depends on a live third-party search API, adding non-determinism. Category weighting is a design choice that shifts rankings, and the leaderboard evolves (v1 to v4), so cross-version scores are not comparable."
citation: "https://gorilla.cs.berkeley.edu/leaderboard.html"
tags: [agent, tool-use, function-calling, leaderboard]
updated: 2026-07-20
sources:
  - { url: "https://gorilla.cs.berkeley.edu/leaderboard.html", title: "Berkeley Function-Calling Leaderboard (Gorilla, UC Berkeley)", trust: 0.95 }
  - { url: "https://proceedings.mlr.press/v267/patil25a.html", title: "The Berkeley Function Calling Leaderboard (BFCL): From Tool Use to Agentic Evaluation of Large Language Models (Patil et al., ICML 2025)", trust: 0.95 }
  - { url: "https://github.com/ShishirPatil/gorilla/blob/main/berkeley-function-call-leaderboard/README.md", title: "BFCL README — Gorilla repo", trust: 0.9 }
related: [benchmark/tau2-bench, benchmark/vita-bench, benchmark/swe-bench]
---

## What It Tests
Whether a model can read a set of function/API schemas and a natural-language request, then produce
the right calls — correct function chosen, correct argument names, correct types. Across versions the
target has widened:

- **v1** — simple, parallel, and multiple function calls.
- **v2** — live, user-contributed and enterprise functions (harder to game).
- **v3** — multi-turn and multi-step calling.
- **v4** — agentic behavior: web search, memory management, and format sensitivity.

The point of v4 is that real tool use is a loop — search, remember, re-call — not a single formatted
JSON object.

## How It Scores
The headline is **overall accuracy**, an unweighted average across sub-categories, with the v4
composite weighting **Agentic 40% + Multi-Turn 30% + Live 10% + Non-Live 10% + Hallucination 10%**.
Most tracks use a deterministic **abstract-syntax-tree (AST) matcher** that parses the predicted call
and compares it to a reference, plus executable checks where the function can actually be run. This is
why BFCL is prized for reproducibility: no LLM judge, so no judge variance. The leaderboard also reports
**cost** (USD to run the whole benchmark) and **latency**.

## Format
Zero-shot. The model is handed the user query and JSON function schemas and must emit calls with no
worked examples. The agentic tracks are larger and multi-step: v4's web-search track is built from
~100 human-crafted multihop questions (2-, 3-, and 4-hop) expanded to 200 test cases and answered via a
live search tool; the memory track spans vector-store, key-value, and summarization storage types
(hundreds of cases); format sensitivity probes ~26 prompt/format configurations across thousands of
cases.

## Limitations
AST matching scores structural agreement with a reference call, so a semantically correct call written
a different way can be marked wrong, and the matcher cannot fully evaluate open-ended agentic judgment.
The web-search track leans on a live third-party API (DuckDuckGo), which injects non-determinism the AST
tracks are designed to avoid. The composite weighting is a deliberate choice that moves rankings, and
because the leaderboard has evolved from v1 to v4, scores are not comparable across versions — always
cite the version.

## Example
Given schemas for `get_weather(location, unit)` and `book_flight(origin, destination, date)` and the
request "what's the weather in Paris in celsius, and book me a flight there from London next Friday", a
correct answer emits `get_weather(location="Paris", unit="celsius")` and
`book_flight(origin="London", destination="Paris", date="2026-08-07")`. The AST matcher checks that
both functions are selected, arguments are named correctly, and `unit` is `"celsius"` rather than a
hallucinated `"C"` — a wrong type or an invented parameter fails the case.
