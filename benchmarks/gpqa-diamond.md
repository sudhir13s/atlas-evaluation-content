---
id: benchmark/gpqa-diamond
type: benchmark
slug: gpqa-diamond
schema_version: 1.0
title: "GPQA Diamond"
name: GPQA Diamond
category: Knowledge & Academic Reasoning
description: "The 198-question highest-quality subset of GPQA: expert-written biology, physics and chemistry questions that both expert validators answered correctly and most non-experts did not."
what_is_it: "GPQA Diamond is the subset that the GPQA authors (Rein et al., 2023) call their highest-quality one. It includes only questions where both expert validators answer correctly (or the second makes a demonstrable mistake) and the majority of non-expert validators answer incorrectly. GPQA's experts have or are pursuing PhDs in the field."
why_people_use_it: "GPQA was built for scalable-oversight experiments: its questions are hard for skilled non-experts even with unrestricted web access ('Google-proof'). The diamond set keeps the questions with the strongest expert agreement."
examples:
  - "A graduate physics item on the quantum-mechanical selection rules for an electric-dipole transition between two specified atomic states, with four expert-written options where the distractors correspond to plausible but incorrect rule applications."
method: "Four-option multiple choice. The paper's baselines use zero-shot, few-shot, zero-shot chain-of-thought and few-shot chain-of-thought prompting; the chosen letter is compared with the key."
metric: "Accuracy (% correct); random choice scores 25%."
format: "Four-option multiple choice in biology, physics and chemistry, 198 questions."
limitations: "198 questions, so each question moves the score by about half a percentage point. Three science domains only, in multiple-choice form. The paper reports expert validator accuracy of 81.3% on the diamond set but notes that validator accuracies on it are skewed by the selection rule. The dataset carries a canary string so training corpora can filter it out."
citation: "https://arxiv.org/abs/2311.12022"
worked_examples:
  - task_type: "Graduate physics — atomic electric-dipole selection rules"
    input: "For an electric-dipole (E1) transition in atomic hydrogen, which of the following is allowed?"
    options:
      - "2s → 1s"
      - "2p → 1s"
      - "3d → 1s"
      - "3s → 2s"
    success:
      answer: "B — 2p → 1s"
      why: "E1 transitions require Δl = ±1 with a parity change; 2p → 1s has Δl = −1 and opposite parity, so it is allowed."
    failure:
      answer: "A — 2s → 1s"
      why: "It assumes any large energy drop to the ground state radiates, but Δl = 0 violates the E1 rule — 2s is metastable, not E1-allowed."
    checks: "Applying specific quantum selection rules by reasoning through the quantum numbers, which web search cannot shortcut."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Graduate organic chemistry — reaction stereochemistry"
    input: "A concerted bimolecular nucleophilic substitution (SN2) occurs at a chiral secondary carbon. What is the stereochemical outcome at that carbon?"
    options:
      - "Retention of configuration"
      - "Inversion of configuration (Walden inversion)"
      - "Racemization (a 50:50 mixture of both configurations)"
      - "Loss of chirality by forming an achiral product"
    success:
      answer: "B — Inversion of configuration (Walden inversion)"
      why: "The nucleophile attacks from the side opposite the leaving group in one concerted step, inverting the stereocenter like an umbrella turning inside out."
    failure:
      answer: "C — Racemization (a 50:50 mixture of both configurations)"
      why: "Racemization is the signature of SN1, which goes through a planar carbocation attacked from both faces; the concerted SN2 pathway gives clean inversion, not a racemate."
    checks: "Distinguishing SN2 (concerted, inversion) from SN1 (carbocation, racemization) from mechanism rather than memory."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Graduate molecular biology — translation initiation"
    input: "In eukaryotes, which feature added to the 5′ end of a pre-mRNA is recognized by the eIF4E factor and is required for efficient cap-dependent translation initiation?"
    options:
      - "The poly(A) tail"
      - "The 7-methylguanosine (m7G) cap"
      - "Removal of introns by splicing"
      - "The Shine–Dalgarno sequence"
    success:
      answer: "B — The 7-methylguanosine (m7G) cap"
      why: "eIF4E binds the 5′ m7G cap to recruit the ribosome, so the cap is what drives cap-dependent initiation."
    failure:
      answer: "A — The poly(A) tail"
      why: "The poly(A) tail aids stability, export, and initiation efficiency, but it sits at the 3′ end and is not the 5′ feature eIF4E recognizes; the question specifies the 5′ end."
    checks: "Precise recall of the molecular players in eukaryotic translation initiation, keyed to the correct end of the transcript."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Graduate physics — conservation laws in particle physics"
    input: "In the Standard Model, which conservation law forbids the hypothetical decay of a free proton into a positron and a neutral pion (p → e⁺ + π⁰)?"
    options:
      - "Conservation of electric charge"
      - "Conservation of baryon number"
      - "Conservation of energy"
      - "Conservation of angular momentum"
    success:
      answer: "B — Conservation of baryon number"
      why: "The proton has baryon number 1 while the positron and pion have baryon number 0, so the decay violates baryon-number conservation, which the Standard Model preserves."
    failure:
      answer: "A — Conservation of electric charge"
      why: "The decay actually conserves charge (+1 → +1 + 0), so charge is not what forbids it; the real barrier is baryon number, which this answer overlooks."
    checks: "Identifying which specific conservation law is violated, not just any plausible-sounding one."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Graduate physical chemistry — spontaneity criterion"
    input: "For a process occurring spontaneously at constant temperature and pressure, which thermodynamic quantity of the system must decrease?"
    options:
      - "The enthalpy (H) of the system"
      - "The entropy of the system"
      - "The Gibbs free energy (G) of the system"
      - "The internal energy (U) of the system"
    success:
      answer: "C — The Gibbs free energy (G) of the system"
      why: "At constant T and P, spontaneity is defined by ΔG < 0, so the system's Gibbs free energy must decrease."
    failure:
      answer: "B — The entropy of the system"
      why: "The second law requires the total (universe) entropy to increase; the system's own entropy can fall if the surroundings compensate, so system entropy is the wrong criterion at constant T, P."
    checks: "Applying the correct free-energy spontaneity criterion rather than the total-entropy statement of the second law."
    scoring: "Exact-match accuracy on the selected option."
tags: [knowledge, reasoning, science, multiple-choice, frontier, google-proof]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2311.12022", title: "GPQA: A Graduate-Level Google-Proof Q&A Benchmark (Rein et al., 2023)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2311.12022", title: "GPQA, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/Idavidrein/gpqa", title: "GPQA dataset (Idavidrein/gpqa, includes gpqa_diamond)", trust: 0.9 }
related: [benchmark/gpqa, benchmark/supergpqa, benchmark/mmlu-pro]
---

## What It Tests
**Graduate-level science questions** in biology, physics and chemistry, written by domain experts.

- **Google-proof** — on GPQA (448 questions), skilled non-expert validators reached 34% despite over 30 minutes with unrestricted web access (2023 paper).
- **Highest quality** — Diamond keeps questions where both expert validators agree and at most one in three non-experts answered correctly.

## How It Scores
**Accuracy** on 198 four-option questions; random choice scores 25%. The paper's own sets are extended (546), main (448) and diamond (198).

## Format
Four-option multiple choice. The paper's baselines include zero-shot, few-shot and chain-of-thought prompting; the final letter is compared with the key.

## Limitations
- **Small set.** One question is about 0.5 percentage points.
- **Narrow.** Three science domains, multiple choice only.
- **Selection effect.** Validator accuracies on Diamond are skewed by how it was selected, as the paper notes.

## Example
The dataset carries a canary string so training corpora can filter it out, so no question is reproduced here. Each item is a question with four options written by a PhD-level expert; the model answers with one letter, scored against the key.
