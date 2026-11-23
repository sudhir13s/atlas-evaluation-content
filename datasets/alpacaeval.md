---
id: evaluation-dataset/alpacaeval
type: evaluation-dataset
slug: alpacaeval
schema_version: 4.0
title: AlpacaEval
name: AlpacaEval (Length-Controlled)
category: LLM-as-Judge
size: 805 instruction prompts
url: https://github.com/tatsu-lab/alpaca_eval
description: Automated instruction-following evaluation via win-rate against a reference model, judged by an LLM.
what_it_tests: 'General instruction-following quality: how often a model''s answer is preferred over a reference model''s answer on 805 diverse single-turn instructions.'
why_it_exists: To give a fast, cheap, reproducible proxy for human preference that correlates highly with Chatbot Arena — without waiting for thousands of human votes. Version 2.0 adds a length-control correction to fix a major bias.
how_to_use: Generate answers to the 805 prompts, then an LLM judge compares each to the reference model's answer; the metric is the win rate (%). Use the length-controlled (LC) win rate, which regresses out response length so verbosity does not inflate the score.
known_issues: The raw win rate has a strong length bias — longer answers win regardless of quality — which LC win rate corrects but does not fully eliminate. As an LLM-judged metric it inherits judge biases, and only tests single-turn instruction following.
tags:
- llm-as-judge
- instruction-following
- win-rate
updated: '2026-07-17'
sources:
- url: https://github.com/tatsu-lab/alpaca_eval
  title: tatsu-lab/alpaca_eval (GitHub)
  trust: 0.9
- url: https://arxiv.org/abs/2404.04475
  title: 'Length-Controlled AlpacaEval: A Simple Way to Debias Automatic Evaluators (Dubois et al., 2024)'
  trust: 0.9
related:
- evaluation-dataset/mt-bench
- evaluation-dataset/chatbot-arena
---

## What It Is

An automatic evaluator that scores instruction-following by pitting a model's answers against a fixed
reference model's answers on 805 prompts, with an LLM acting as the judge. It is tuned to correlate
with human preference from Chatbot Arena while being far cheaper to run.

## How It Evaluates

For each prompt, the model and the reference (e.g. a GPT-4-class model) both answer; the judge picks
the better one. The score is the **win rate** over all 805 prompts. AlpacaEval 2.0's headline metric
is the **length-controlled (LC) win rate**, which fits a regression to remove the effect of answer
length so a model cannot win by simply writing more.

## What It Looks For

A quick, reproducible estimate of general instruction-following preference — a stand-in for human
voting when speed and repeatability matter (e.g. during model development).

## Known Issues

Length bias is the defining problem: raw win rate rewards verbosity, which LC win rate corrects but
not perfectly. It is an LLM-judged metric, so it carries judge biases, and it only covers single-turn
instructions, not multi-turn dialogue or correctness-critical tasks.
