---
id: benchmark/mmlu
type: benchmark
slug: mmlu
schema_version: 1.0
title: "MMLU"
name: MMLU
category: Knowledge & Academic Reasoning
description: "Massive Multitask Language Understanding — 57 subjects of expert-level multiple-choice questions."
what_is_it: "A 4-choice knowledge exam spanning 57 subjects, from elementary math to professional law, medicine, and moral reasoning. It tests breadth of world and academic knowledge rather than a single skill."
why_people_use_it: "It became the default single-number proxy for general knowledge and is reported by nearly every model release, which makes it useful for rough cross-model comparison — as long as you remember the contamination caveat."
examples:
  - "Which of the following is an example of operant conditioning? (A) a dog salivating at a bell (B) a rat pressing a lever more often after food rewards (C) a child imitating a parent (D) blinking at a puff of air"
method: "Few-shot (typically 5-shot) multiple-choice; the answer is the option with the highest likelihood, or parsed from a generated letter. 15,908 questions across 57 tasks."
metric: "Accuracy (% correct), macro-averaged across the 57 subjects."
format: "5-shot multiple-choice (also reported 0-shot CoT for chat models)."
saturation: high
limitations: "Well-documented training-data contamination — many models have seen MMLU items during pre-training, inflating scores. A handful of questions have wrong gold answers. Frontier models cluster at 86–90%, so the top of the scale no longer discriminates; use MMLU-Pro or GPQA instead."
citation: "https://arxiv.org/abs/2009.03300"
worked_examples:
  - task_type: "Introductory psychology"
    input: "Which of the following is an example of operant conditioning?"
    options:
      - "a dog salivating at the sound of a bell"
      - "a rat pressing a lever more often after it is rewarded with food"
      - "a child imitating a parent's gesture"
      - "a person blinking at a puff of air"
    success:
      answer: "B — a rat pressing a lever more often after it is rewarded with food"
      why: "Operant conditioning changes the frequency of a behavior through its consequences (reinforcement)."
    failure:
      answer: "A — a dog salivating at the sound of a bell"
      why: "That is classical (Pavlovian) conditioning — an involuntary response to an associated stimulus — the most common confusion with operant conditioning."
    checks: "Discriminating operant from classical conditioning."
    scoring: "Exact-match accuracy on the selected option, macro-averaged across 57 subjects."
  - task_type: "College physics"
    input: "A 2 kg object is lifted vertically 3 m at constant velocity near Earth's surface (g = 10 m/s²). How much work does gravity do on the object?"
    options:
      - "+60 J"
      - "−60 J"
      - "0 J"
      - "+20 J"
    success:
      answer: "B — −60 J"
      why: "Gravity points down while the displacement is up, so the work is negative: W = −mgh = −(2)(10)(3) = −60 J."
    failure:
      answer: "A — +60 J"
      why: "Right magnitude, wrong sign — it ignores that gravity opposes the upward displacement. A sign error is the classic mistake on work-by-a-force items."
    checks: "Applying W = F·d with the correct sign convention for a force opposing displacement."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Professional law"
    input: "In contract law, which of the following is valid consideration?"
    options:
      - "a promise to perform a pre-existing legal duty"
      - "a bargained-for exchange of value between the parties"
      - "a gift promised with nothing given in return"
      - "past consideration for an act already completed"
    success:
      answer: "B — a bargained-for exchange of value between the parties"
      why: "Consideration requires something of value bargained for by both sides; the mutual exchange is what makes the promise enforceable."
    failure:
      answer: "A — a promise to perform a pre-existing legal duty"
      why: "The pre-existing duty rule says doing what you are already legally bound to do is not fresh consideration — a common trap that looks like a valid promise."
    checks: "Recognizing the bargained-for-exchange requirement vs the pre-existing duty rule."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "High school biology"
    input: "During aerobic cellular respiration, the majority of ATP is produced in which stage?"
    options:
      - "glycolysis"
      - "the citric acid (Krebs) cycle"
      - "oxidative phosphorylation (the electron transport chain)"
      - "fermentation"
    success:
      answer: "C — oxidative phosphorylation (the electron transport chain)"
      why: "The electron transport chain uses the NADH/FADH₂ from earlier stages to drive chemiosmotic ATP synthesis, yielding the large majority of the cell's ATP."
    failure:
      answer: "B — the citric acid (Krebs) cycle"
      why: "The Krebs cycle produces only a small amount of ATP directly (GTP); it mainly supplies the electron carriers that oxidative phosphorylation then uses — a frequent mix-up."
    checks: "Locating where the bulk of ATP is generated in aerobic respiration."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "World history"
    input: "The Peace of Westphalia (1648) is most associated with establishing which principle of international relations?"
    options:
      - "the divine right of kings"
      - "state sovereignty and non-interference in domestic affairs"
      - "universal free trade between nations"
      - "the abolition of the slave trade"
    success:
      answer: "B — state sovereignty and non-interference in domestic affairs"
      why: "Westphalia ended the Thirty Years' War and is conventionally credited with the modern system of sovereign states that do not interfere in one another's internal affairs."
    failure:
      answer: "A — the divine right of kings"
      why: "A real early-modern idea, but a different one; Westphalia is about relations between states, not the source of a monarch's authority."
    checks: "Associating a landmark treaty with the correct political principle."
    scoring: "Exact-match accuracy on the selected option."
tags: [knowledge, multiple-choice, reasoning, contamination]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2009.03300", title: "Measuring Massive Multitask Language Understanding (Hendrycks et al., 2021)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2310.18018", title: "Data Contamination Through the Lens of Time (Sainz et al. discussion of MMLU contamination)", trust: 0.75 }
related: [benchmark/mmlu-pro, benchmark/gpqa]
---

## What It Tests
Breadth of academic and professional knowledge plus light reasoning, across 57 subjects grouped
into STEM, humanities, social sciences, and "other" (business, health, misc). It is a knowledge
exam, not a reasoning or generation test.

## How It Scores
Plain accuracy: the fraction of the 15,908 four-choice questions answered correctly, macro-averaged
so each subject counts equally. Random guessing scores 25%. Human expert (domain-specialist)
performance is ~89.8%; unspecialized humans score far lower on out-of-field subjects.

## Format
Standardly 5-shot: five worked examples are prepended, then the model answers the sixth. Base models
are scored by comparing the likelihood of each answer letter; chat models are often run 0-shot with
chain-of-thought and the letter is parsed from the response. The two protocols are not directly
comparable — always check which was used.

## Limitations
Contamination is the headline problem: MMLU predates most models' training cutoffs, so items leak
into pre-training corpora and scores overstate true generalization. The benchmark is also near
**saturated** — frontier models sit at 86–90%, within noise of each other and of the human ceiling —
and a small number of questions have disputed or incorrect keys. Treat any score above ~90% with
suspicion.

## Example
"Which of the following is an example of operant conditioning?" with options (A) a dog salivating at
a bell, (B) a rat pressing a lever more often after receiving food, (C) a child imitating a parent,
(D) blinking at a puff of air. The correct answer is **(B)** — behavior changed by its consequences.
(A) is classical conditioning, (C) is observational learning, (D) is a reflex.
