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
worked_examples:
  - task_type: "Graduate physics (QCD)"
    input: "In quantum chromodynamics the proton weighs ~938 MeV/c², yet its three valence quarks have a combined bare mass of only ~9 MeV/c². What accounts for ~99% of the proton's mass?"
    options:
      - "the Higgs mechanism"
      - "QCD binding energy — the gluon field and virtual quark–antiquark pairs"
      - "electromagnetic self-energy of the charged quarks"
      - "the relativistic kinetic mass of the valence quarks"
    success:
      answer: "B — QCD binding energy — the gluon field and virtual quark–antiquark pairs"
      why: "The dynamical energy of the confined gluon field and sea quarks, confirmed by first-principles lattice-QCD, supplies almost all nucleon mass; the bare quark masses contribute only ~1%."
    failure:
      answer: "A — the Higgs mechanism"
      why: "A sophisticated near-miss: the Higgs mechanism sets the quarks' small bare masses but not the emergent mass of the bound state — it conflates the origin of quark mass with the origin of hadron mass."
    checks: "Separating emergent, dynamical QCD mass from a pattern-match of 'mass' to 'Higgs' — a distinction only expert physics reasoning makes reliably."
    scoring: "Accuracy (% correct) on GPQA Diamond."
  - task_type: "Graduate organic chemistry (pericyclic reactions)"
    input: "A thermal [4+2] Diels–Alder cycloaddition proceeds suprafacial–suprafacial and is symmetry-allowed. Under the Woodward–Hoffmann rules, what makes the thermal reaction allowed?"
    options:
      - "The diene is electron-rich and the dienophile electron-poor, so frontier orbitals overlap favorably."
      - "Six π electrons in a Hückel (suprafacial–suprafacial) topology satisfy the 4n+2 rule, so orbital symmetry is conserved."
      - "The transition state is antiaromatic under thermal conditions, which drives the reaction forward."
      - "All concerted cycloadditions are thermally allowed regardless of electron count."
    success:
      answer: "B — Six π electrons in a Hückel topology satisfy the 4n+2 rule, so orbital symmetry is conserved"
      why: "Woodward–Hoffmann allowedness is fixed by electron count and topology: a 6-electron suprafacial–suprafacial (Hückel) transition state is aromatic and thermally allowed."
    failure:
      answer: "A — the diene is electron-rich and the dienophile electron-poor"
      why: "Electronic demand governs rate and regiochemistry (endo/ortho preference), not symmetry allowedness — a near-miss that swaps a kinetic effect for the conservation-of-orbital-symmetry rule."
    checks: "Distinguishing orbital-symmetry allowedness from electronic-demand rate effects in pericyclic chemistry."
    scoring: "Accuracy (% correct) on GPQA Diamond."
  - task_type: "Graduate molecular biology (NMD)"
    input: "A nonsense (premature stop) mutation located in the last exon of a gene frequently escapes nonsense-mediated mRNA decay (NMD). What best explains this escape?"
    options:
      - "The premature stop lies downstream of the final exon–exon junction, so no exon-junction complex remains to mark it for decay."
      - "The last exon is the longest, so the ribosome reads through the premature stop."
      - "NMD acts only on intron-retaining transcripts, which are absent after the final splice."
      - "The poly-A tail physically shields every terminal codon from the decay machinery."
    success:
      answer: "A — The premature stop lies downstream of the final exon–exon junction, so no exon-junction complex remains to mark it for decay"
      why: "NMD is triggered when a stop is >~50 nt upstream of an exon-junction complex; a last-exon stop has no downstream EJC, so the surveillance signal is absent."
    failure:
      answer: "B — the last exon is the longest, so the ribosome reads through the premature stop"
      why: "Read-through is not the mechanism; escape is about EJC positioning, not exon length — a plausible-sounding distractor that misses the exon-junction-complex logic."
    checks: "Whether the model grasps the EJC-dependent positional rule for NMD, not a surface heuristic about exon size."
    scoring: "Accuracy (% correct) on GPQA Diamond."
  - task_type: "Graduate quantum mechanics (identical particles)"
    input: "Two identical spin-½ fermions occupy a one-dimensional infinite square well. What is the ground-state configuration?"
    options:
      - "Both particles in n=1 with parallel spins (spin triplet)."
      - "Both particles in n=1 in a spin singlet, giving a symmetric spatial wavefunction."
      - "One particle in n=1 and one in n=2, spin triplet, antisymmetric spatial part."
      - "Both particles in n=2 with opposite spins."
    success:
      answer: "B — Both particles in n=1 in a spin singlet, giving a symmetric spatial wavefunction"
      why: "The antisymmetric singlet spin state pairs with a symmetric spatial part, so both fermions can share the n=1 orbital while keeping the total wavefunction antisymmetric — the lowest energy."
    failure:
      answer: "C — one particle in n=1 and one in n=2, spin triplet, antisymmetric spatial part"
      why: "This is the correct antisymmetrization for the triplet, but promoting a particle to n=2 costs energy; it is the lowest triplet state, not the ground state — a near-miss that forgets the singlet lets both stay in n=1."
    checks: "Correctly combining spin symmetry with the Pauli requirement to find the true ground state."
    scoring: "Accuracy (% correct) on GPQA Diamond."
  - task_type: "Graduate spectroscopy (NMR topicity)"
    input: "The two methyl groups of an isopropyl group attached directly to a stereocenter appear as two distinct doublets in the ¹H NMR spectrum. Why are they inequivalent?"
    options:
      - "They are diastereotopic: the adjacent stereocenter makes them chemically inequivalent in any environment."
      - "They are enantiotopic and are therefore inequivalent in every solvent."
      - "Restricted rotation about the C–C bond freezes them into fixed, different environments."
      - "They are homotopic but J-coupled to each other, which splits each into a doublet."
    success:
      answer: "A — They are diastereotopic: the adjacent stereocenter makes them chemically inequivalent in any environment"
      why: "Swapping the two methyls gives diastereomers, so they are diastereotopic and resonate at different shifts regardless of solvent chirality."
    failure:
      answer: "B — they are enantiotopic and therefore inequivalent in every solvent"
      why: "Enantiotopic groups are equivalent in an achiral environment; the correct term is diastereotopic — a subtle topicity distinction that trips non-experts."
    checks: "Applying the homotopic/enantiotopic/diastereotopic hierarchy to predict NMR inequivalence."
    scoring: "Accuracy (% correct) on GPQA Diamond."
tags: [frontier, science, multiple-choice, reasoning]
updated: 2026-08-02
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
