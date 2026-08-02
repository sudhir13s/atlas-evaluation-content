---
id: benchmark/mmlu-prox
type: benchmark
slug: mmlu-prox
schema_version: 1.0
title: "MMLU-ProX"
name: MMLU-ProX
category: Multilingual
description: "MMLU-Pro's harder 10-option reasoning questions extended to 29 languages."
what_is_it: "MMLU-ProX takes MMLU-Pro — the reasoning-heavy, 10-choice successor to MMLU — and produces the same 11,829 questions in 29 typologically diverse languages via LLM translation plus expert review."
why_people_use_it: "It is the multilingual test that is still hard at the frontier: unlike translated MMLU (near-saturated), MMLU-ProX keeps MMLU-Pro's difficulty, so it exposes reasoning gaps across languages rather than just recall gaps."
examples:
  - "A university-level physics or economics MMLU-Pro item with 10 answer options, presented identically in Swahili, Thai, Hindi, and 26 other languages."
method: "Few-shot chain-of-thought multiple-choice over 11,829 parallel questions per language (a 658-question lite split also exists); the final answer letter is parsed from the reasoning output."
metric: "Accuracy (% correct), per language and averaged."
format: "10-way multiple-choice, chain-of-thought; run few-shot (inherits the MMLU-Pro protocol)."
saturation: medium
limitations: "Translations are LLM-generated then expert-reviewed rather than fully human-authored, so subtle translationese can remain. It is a translation of English-origin questions (no culturally-native content), and like MMLU-Pro its 10-option chain-of-thought format makes scores sensitive to answer parsing and prompt template."
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
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2503.10497", title: "MMLU-ProX: A Multilingual Benchmark for Advanced Large Language Model Evaluation (Xuan et al., 2025)", trust: 0.95 }
  - { url: "https://github.com/weihao1115/MMLU-ProX", title: "weihao1115/MMLU-ProX — official EMNLP 2025 repo", trust: 0.9 }
related: [benchmark/mmlu-pro, benchmark/mmmlu, benchmark/mmlu]
---

## What It Tests
Advanced, reasoning-heavy academic knowledge — the same 10-option, distractor-hardened questions
as MMLU-Pro — evaluated in 29 languages. Because it inherits MMLU-Pro's difficulty, it probes
whether a model can *reason* in a language, not merely recall facts in it.

## How It Scores
Accuracy over 11,829 identical questions per language, reported per language and averaged; a
658-question lite split supports cheap iteration. The paper's headline finding is the disparity:
strong performance in high-resource languages but drops of up to 24.3 percentage points in
low-resource ones.

## Format
Ten-way multiple choice with chain-of-thought, run few-shot — the MMLU-Pro reference protocol.
The ten options push the random baseline to ~10% and the reasoning requirement means the answer
is parsed from a generated chain, not a single token.

## Limitations
Questions are translated (LLM-then-expert-review, not fully human), so residual translationese
and English-origin cultural framing remain. As with any chain-of-thought multiple-choice test,
answer-letter parsing and prompt formatting introduce measurement noise, and cross-report
comparisons must fix the same shot count and template.

## Example
A graduate-level economics MMLU-Pro item — "Which of the following best explains why a
monopolistically competitive firm earns zero economic profit in the long run?" among ten
options — appears in each of the 29 languages. A model scoring 78% in German but 54% in Swahili
on the identical question set reveals a reasoning-in-language gap.
