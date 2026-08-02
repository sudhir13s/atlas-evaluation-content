---
id: benchmark/gpqa
type: benchmark
slug: gpqa
schema_version: 1.0
title: "GPQA"
name: GPQA
category: Frontier / Hardest
description: "Graduate-level 'Google-proof' science questions in physics, chemistry, and biology."
what_is_it: "448 hard multiple-choice questions written by PhD-level domain experts and validated to be difficult even for skilled non-experts with web access. The 'Diamond' subset (198 questions) is the highest-quality, most-cited slice."
why_people_use_it: "It is one of the few knowledge benchmarks not yet saturated. Because questions are 'Google-proof', a high score reflects genuine expert reasoning rather than retrieval or memorization."
examples:
  - "Why does the proton have a mass of ~938 MeV/c^2 despite being made of quarks with a combined bare mass of only ~9 MeV/c^2? (multiple choice; correct answer attributes ~99% of the mass to QCD binding energy)"
method: "0-shot or few-shot multiple-choice; frontier chat models are usually run 0-shot with chain-of-thought. GPQA Diamond (198 Qs) is the standard reporting subset."
metric: "Accuracy (% correct)."
format: "0-shot CoT multiple-choice (4 options)."
saturation: low
limitations: "Small dataset (448 total / 198 Diamond), so scores have wide confidence intervals. Experts occasionally disagree on gold answers. As a headline frontier metric it is now heavily optimized for, and reasoning models have begun climbing it fast — watch for contamination as it ages."
citation: "https://arxiv.org/abs/2311.12022"
example:
  task_type: "Graduate-level multiple-choice science (Google-proof)"
  input: "In quantum chromodynamics the proton weighs ~938 MeV/c^2, yet its three valence quarks have a combined bare mass of only ~9 MeV/c^2. What accounts for ~99% of the proton's mass? (A) the Higgs mechanism (B) QCD binding energy - the gluon field and virtual quark-antiquark pairs (C) electromagnetic self-energy (D) the relativistic kinetic mass of the valence quarks"
  success:
    answer: "(B) QCD binding energy"
    why: "The dynamical energy of the confined gluon field and sea quarks, confirmed by first-principles lattice-QCD, supplies almost all nucleon mass; the bare quark masses contribute only ~1%."
  failure:
    answer: "(A) the Higgs mechanism"
    why: "A sophisticated near-miss: the Higgs mechanism sets the quarks' small bare masses but not the emergent mass of the bound state - it conflates the origin of quark mass with the origin of hadron mass."
  checks: "It separates models that grasp emergent, dynamical QCD mass from those that pattern-match 'mass' to 'Higgs' - a distinction only expert-level physics reasoning reliably makes."
  scoring: "Accuracy (% correct) on GPQA Diamond."
tags: [frontier, science, multiple-choice, reasoning]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2311.12022", title: "GPQA: A Graduate-Level Google-Proof Q&A Benchmark (Rein et al., 2023)", trust: 0.95 }
related: [benchmark/mmlu-pro]
---

## What It Tests
Deep, expert-level reasoning in physics, chemistry, and biology — the kind of question a domain PhD
can answer but a smart generalist with a search engine cannot. It probes whether a model has
internalized graduate-level scientific understanding, not just facts.

## How It Scores
Accuracy on multiple-choice items. Skilled non-experts (with web access) score ~34%; domain experts
score ~65–74%. Random baseline is 25%. Because GPQA Diamond has only 198 questions, each question is
worth ~0.5%, so single-run scores are noisy — report averaged runs.

## Format
Reported 0-shot with chain-of-thought for chat and reasoning models; the model reasons then commits
to a letter. Four answer options.

## Limitations
The dataset is small, which makes the confidence interval large and makes contamination cheap once
items circulate. Expert disagreement introduces label noise. As with any headline frontier metric,
labs now optimize for it directly, so a sudden jump warrants scrutiny.

## Example
"In quantum chromodynamics, why does the proton weigh ~938 MeV/c² when its three valence quarks have
a combined bare mass of only ~9 MeV/c²?" The correct answer attributes ~99% of the mass to QCD
binding energy (gluon field and virtual quark-antiquark pairs), confirmed by lattice-QCD
first-principles calculations — not to the bare quark masses.
