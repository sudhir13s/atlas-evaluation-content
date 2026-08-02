---
id: benchmark/gpqa-diamond
type: benchmark
slug: gpqa-diamond
schema_version: 1.0
title: "GPQA Diamond"
name: GPQA Diamond
category: Knowledge & Academic Reasoning
description: "The hardest 198-question subset of GPQA — expert-written, Google-proof science questions where both experts agreed and most non-experts failed."
what_is_it: "A curated 198-question slice of GPQA (Graduate-Level Google-Proof Q&A). It keeps only items where both independent domain experts answered correctly and the majority of skilled non-experts got it wrong even with web access — the highest-confidence, hardest questions in the set."
why_people_use_it: "It is the go-to frontier-reasoning benchmark on modern model cards and leaderboards. Being 'Google-proof' and expert-validated, it measures deep PhD-level reasoning that search and shallow recall cannot shortcut."
examples:
  - "A graduate physics item on the quantum-mechanical selection rules for an electric-dipole transition between two specified atomic states, with four expert-written options where the distractors correspond to plausible but incorrect rule applications."
method: "Zero-shot chain-of-thought over 198 four-way multiple-choice questions in biology, physics, and chemistry; the final answer letter is parsed and matched to the expert-verified key."
metric: "Accuracy (% correct); random baseline is 25%."
format: "4-way multiple-choice; zero-shot (or few-shot) chain-of-thought."
saturation: medium
limitations: "Only 198 questions, so a handful of items swings the score and confidence intervals are wide. It covers just biology, physics, and chemistry, is multiple-choice (recognition, not derivation), and frontier reasoning models are climbing past ~85%, eroding its headroom."
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
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2311.12022", title: "GPQA: A Graduate-Level Google-Proof Q&A Benchmark (Rein et al., 2023)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/Idavidrein/gpqa", title: "GPQA dataset (Idavidrein/gpqa, includes gpqa_diamond)", trust: 0.9 }
  - { url: "https://epoch.ai/benchmarks/gpqa-diamond", title: "GPQA Diamond leaderboard (Epoch AI)", trust: 0.9 }
related: [benchmark/gpqa, benchmark/supergpqa, benchmark/mmlu-pro]
---

## What It Tests
Deep, graduate-level science reasoning in **biology, physics, and chemistry** that cannot be looked
up. GPQA's headline property is being "Google-proof": skilled non-expert validators, given over 30
minutes and open web access, still reach only ~34% on the full set.

Diamond is the hardest slice. From GPQA's 448 expert-written questions, it keeps the 198 where **both
expert annotators answered correctly and the majority of non-experts answered wrong** — maximizing
both difficulty and label confidence.

## How It Scores
Accuracy over 198 four-way multiple-choice questions, with a 25% random baseline. On the full GPQA,
domain experts score ~65% and skilled non-experts ~34%; Diamond selects the items experts nailed and
non-experts missed, so it is a clean expert-versus-model gap.

Because the set is small, scores carry meaningful variance — a few flipped items move the number by a
percent or more, so single-run comparisons should be read with that noise in mind.

## Format
Standard 4-way multiple-choice, evaluated zero-shot (sometimes few-shot) with chain-of-thought: the
model reasons through the problem, then emits a final letter that is regex-parsed. It is a stable,
widely reported protocol, which is part of why Diamond became the default frontier science benchmark.

## Limitations
Its 198-question size makes it statistically noisy and easy to overfit to. Coverage is only three
science domains and the format is recognition, not open derivation. Frontier reasoning models are now
pushing past ~85%, so its discriminating headroom is shrinking — hence the move to larger successors
like SuperGPQA.

## Example
A physics item might ask which selection rule permits an electric-dipole transition between two named
atomic states, offering four options. The distractors encode believable errors — applying a
magnetic-dipole rule, or ignoring the change-in-parity requirement — so a model that half-remembers
"selection rules" without reasoning through the specific quantum numbers lands on a wrong but tempting
choice. Web search does not help, which is the whole design.
