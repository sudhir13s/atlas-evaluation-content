---
id: benchmark/critpt
type: benchmark
slug: critpt
schema_version: 3.1
title: "CritPt"
name: CritPt (Complex Research using Integrated Thinking - Physics Test)
category: Science & Research
description: "A physics-research benchmark of 71 unpublished composite challenges, written by over 50 active researchers, with machine-verifiable answers."
what_is_it: "CritPt, pronounced 'critical point', tests language models on unpublished, research-level physics reasoning (Zhu, Tian, Yang and 62 co-authors, 2025). It has 71 composite research challenges designed to simulate entry-level research projects, plus 190 simpler checkpoint tasks derived from them for finer-grained analysis. All problems were newly written by more than 50 active physics researchers. Areas include condensed matter, quantum, atomic and optical, astrophysics, high-energy, statistical and nuclear physics, fluid dynamics and biophysics."
why_people_use_it: "It asks whether models can reason through open-ended challenges of the kind found in frontier physics research, not high-school competitions. Each problem is hand-curated to admit a guess-resistant, machine-verifiable answer. Artificial Analysis includes a hard subset of the second version in its Intelligence Index."
examples:
  - "From the maker's public example challenge (critpt.com): for the [[4,2,2]] quantum error-detecting code, compute the logical state fidelity of a prepared logical |00> state as a closed-form rational expression in the two-qubit gate error rate p, with three graded checkpoints along the way."
method: "An automated grading pipeline customised for advanced physics output formats checks the answers, which can be numbers, symbolic expressions or Python functions. The official repository sends responses to a grading server run with Artificial Analysis, which accepts only complete batches covering all 70 test challenges and at most 10 submissions per account per 24 hours."
metric: "Accuracy (pass@1) = challenges answered correctly / challenges, averaged over 5 runs; checkpoint tasks give a finer view."
format: "Physics research problems answered as Python functions, symbolic expressions or numerical answers. The public test set has 70 challenges, and Artificial Analysis' index uses a fixed private hard subset of 40 of them."
limitations: "The answers are not public, so scoring depends on the maker's grading server. The paper counts 71 challenges while the repository's test set has 70 and a separate example challenge is published, so cite which set a score uses. The hard subset used in the Intelligence Index removed the 30 problems that seven frontier models solved in all five attempts at CritPt v2's launch, so it is tied to the models of that moment. However deep, a set of 70 problems is small."
citation: "https://arxiv.org/abs/2509.26574"
openness: partly-open
openness_note: "The 70 test challenges and an example challenge with its solution are public in the official repository, Hugging Face and critpt.com, but the answers are withheld and scoring runs only through the maker's grading server."
tags: [science, physics, research-level, machine-verifiable]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2509.26574", title: "Probing the Critical Point (CritPt) of AI Reasoning: a Frontier Physics Research Benchmark (arXiv:2509.26574)", trust: 0.95 }
  - { url: "https://github.com/CritPt-Benchmark/CritPt", title: "CritPt-Benchmark/CritPt — official repository (test challenges, grading server rules)", trust: 0.9 }
  - { url: "https://critpt.com/example.html", title: "CritPt example challenge: Quantum Error Detection (official site)", trust: 0.9 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (CritPt v2)", trust: 0.85 }
related: [leaderboard/artificial-analysis]
---

## What It Tests
Whether a model can carry out **entry-level physics research** problems that nobody has published.

- **Composite challenges** — 71 in the paper, each a small research project.
- **Checkpoint tasks** — 190 simpler pieces derived from them for finer diagnosis.
- **Authors** — more than 50 active physics researchers wrote new problems.
- **Breadth** — condensed matter, quantum, astrophysics, high-energy, fluids, biophysics and more.

## How It Scores
An **automated, physics-aware grader** checks each answer.

- **Answer types** — numerical values, symbolic expressions, Python functions.
- **Guess-resistant** — each problem was curated so lucky guesses fail.
- **Headline** — accuracy (pass@1): challenges solved / challenges, averaged over 5 runs.
- **Grading server** — run with Artificial Analysis; accepts only complete 70-challenge batches, 10 submissions per account per day.

## Format
Open-ended problems answered as code, expressions or numbers.

- **Public test set** — 70 challenges in the official repository and on Hugging Face; answers are withheld.
- **Artificial Analysis** — uses CritPt v2 and, in its Intelligence Index, a private hard subset of 40 of the 70.

## Limitations
- **Server-dependent.** Answers are private, so only the maker's server can score a run.
- **Counts differ.** 71 challenges in the paper, 70 in the public test set; cite which.
- **Moment-bound subset.** The hard subset depends on which models solved what at launch.
- **Small.** Dozens of problems, however deep.

## Example
From the maker's public example challenge on critpt.com, closely paraphrased:

- **Setting** — the [[4,2,2]] code, which stores two logical qubits in four physical qubits and can detect but not correct errors.
- **Main task** — compute the **logical state fidelity** of a prepared logical |00⟩ state as a function of the two-qubit gate error rate p, post-selecting on no detected error; noise is depolarizing error after each controlled-NOT (CNOT) gate.
- **Checkpoints** — (1) physical fidelity of a circuit preparing two Greenberger–Horne–Zeilinger (GHZ) entangled pairs, (2) logical fidelity of the GHZ preparation, (3) the main question.
- **Answer type** — a closed-form rational expression in p, checked automatically.
