---
id: benchmark/c-eval
type: benchmark
slug: c-eval
schema_version: 1.0
title: "C-Eval"
name: C-Eval
category: Knowledge & Academic Reasoning
description: "A Chinese evaluation suite of 13,948 four-choice exam questions across 52 disciplines and four difficulty levels, with a harder C-Eval Hard subset."
what_is_it: "C-Eval (Huang et al., 2023; Shanghai Jiao Tong University, Tsinghua University, University of Edinburgh and others) tests advanced knowledge and reasoning in a Chinese context. Its 13,948 multiple-choice questions span 52 disciplines from humanities to science and engineering, at middle school, high school, college and professional level. C-Eval Hard is a subset of very challenging subjects, such as advanced mathematics and college physics."
why_people_use_it: "The authors present it as the first comprehensive Chinese evaluation suite for foundation models, built to compare English- and Chinese-oriented models in a Chinese context (2023 paper)."
examples:
  - "A college-level advanced-mathematics item asking to evaluate a limit or choose the correct derivative, presented in Chinese with four options (A-D) and one correct answer."
method: "The paper evaluates zero-shot and five-shot answer-only prompting and five-shot chain-of-thought prompting, with the five exemplars taken from the development split; a regular expression extracts the chosen letter. Test-split labels are private: the paper has users submit test predictions to the authors' leaderboard site for scoring."
metric: "Accuracy (% correct), averaged over subjects; C-Eval Hard is reported separately."
format: "Four-choice multiple-choice in Chinese; splits are dev (260), validation (1,346) and test (12,342)."
limitations: "Exam questions from national tests are often widely available online, so the authors kept test labels private to limit contamination. Multiple choice tests picking an answer, not writing Chinese text."
citation: "https://arxiv.org/abs/2305.08322"
worked_examples:
  - task_type: "College advanced mathematics (item in Chinese; shown here in English)"
    input: "Evaluate the definite integral ∫₀^π sin(x) dx."
    options:
      - "0"
      - "1"
      - "2"
      - "−2"
    success:
      answer: "C — 2"
      why: "The antiderivative is −cos(x); [−cos x]₀^π = (−cos π) − (−cos 0) = 1 + 1 = 2."
    failure:
      answer: "D — −2"
      why: "It uses +cos(x) as the antiderivative instead of −cos(x), flipping the sign to give cos π − cos 0 = −2 — a classic sign error."
    checks: "Calculus reasoning on a Chinese-language exam item; decoding the language is not enough, the answer turns on the math."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "High-school chemistry (item in Chinese)"
    input: "How many moles of electrons are transferred when 1 mol of Cl₂ is completely reduced to Cl⁻?"
    options:
      - "1 mol"
      - "2 mol"
      - "0.5 mol"
      - "4 mol"
    success:
      answer: "B — 2 mol"
      why: "1 mol Cl₂ contains 2 mol of Cl atoms, and each gains one electron (Cl₂ + 2e⁻ → 2Cl⁻), so 2 mol of electrons are transferred."
    failure:
      answer: "A — 1 mol"
      why: "It counts one electron per Cl₂ molecule, forgetting that the molecule has two chlorine atoms each gaining an electron."
    checks: "Balancing electron transfer in a redox half-reaction, tracking atoms per molecule."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Chinese history (item in Chinese)"
    input: "Which dynasty first unified China and standardized its writing system, currency, and weights and measures?"
    options:
      - "Han dynasty"
      - "Qin dynasty"
      - "Tang dynasty"
      - "Zhou dynasty"
    success:
      answer: "B — Qin dynasty"
      why: "Qin Shi Huang unified China in 221 BCE and imposed standardized script, currency, and measures across the empire."
    failure:
      answer: "A — Han dynasty"
      why: "The Han consolidated and expanded the empire but came after the Qin, which was the first to unify and standardize — a common ordering slip."
    checks: "Recall of a foundational fact of Chinese history and the correct dynastic sequence."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Civics and law / moral cultivation (item in Chinese)"
    input: "Under China's Constitution, which body is the highest organ of state power and holds the authority to amend the Constitution?"
    options:
      - "The State Council"
      - "The National People's Congress"
      - "The Supreme People's Court"
      - "The Central Military Commission"
    success:
      answer: "B — The National People's Congress"
      why: "The NPC is defined as the highest organ of state power and is the body empowered to enact and amend the Constitution."
    failure:
      answer: "A — The State Council"
      why: "The State Council is the executive (administrative) organ, not the legislature — a common confusion of executive and legislative roles."
    checks: "Knowledge of the structure of the Chinese state and separation of governing bodies."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Middle-school biology (item in Chinese)"
    input: "Which organelle is the primary site of photosynthesis in a plant cell?"
    options:
      - "Mitochondrion"
      - "Chloroplast"
      - "Nucleus"
      - "Ribosome"
    success:
      answer: "B — Chloroplast"
      why: "Chloroplasts contain chlorophyll and carry out photosynthesis, converting light energy into chemical energy."
    failure:
      answer: "A — Mitochondrion"
      why: "The mitochondrion is the site of cellular respiration, not photosynthesis — the two energy pathways are frequently swapped."
    checks: "Matching an organelle to its function and distinguishing photosynthesis from respiration."
    scoring: "Exact-match accuracy on the selected option."
tags: [knowledge, multiple-choice, chinese, multilingual, reasoning]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2305.08322", title: "C-Eval: A Multi-Level Multi-Discipline Chinese Evaluation Suite for Foundation Models (Huang et al., 2023)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2305.08322", title: "C-Eval, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/ceval/ceval-exam", title: "C-Eval dataset (ceval/ceval-exam)", trust: 0.9 }
related: [benchmark/mmlu, benchmark/mmlu-pro]
---

## What It Tests
**Knowledge and reasoning in Chinese**, from Chinese exam questions.

- **52 disciplines** — humanities to science and engineering, grouped as STEM, humanities, social science and other.
- **Four levels** — middle school, high school, college and professional.
- **C-Eval Hard** — very challenging subjects such as advanced mathematics and college physics.

## How It Scores
**Accuracy**, averaged over subjects.

- **Settings in the paper** — zero- and five-shot answer-only, five-shot chain-of-thought.
- **Private test labels** — test predictions are scored by the authors' leaderboard site; dev and validation labels are public.

## Format
Every question has exactly four choices and one correct answer, in Chinese. Splits: dev 260, validation 1,346, test 12,342 (13,948 in total).

## Limitations
- **Contamination.** National exam questions are often online; test labels are kept private for that reason.
- **Multiple choice.** It does not test writing Chinese text.

## Example
Figure 2 of the paper shows a college-economics question with its four options and an English translation. A model reads the Chinese question and options and answers with one letter, which is compared with the key.
