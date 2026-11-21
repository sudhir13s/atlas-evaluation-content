---
id: benchmark/supergpqa
type: benchmark
slug: supergpqa
schema_version: 1.0
title: "SuperGPQA"
name: SuperGPQA
category: Knowledge & Academic Reasoning
description: "A 26,529-question multiple-choice benchmark of graduate-level knowledge and reasoning across 285 subfields, with an average of 9.67 options per question."
what_is_it: "SuperGPQA (M-A-P Team, Du et al., 2025) covers 13 disciplines, 72 fields and 285 subfields, including specialised areas such as light industry, agriculture and service-oriented disciplines. Expert annotators collected source questions from credible resources; annotators then turned them into multiple-choice questions with added confusion options, and a Human-LLM collaborative filter removed trivial or ambiguous questions."
why_people_use_it: "The authors say existing benchmarks cover far fewer of the over 200 specialised disciplines of human knowledge. SuperGPQA is larger than GPQA (448 questions) and MMLU-Pro (12,032), and averages 9.67 options per question instead of the usual four."
examples:
  - "A multiple-choice question from one of the 285 subfields, with about ten options and one correct answer, labelled easy, middle or hard."
method: "In the paper, reasoning and chat models are evaluated zero-shot and base models five-shot, following MMLU-Pro. Over 80 expert annotators and an interactive Human-LLM system built the questions."
metric: "Accuracy (% correct), overall and by discipline, field, subfield and difficulty split (easy, middle, hard)."
format: "Multiple choice with an average of 9.67 options; zero-shot for chat and reasoning models, five-shot for base models."
limitations: "It is a multiple-choice test of knowledge and reasoning only. The paper's robustness test over 24 equivalent prompts shows scores vary with the prompt, more for smaller models, so the prompt and shot setting must be reported."
citation: "https://arxiv.org/abs/2502.14739"
worked_examples:
  - task_type: "Graduate materials science — high-temperature creep"
    input: "In a fine-grained metal held at high homologous temperature under low applied stress, which mechanism dominates creep deformation?"
    options:
      - "Nabarro–Herring diffusional creep (lattice diffusion)"
      - "Dislocation (power-law) creep"
      - "Coble creep (grain-boundary diffusion)"
      - "Grain-boundary sliding"
      - "Dislocation glide"
      - "Harper–Dorn creep"
      - "Deformation twinning"
      - "Superplastic flow"
      - "Solute-drag creep"
      - "Athermal yielding"
    success:
      answer: "A — Nabarro–Herring diffusional creep (lattice diffusion)"
      why: "At high temperature and low stress, stress-directed vacancy diffusion through the crystal lattice governs deformation."
    failure:
      answer: "B — Dislocation (power-law) creep"
      why: "Dislocation creep is the most familiar mechanism, but it dominates at higher stresses; in the specified low-stress regime it is not rate-controlling."
    checks: "Reasoning about the specific temperature/stress regime rather than recalling the best-known mechanism, with ~10 options defeating eliminate-then-guess."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Agronomy (long-tail discipline)"
    input: "A crop shows interveinal chlorosis (yellowing between the veins) on its youngest leaves while the older, lower leaves stay green. Which nutrient deficiency is most consistent with this pattern?"
    options:
      - "Nitrogen (N)"
      - "Iron (Fe)"
      - "Potassium (K)"
      - "Magnesium (Mg)"
      - "Phosphorus (P)"
      - "Calcium (Ca)"
      - "Sulfur (S)"
      - "Zinc (Zn)"
      - "Boron (B)"
      - "Molybdenum (Mo)"
    success:
      answer: "B — Iron (Fe)"
      why: "Iron is immobile in the plant, so deficiency appears first as interveinal chlorosis in the youngest, newest leaves."
    failure:
      answer: "D — Magnesium (Mg)"
      why: "Mg deficiency also causes interveinal chlorosis, but Mg is mobile and is remobilized to new growth, so its symptoms show on older leaves first; the 'young leaves' cue points to an immobile nutrient."
    checks: "Using nutrient mobility to localize a deficiency from where symptoms appear, not just the symptom type."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Electrical engineering — control systems"
    input: "A continuous-time linear time-invariant system is BIBO stable if all poles of its transfer function lie in which region of the complex s-plane?"
    options:
      - "The open left half-plane (strictly negative real part)"
      - "On the imaginary axis"
      - "The right half-plane (positive real part)"
      - "Inside the unit circle"
      - "Outside the unit circle"
      - "At the origin"
      - "On the real axis"
      - "In the upper half-plane"
      - "Anywhere with magnitude less than 1"
      - "At infinity"
    success:
      answer: "A — The open left half-plane (strictly negative real part)"
      why: "For a continuous LTI system, BIBO stability requires every pole to have a strictly negative real part so the impulse response decays."
    failure:
      answer: "D — Inside the unit circle"
      why: "The unit-circle criterion is for discrete-time (z-domain) systems; applying it to a continuous s-domain system is a common domain confusion."
    checks: "Applying the correct stability region for the continuous (s-domain) rather than discrete (z-domain) setting."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Pharmacology (long-tail subfield)"
    input: "A drug is eliminated by first-order kinetics with a half-life of 4 hours. Approximately what fraction of an initial dose remains 12 hours after administration?"
    options:
      - "1/2"
      - "1/4"
      - "1/8"
      - "1/16"
      - "1/3"
      - "1/6"
      - "1/12"
      - "0"
      - "3/4"
      - "1/9"
    success:
      answer: "C — 1/8"
      why: "12 hours is three half-lives, so the remaining fraction is (1/2)³ = 1/8."
    failure:
      answer: "G — 1/12"
      why: "It scales the dose linearly with elapsed time (dose ÷ 12 h thinking) instead of applying exponential first-order decay over three half-lives."
    checks: "Applying exponential half-life decay rather than a linear time proportion."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Food science (light-industry subfield)"
    input: "In the High-Temperature Short-Time (HTST) method of milk pasteurization, milk is held at about 72 °C for how long?"
    options:
      - "15 seconds"
      - "30 minutes"
      - "2 hours"
      - "1 second"
      - "5 minutes"
      - "10 seconds"
      - "15 minutes"
      - "3 seconds"
      - "60 seconds"
      - "30 seconds"
    success:
      answer: "A — 15 seconds"
      why: "HTST pasteurization holds milk at 72 °C for 15 seconds — hot enough and long enough to kill pathogens while preserving quality."
    failure:
      answer: "B — 30 minutes"
      why: "30 minutes at ~63 °C is the older Low-Temperature Long-Time (batch) method, not HTST — pairing the correct temperature with the wrong process's hold time."
    checks: "Matching a food-processing method to its specific time–temperature parameters, distinguishing HTST from batch pasteurization."
    scoring: "Exact-match accuracy on the selected option."
tags: [knowledge, multiple-choice, reasoning, graduate-level, frontier]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2502.14739", title: "SuperGPQA: Scaling LLM Evaluation across 285 Graduate Disciplines (M-A-P Team, 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2502.14739", title: "SuperGPQA, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/m-a-p/SuperGPQA", title: "SuperGPQA dataset (m-a-p/SuperGPQA)", trust: 0.9 }
  - { url: "https://github.com/SuperGPQA/SuperGPQA", title: "SuperGPQA GitHub repository", trust: 0.85 }
related: [benchmark/gpqa, benchmark/gpqa-diamond, benchmark/mmlu-pro]
---

## What It Tests
Graduate-level knowledge and reasoning across **13 disciplines, 72 fields and 285 subfields**, including light industry, agriculture and service-oriented disciplines.

- **26,529 questions**, multiple choice.
- **About ten options** — an average of 9.67 per question.

## How It Scores
**Accuracy**, overall and by discipline, field, subfield and difficulty split (easy, middle, hard).

## Format
Multiple choice. In the paper, chat and reasoning models answer zero-shot and base models five-shot, following MMLU-Pro. Questions came from credible sources gathered by experts, were converted to multiple choice with added confusion options, and were filtered by a Human-LLM collaborative process.

## Limitations
- **Multiple choice only.** Knowledge and reasoning, not open generation or tool use.
- **Prompt-sensitive.** Scores vary across the paper's 24 equivalent prompts, more for smaller models.

## Example
A question from one subfield is shown with its options (about ten on average); the model picks one, and the letter is compared with the key. Appendix A of the paper shows samples from each difficulty split.
