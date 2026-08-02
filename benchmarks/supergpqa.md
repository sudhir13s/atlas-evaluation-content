---
id: benchmark/supergpqa
type: benchmark
slug: supergpqa
schema_version: 1.0
title: "SuperGPQA"
name: SuperGPQA
category: Knowledge & Academic Reasoning
description: "A 26,529-question graduate-level benchmark spanning 285 disciplines, with up to 10 answer options per question to defeat guessing."
what_is_it: "A large-scale successor to GPQA and MMLU-Pro that scales expert-authored, graduate-level questions to 285 subfields — including long-tail disciplines like agronomy and light industry — with an average of ~9.67 options per item to make lucky guessing nearly worthless."
why_people_use_it: "It restores headroom at the frontier: where GPQA has only 448 questions and MMLU-Pro ~12K over 14 fields, SuperGPQA covers 285 subfields at 26K+ questions, so it discriminates among top reasoning models that saturate narrower knowledge tests."
examples:
  - "A graduate materials-science item asking which crystal defect mechanism dominates creep at a given temperature range, presented with roughly ten expert-written options where several are subtly plausible."
method: "Zero-shot evaluation over 26,529 multiple-choice questions with up to 10 options each; built via a Human-LLM collaborative pipeline (80+ expert annotators) that adds distractors and filters trivial or ambiguous items."
metric: "Accuracy (% correct), reported overall and sliced by discipline, field, subfield, and difficulty (easy/medium/hard)."
format: "Multiple-choice with an average of ~9.67 options (up to 10); zero-shot, chain-of-thought optional."
saturation: low
limitations: "New (Feb 2025) so contamination is limited but not zero. Many-option distractors make answer parsing and prompt template matter. Some long-tail subfields have thin coverage, and it remains a knowledge-and-reasoning multiple-choice test — no generation, tools, or long context."
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
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2502.14739", title: "SuperGPQA: Scaling LLM Evaluation across 285 Graduate Disciplines (M-A-P & ByteDance, 2025)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/m-a-p/SuperGPQA", title: "SuperGPQA dataset (m-a-p/SuperGPQA)", trust: 0.9 }
  - { url: "https://github.com/SuperGPQA/SuperGPQA", title: "SuperGPQA GitHub repository", trust: 0.85 }
related: [benchmark/gpqa, benchmark/gpqa-diamond, benchmark/mmlu-pro]
---

## What It Tests
Graduate-level knowledge and reasoning at breadth no prior benchmark reached: **13 disciplines, 72
fields, and 285 subfields**. Crucially it reaches long-tail areas — agronomy, light industry,
service science, military science — that mainstream tests ignore.

Each of the 26,529 questions is expert-authored and multiple-choice, but with an average of **~9.67
options** (up to 10). That wide option set drops the random baseline near 10% and blunts process-of-
elimination, forcing genuine reasoning.

## How It Scores
Accuracy over the full set, reported overall and sliced by discipline, field, subfield, and a
difficulty label (easy / medium / hard) attached to every item. The difficulty slices let you see
whether a model's edge is uniform or concentrated in easy questions.

At release the strongest reasoning models scored only in the low-60s percent, leaving large headroom
— which is the point of building it.

## Format
Zero-shot multiple-choice; the model reads the question plus up to ten options and emits a letter,
optionally with chain-of-thought. Questions were produced by a Human-LLM collaborative pipeline —
80+ expert annotators write and refine items, add plausible distractors, and iteratively filter out
trivial or ambiguous ones.

## Limitations
It is knowledge-and-reasoning multiple-choice only — it says nothing about generation, agentic
behavior, or long context. Being new (Feb 2025) its contamination is low today but will grow. The
many-option format makes scores sensitive to answer parsing and prompt template, and some long-tail
subfields carry relatively few questions, so those per-subfield estimates are noisier.

## Example
A graduate metallurgy item might ask which deformation mechanism governs creep in a nickel
superalloy over a stated temperature and stress regime, offering roughly ten mechanisms as options.
Several distractors (dislocation glide, grain-boundary sliding, Coble vs. Nabarro-Herring diffusion
creep) are each correct in *some* regime, so a model must reason about the specific conditions rather
than recognize a keyword — and with ten options, guessing after eliminating two or three still leaves
it badly exposed.
