---
id: benchmark/gpqa
type: benchmark
slug: gpqa
schema_version: 4.0
title: GPQA
name: GPQA
category: Knowledge & Academic Reasoning
description: 'A Graduate-Level Google-Proof Q&A benchmark: 448 multiple-choice questions in biology, physics and chemistry written by domain experts.'
what_is_it: GPQA was introduced by David Rein, Samuel R. Bowman and colleagues at New York University, Cohere and Anthropic (2023). 61 contractors hired through Upwork, each with or pursuing a PhD in their field, wrote and validated the questions. Its main set has 448 multiple-choice questions in biology, physics and chemistry; the full set (GPQA Extended) has 546 questions and the highest-quality subset (GPQA Diamond) has 198. In the paper, experts reached 65% accuracy (74% after discounting mistakes they identified in retrospect), while skilled non-experts reached 34% despite spending 37 minutes per question on average with full internet access.
why_people_use_it: The authors made the questions hard enough that searching the web does not answer them ("Google-proof"), to support scalable-oversight experiments in which humans must supervise AI answers they cannot check easily.
examples:
- 'No item is reproduced: the authors ask that GPQA examples not be revealed in plain text or images online. Each item is an expert-written graduate-level biology, physics or chemistry question with four answer options, one correct.'
method: 'Multiple choice: the model chooses one of four options per question. The paper''s closed-book baselines use zero-shot, few-shot, zero-shot chain-of-thought and few-shot chain-of-thought prompting, and an open-book baseline gives the model a web-search tool. Reports name the subset used (main, Extended or Diamond).'
metric: Accuracy = correct answers / total questions in the chosen set; random guessing scores 25%.
format: Multiple choice, four options.
limitations: The sets are small (198 to 546 questions), so scores have wide uncertainty. The authors estimate that 74% of GPQA Extended questions have uncontroversially correct answers. Results on different subsets are not comparable; GPQA Diamond has its own record.
citation: https://arxiv.org/abs/2311.12022
openness: open
openness_note: All question sets are downloadable (CC BY 4.0) from Hugging Face after accepting access conditions and from a password-protected archive in the GitHub repository, with baseline code; the authors ask that items not be posted online in plain text.
worked_examples:
- task_type: Graduate organic chemistry (pericyclic reactions)
  input: A thermal [4+2] Diels–Alder cycloaddition proceeds suprafacial–suprafacial and is symmetry-allowed. Under the Woodward–Hoffmann rules, what makes the thermal reaction allowed?
  options:
  - The diene is electron-rich and the dienophile electron-poor, so frontier orbitals overlap favorably.
  - Six π electrons in a Hückel (suprafacial–suprafacial) topology satisfy the 4n+2 rule, so orbital symmetry is conserved.
  - The transition state is antiaromatic under thermal conditions, which drives the reaction forward.
  - All concerted cycloadditions are thermally allowed regardless of electron count.
  success:
    answer: B — Six π electrons in a Hückel topology satisfy the 4n+2 rule, so orbital symmetry is conserved
    why: 'Woodward–Hoffmann allowedness is fixed by electron count and topology: a 6-electron suprafacial–suprafacial (Hückel) transition state is aromatic and thermally allowed.'
  failure:
    answer: A — the diene is electron-rich and the dienophile electron-poor
    why: Electronic demand governs rate and regiochemistry (endo/ortho preference), not symmetry allowedness — a near-miss that swaps a kinetic effect for the conservation-of-orbital-symmetry rule.
  checks: Distinguishing orbital-symmetry allowedness from electronic-demand rate effects in pericyclic chemistry.
  scoring: Accuracy (% correct) on GPQA Diamond.
- task_type: Graduate molecular biology (NMD)
  input: A nonsense (premature stop) mutation located in the last exon of a gene frequently escapes nonsense-mediated mRNA decay (NMD). What best explains this escape?
  options:
  - The premature stop lies downstream of the final exon–exon junction, so no exon-junction complex remains to mark it for decay.
  - The last exon is the longest, so the ribosome reads through the premature stop.
  - NMD acts only on intron-retaining transcripts, which are absent after the final splice.
  - The poly-A tail physically shields every terminal codon from the decay machinery.
  success:
    answer: A — The premature stop lies downstream of the final exon–exon junction, so no exon-junction complex remains to mark it for decay
    why: NMD is triggered when a stop is >~50 nt upstream of an exon-junction complex; a last-exon stop has no downstream EJC, so the surveillance signal is absent.
  failure:
    answer: B — the last exon is the longest, so the ribosome reads through the premature stop
    why: Read-through is not the mechanism; escape is about EJC positioning, not exon length — a plausible-sounding distractor that misses the exon-junction-complex logic.
  checks: Whether the model grasps the EJC-dependent positional rule for NMD, not a surface heuristic about exon size.
  scoring: Accuracy (% correct) on GPQA Diamond.
- task_type: Graduate quantum mechanics (identical particles)
  input: Two identical spin-½ fermions occupy a one-dimensional infinite square well. What is the ground-state configuration?
  options:
  - Both particles in n=1 with parallel spins (spin triplet).
  - Both particles in n=1 in a spin singlet, giving a symmetric spatial wavefunction.
  - One particle in n=1 and one in n=2, spin triplet, antisymmetric spatial part.
  - Both particles in n=2 with opposite spins.
  success:
    answer: B — Both particles in n=1 in a spin singlet, giving a symmetric spatial wavefunction
    why: The antisymmetric singlet spin state pairs with a symmetric spatial part, so both fermions can share the n=1 orbital while keeping the total wavefunction antisymmetric — the lowest energy.
  failure:
    answer: C — one particle in n=1 and one in n=2, spin triplet, antisymmetric spatial part
    why: This is the correct antisymmetrization for the triplet, but promoting a particle to n=2 costs energy; it is the lowest triplet state, not the ground state — a near-miss that forgets the singlet lets both stay in n=1.
  checks: Correctly combining spin symmetry with the Pauli requirement to find the true ground state.
  scoring: Accuracy (% correct) on GPQA Diamond.
- task_type: Graduate spectroscopy (NMR topicity)
  input: The two methyl groups of an isopropyl group attached directly to a stereocenter appear as two distinct doublets in the ¹H NMR spectrum. Why are they inequivalent?
  options:
  - 'They are diastereotopic: the adjacent stereocenter makes them chemically inequivalent in any environment.'
  - They are enantiotopic and are therefore inequivalent in every solvent.
  - Restricted rotation about the C–C bond freezes them into fixed, different environments.
  - They are homotopic but J-coupled to each other, which splits each into a doublet.
  success:
    answer: 'A — They are diastereotopic: the adjacent stereocenter makes them chemically inequivalent in any environment'
    why: Swapping the two methyls gives diastereomers, so they are diastereotopic and resonate at different shifts regardless of solvent chirality.
  failure:
    answer: B — they are enantiotopic and therefore inequivalent in every solvent
    why: Enantiotopic groups are equivalent in an achiral environment; the correct term is diastereotopic — a subtle topicity distinction that trips non-experts.
  checks: Applying the homotopic/enantiotopic/diastereotopic hierarchy to predict NMR inequivalence.
  scoring: Accuracy (% correct) on GPQA Diamond.
tags:
- science
- expert-questions
- multiple-choice
- reasoning
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2311.12022
  title: 'GPQA: A Graduate-Level Google-Proof Q&A Benchmark (Rein et al., 2023)'
  trust: 0.95
- url: https://arxiv.org/html/2311.12022
  title: GPQA paper, full text (arXiv HTML)
  trust: 0.95
- url: https://github.com/idavidrein/gpqa
  title: idavidrein/gpqa — official repository (data archive, baseline code)
  trust: 0.9
- url: https://huggingface.co/datasets/Idavidrein/gpqa
  title: Idavidrein/gpqa — dataset card (CC BY 4.0, access conditions)
  trust: 0.9
related:
- benchmark/gpqa-diamond
- benchmark/mmlu-pro
---

## What It Tests

**Graduate-level science** in biology, physics and chemistry that a skilled non-expert cannot answer with a web search ("Google-proof").

- **Makers** — Rein, Bowman and colleagues (New York University, Cohere, Anthropic), 2023.
- **Writers** — 61 Upwork contractors with or pursuing a PhD in their field wrote and cross-validated the questions.

## How It Scores

**Accuracy** = correct answers / total questions in the chosen set; random guessing scores 25%. Paper baselines:

- **Experts** — 65% (74% discounting clear mistakes).
- **Skilled non-experts with web access** — 34%, after 37 minutes per question on average.

## Format

Four-option multiple choice. Three sets: Extended (546), main (448, the authors' recommended primary set) and Diamond (198).

- **Paper's prompts** — zero-shot, few-shot, and chain-of-thought (the model reasons step by step first); plus an open-book run with web search.
- **Data** — CC BY 4.0, on Hugging Face behind an access agreement and in a password-protected archive in the GitHub repository; the files carry a canary string so training pipelines can filter them out.

## Limitations

- **Small.** Wide confidence intervals.
- **Label quality.** The authors estimate 74% of Extended questions are uncontroversially correct.
- **Subset matters.** Always state which set a score is on.

## Example

**No GPQA item is shown here.** The authors request that people "do not reveal examples from this dataset in plain text or images online", to keep them out of model training data.

As the paper describes the task, each item is a question written by a PhD-level expert in biology, physics or chemistry, with **four answer options**, one correct. The model answers with a letter, which is compared with the key. The illustrative examples on this page are site-written, not GPQA items.
