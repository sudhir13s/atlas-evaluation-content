---
id: benchmark/aider-polyglot
type: benchmark
slug: aider-polyglot
schema_version: 3.1
title: "Aider Polyglot"
name: Aider polyglot
category: Code Generation
description: "225 hard Exercism coding exercises in C++, Go, Java, JavaScript, Python and Rust, solved through the Aider coding assistant and checked by unit tests."
what_is_it: "Aider's polyglot benchmark tests how well a model follows instructions and edits code without human help, inside the Aider coding assistant. It uses the 225 hardest of the 697 Exercism exercises in six languages: C++ (26), Go (39), Java (47), JavaScript (49), Python (34) and Rust (30). An exercise was kept if 3 or fewer of the seven models Aider tried at launch solved it. Aider launched it on 2024-12-21, maintains it, and publishes results on its LLM leaderboards page; the exercises are in the public Aider-AI/polyglot-benchmark repository."
why_people_use_it: "Aider built it because its original 133-problem Python benchmark was saturating, with top scores above 80%. It measures both writing correct code and producing edits in a format the tool can apply, across several languages."
method: "Aider sends the model the exercise's Exercism instructions and the stub solution file(s); the unit tests are not shown. The tests are then run. If they fail, the test output is sent back and the model gets a second try. The harness runs inside a Docker container. Each run records the model, the edit format and the Aider commit, which Aider says are enough to reproduce it."
metric: "Percent correct: the share of the 225 exercises whose unit tests all pass by the second try (pass_rate_2; higher is better). The leaderboard also lists the share of cases with a correct edit format and the run's cost; each entry's details give the first-try pass rate."
format: "Agentic code editing through Aider: instructions plus stub files, hidden unit tests, up to two tries with test feedback; the edit format (for example whole, diff or architect) is part of each result."
limitations: "Results measure the model together with Aider's prompts and the chosen edit format: Aider tests the ability to format code edits as well as write code, so a correct solution in an edit Aider cannot apply does not count. A result is tied to its model, edit format and Aider commit, and the same model can appear with several edit formats."
citation: "https://aider.chat/2024/12/21/polyglot.html"
openness: open
openness_note: "The 225 exercises with their instructions, stubs and unit tests are public in the Aider-AI/polyglot-benchmark repository (used under Exercism's open-source licences), and the harness that scores them is public in the Aider repository."
examples:
  - "Python exercise book-store: price a basket of books from a five-book series where one copy costs $8 and sets of different titles earn discounts from 5% to 25%, choosing the grouping with the largest discount."
tags: [code, code-editing, multi-language, exercism, aider]
updated: 2026-11-23
sources:
  - { url: "https://aider.chat/docs/leaderboards/", title: "Aider LLM Leaderboards", trust: 0.95 }
  - { url: "https://aider.chat/2024/12/21/polyglot.html", title: "o1 tops aider's new polyglot leaderboard (Aider, 2024-12-21)", trust: 0.95 }
  - { url: "https://github.com/Aider-AI/aider/blob/main/benchmark/README.md", title: "Aider benchmark harness README (Aider-AI/aider)", trust: 0.95 }
  - { url: "https://github.com/Aider-AI/aider/blob/main/benchmark/benchmark.py", title: "Aider benchmark harness source, benchmark.py (Aider-AI/aider)", trust: 0.95 }
  - { url: "https://github.com/Aider-AI/polyglot-benchmark", title: "Aider polyglot benchmark exercises (Aider-AI/polyglot-benchmark)", trust: 0.95 }
  - { url: "https://github.com/Aider-AI/aider/blob/main/aider/website/_data/polyglot_leaderboard.yml", title: "Aider polyglot leaderboard data file (Aider-AI/aider)", trust: 0.95 }
  - { url: "https://aider.chat/docs/benchmarks.html", title: "GPT code editing benchmarks (Aider, original benchmark method)", trust: 0.85 }
  - { url: "https://raw.githubusercontent.com/Aider-AI/polyglot-benchmark/main/python/exercises/practice/book-store/.docs/instructions.md", title: "book-store exercise instructions (Aider-AI/polyglot-benchmark)", trust: 0.9 }
related: [benchmark/swe-bench]
---

## What It Tests
Whether a model can **write and edit code that passes unit tests** in six languages, driven through the Aider coding assistant.

- **Exercises** — 225 Exercism problems in C++, Go, Java, JavaScript, Python and Rust.
- **Hard by selection** — kept only if 3 or fewer of seven launch models solved them.
- **Editing** — the model must return changes in a format Aider can apply.
- **Per language** — C++ 26, Go 39, Java 47, JavaScript 49, Python 34, Rust 30.
- **Data** — exercises in the public `Aider-AI/polyglot-benchmark` repository, used under Exercism's open-source licences.

## How It Scores
**Percent correct** = exercises whose unit tests all pass by the second try / 225.

- **Two tries** — after a failure the model sees the test output and tries once more.
- **Headline** — pass rate after the second try (pass_rate_2).
- **Also shown** — correct edit format rate, cost, and the first-try pass rate in each entry's details.

## Format
Aider sends the instructions and stub files; tests stay hidden from the model. The harness runs in Docker, and each result names its edit format.

## Limitations
- **System, not model alone.** Aider's prompts and edit format are part of the score.
- **Format counts.** An edit Aider cannot apply fails even if the code idea is right.
- **Setup-dependent.** One model may have several entries, one per edit format.

## Example
From the public repository, the Python exercise **book-store**. Its instructions say: "One copy of any of the five books costs $8. If, however, you buy two different books, you get a 5% discount on those two books." Larger sets of different titles earn bigger discounts, up to 25%.

- **Task** — fill in the stub so it returns the lowest price for any basket, choosing the best grouping.
- **Scoring** — Aider applies the model's edit and runs the hidden unit tests; on a failure the model sees the test output and tries once more.
- **Counts** — only if all tests pass by that second try.
