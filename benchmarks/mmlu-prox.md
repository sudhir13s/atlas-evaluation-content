---
id: benchmark/mmlu-prox
type: benchmark
slug: mmlu-prox
schema_version: 1.0
title: "MMLU-ProX"
name: MMLU-ProX
category: Multilingual
description: "MMLU-Pro's 10-option questions carried into 29 languages, with the same 11,829 questions in every language."
what_is_it: "MMLU-ProX (Xuan et al., 2025) is a multilingual benchmark built on the English MMLU-Pro. Each of its 29 language versions has the same 11,829 questions, translated by several LLMs and then reviewed by experts; a lite version has 658 questions per language."
why_people_use_it: "Because every language has the identical question set, scores can be compared directly across languages. The authors report that the 36 models they tested do well in high-resource languages and decline in low-resource ones, with gaps of up to 24.3% (paper, 2025)."
examples:
  - "An MMLU-Pro item with ten answer options, presented with the same content in each of the 29 languages."
method: "Following MMLU-Pro, the paper primarily uses 5-shot chain-of-thought (CoT) prompting and also reports 0-shot results. 70 validation questions per language build the few-shot prompts, so assessment runs on 11,759 questions (full) or 588 (lite)."
metric: "Accuracy (% correct), per language and averaged."
format: "Multiple choice with ten options (inherited from MMLU-Pro); 5-shot CoT as the main setting."
limitations: "The questions are translations of an English benchmark (LLM translation followed by expert review), not material written in each language. The paper reports clear differences between 0-shot and 5-shot results, so a score needs its prompting setting."
citation: "https://arxiv.org/abs/2503.10497"
worked_examples:
  - task_type: "Advanced economics reasoning (Swahili)"
    input: "Kwa nini kampuni katika ushindani wa kimonopoli hupata faida sifuri ya kiuchumi kwa muda mrefu? (Why does a monopolistically competitive firm earn zero economic profit in the long run?)"
    options:
      - "Kwa sababu kuingia bila kizuizi kwa makampuni mapya hushusha faida hadi bei = wastani wa gharama. (Free entry of new firms drives profit down until price = average total cost.)"
      - "Kwa sababu kampuni huweka mapato ya nyongeza sawa na gharama ya nyongeza. (The firm sets marginal revenue = marginal cost.)"
      - "Kwa sababu serikali hudhibiti bei. (The government fixes the price.)"
      - "Kwa sababu bidhaa zinafanana kikamilifu. (The products are perfectly identical.)"
      - "Kwa sababu kampuni ina nguvu kamili ya soko. (The firm has full market power.)"
      - "Kwa sababu gharama za kudumu ni sifuri. (Fixed costs are zero.)"
    success:
      answer: "A — Free entry of new firms drives profit down until price = average total cost"
      why: "Names the actual long-run zero-profit driver — entry competing away profit — reasoned correctly in-language."
    failure:
      answer: "B — The firm sets marginal revenue equal to marginal cost"
      why: "MR = MC sets the profit-maximizing output level, not the zero-profit condition; a plausible in-language distractor and a reasoning error, not a translation error."
    checks: "Reasoning through a distractor-hardened economics question in a lower-resource language, not mere fact recall."
    scoring: "Accuracy (% correct); random baseline ~10% on 10 options."
  - task_type: "Physics reasoning (Thai)"
    input: "วัตถุตกอิสระจากหยุดนิ่ง หลังจาก 3 วินาที ความเร็วเป็นเท่าใด (g = 10 m/s²)? (An object falls freely from rest; what is its speed after 3 s, with g = 10 m/s²?)"
    options:
      - "10 m/s"
      - "15 m/s"
      - "20 m/s"
      - "25 m/s"
      - "30 m/s"
      - "33 m/s"
      - "45 m/s"
      - "60 m/s"
      - "90 m/s"
      - "300 m/s"
    success:
      answer: "E — 30 m/s"
      why: "Speed under constant acceleration is v = gt = 10 × 3 = 30 m/s."
    failure:
      answer: "G — 45 m/s"
      why: "Uses ½gt² = 45 (the fallen distance) as if it were the speed — a kinematics reasoning slip that has nothing to do with the Thai wording."
    checks: "Selecting the right kinematic relation (v = gt) among numeric distractors in a lower-resource language."
    scoring: "Accuracy (% correct); random baseline ~10% on 10 options."
  - task_type: "Chemistry stoichiometry (Bengali)"
    input: "STP-তে 2 মোল আদর্শ গ্যাসের আয়তন কত? (What is the volume of 2 mol of an ideal gas at STP?)"
    options:
      - "1 L"
      - "2 L"
      - "4 L"
      - "5.6 L"
      - "11.2 L"
      - "22.4 L"
      - "33.6 L"
      - "44.8 L"
      - "67.2 L"
      - "89.6 L"
    success:
      answer: "H — 44.8 L"
      why: "The molar volume at STP is 22.4 L/mol, so 2 mol occupies 2 × 22.4 = 44.8 L."
    failure:
      answer: "F — 22.4 L"
      why: "Uses the 1-mol molar volume and forgets to multiply by the 2 mol amount — an arithmetic reasoning error independent of the Bengali script."
    checks: "Applying molar volume with the correct amount factor among numeric options in a lower-resource language."
    scoring: "Accuracy (% correct); random baseline ~10% on 10 options."
  - task_type: "Discrete probability reasoning (Chinese)"
    input: "从1到20的整数中随机取一个数，它能被2或3整除的概率是多少？ (Pick an integer from 1 to 20 at random; what is the probability it is divisible by 2 or 3?)"
    options:
      - "3/20"
      - "6/20"
      - "7/20"
      - "9/20"
      - "10/20"
      - "13/40"
      - "13/20"
      - "16/20"
      - "1/2"
      - "20/20"
    success:
      answer: "G — 13/20"
      why: "By inclusion–exclusion: multiples of 2 = 10, of 3 = 6, of 6 = 3, so 10 + 6 − 3 = 13 favorable out of 20."
    failure:
      answer: "H — 16/20"
      why: "Adds 10 + 6 without subtracting the 3 numbers counted twice (multiples of 6) — the classic inclusion–exclusion omission, not a translation issue."
    checks: "Carrying out inclusion–exclusion correctly among close numeric distractors in-language."
    scoring: "Accuracy (% correct); random baseline ~10% on 10 options."
  - task_type: "Microeconomics reasoning (German)"
    input: "Wenn der Preis um 10 % steigt und die nachgefragte Menge um 20 % fällt, wie groß ist der Betrag der Preiselastizität der Nachfrage? (If price rises 10% and quantity demanded falls 20%, what is the magnitude of the price elasticity of demand?)"
    options:
      - "0.2"
      - "0.5"
      - "1"
      - "1.5"
      - "2"
      - "2.5"
      - "3"
      - "10"
      - "20"
      - "200"
    success:
      answer: "E — 2"
      why: "Elasticity magnitude = |%ΔQ / %ΔP| = 20 / 10 = 2."
    failure:
      answer: "B — 0.5"
      why: "Inverts the ratio to %ΔP / %ΔQ = 10 / 20 = 0.5 — a formula-direction reasoning error, unrelated to the German phrasing."
    checks: "Applying the elasticity formula in the correct direction among numeric distractors in a high-resource language."
    scoring: "Accuracy (% correct); random baseline ~10% on 10 options."
tags: [multilingual, knowledge, reasoning, multiple-choice]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2503.10497", title: "MMLU-ProX: A Multilingual Benchmark for Advanced Large Language Model Evaluation (Xuan et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2503.10497", title: "MMLU-ProX, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://github.com/weihao1115/MMLU-ProX", title: "weihao1115/MMLU-ProX — official EMNLP 2025 repo", trust: 0.9 }
related: [benchmark/mmlu-pro, benchmark/mmmlu, benchmark/mmlu]
---

## What It Tests
Academic knowledge and reasoning on MMLU-Pro's questions, asked in **29 languages**.

- **Parallel** — the same 11,829 questions in every language.
- **Cross-lingual comparison** — differences between languages show directly.

## How It Scores
**Accuracy**, per language and averaged.

- **Full and lite** — 11,829 or 658 questions per language; 70 of them build the few-shot prompts.
- **Paper's finding** — gaps of up to 24.3% between high- and low-resource languages (2025).

## Format
Multiple choice with **ten options**, as in MMLU-Pro. The paper's main setting is 5-shot chain-of-thought; it also reports 0-shot.

## Limitations
- **Translated content.** LLM translation plus expert review of an English benchmark.
- **Setting-dependent.** 0-shot and 5-shot results differ, so a score needs its setting.

## Example
An MMLU-Pro question with ten options appears with the same content in each of the 29 languages, and the model's chosen option is scored against the key.
