---
id: benchmark/mmlu
type: benchmark
slug: mmlu
schema_version: 1.0
title: "MMLU"
name: MMLU
category: Knowledge & Academic Reasoning
description: "Massive Multitask Language Understanding: 15,908 multiple-choice questions across 57 subjects, from elementary mathematics to law."
what_is_it: "MMLU was introduced by Hendrycks et al. (2020) to measure a text model's multitask accuracy. It covers 57 tasks, including elementary mathematics, US history, computer science and law, with 15,908 four-option questions: a few-shot development set of 5 questions per subject, a validation set of 1,540 and a test set of 14,079. The authors estimate expert-level accuracy at about 89.8%, from the 95th-percentile human scores on the source exams."
why_people_use_it: "It evaluates breadth of academic and professional knowledge in zero-shot and few-shot settings, and its per-subject results show where a model is weak."
examples:
  - "Which of the following is an example of operant conditioning? (A) a dog salivating at a bell (B) a rat pressing a lever more often after food rewards (C) a child imitating a parent (D) blinking at a puff of air"
method: "Zero-shot or few-shot (5-shot uses the development set) multiple choice; the answer is the option with the highest likelihood or a letter parsed from the response."
metric: "Accuracy (% correct) over the 57 subjects."
format: "5-shot or 0-shot multiple choice, four options."
limitations: "MMLU-Redux (Gema et al., 2024) estimates that 6.49% of MMLU questions contain errors. Likelihood-based scoring and parsing a generated letter are different protocols and give different numbers. The questions are public, so they can appear in training data."
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
tags: [knowledge, multiple-choice, reasoning]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2009.03300", title: "Measuring Massive Multitask Language Understanding (Hendrycks et al., 2020)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/2009.03300", title: "MMLU paper, full text (ar5iv)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2406.04127", title: "Are We Done with MMLU? (MMLU-Redux, Gema et al., 2024)", trust: 0.9 }
related: [benchmark/mmlu-pro, benchmark/mmlu-redux, benchmark/gpqa]
---

## What It Tests
**Breadth of academic and professional knowledge** with some problem solving, across 57 subjects in the humanities, social sciences, STEM and other areas.

## How It Scores
**Accuracy** on four-option questions; random guessing scores 25%. The authors estimate expert-level accuracy at about 89.8%.

## Format
The 5-shot setup prepends five development questions per subject; zero-shot runs give none. Likelihood scoring and letter parsing are not directly comparable.

## Limitations
- **Label errors.** MMLU-Redux estimates 6.49% of questions contain errors.
- **Protocol.** Scoring method changes the number.
- **Public items.** Questions can leak into training data.

## Example
"Which of the following is an example of operant conditioning?" with options (A) a dog salivating at
a bell, (B) a rat pressing a lever more often after receiving food, (C) a child imitating a parent,
(D) blinking at a puff of air. The correct answer is **(B)** — behavior changed by its consequences.
(A) is classical conditioning, (C) is observational learning, (D) is a reflex.
