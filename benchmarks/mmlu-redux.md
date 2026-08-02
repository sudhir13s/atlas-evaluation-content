---
id: benchmark/mmlu-redux
type: benchmark
slug: mmlu-redux
schema_version: 1.0
title: "MMLU-Redux"
name: MMLU-Redux
category: Knowledge & Academic Reasoning
description: "A hand-re-annotated, error-corrected subset of MMLU used to measure knowledge without the noise of the original's wrong answer keys."
what_is_it: "A 5,700-question subset of MMLU that 14 experts manually re-checked against a formal error taxonomy, correcting or flagging items whose ground-truth answer was wrong, missing, or ambiguous — so scores reflect knowledge rather than label noise."
why_people_use_it: "It quantified that MMLU itself is buggy (~6.49% of questions have errors) and gives a cleaner surface for comparing near-saturated models, where a few percent of bad labels can flip rankings."
examples:
  - "A re-audit of an original MMLU virology item flagged as 'Wrong Ground Truth' — the answer key names an option that the domain expert re-annotator marks as incorrect, so the item is corrected or excluded rather than scored against the faulty key."
method: "Re-annotate 5,700 MMLU questions (100 per subject × 57 subjects) under a two-level error taxonomy, then re-score models on the corrected set and compare against original MMLU numbers."
metric: "Accuracy (% correct) on the corrected items."
format: "Same 4-way multiple-choice as MMLU; typically 0-shot or 5-shot."
saturation: high
limitations: "It corrects labels but keeps MMLU's 4-option multiple-choice format, so guessing headroom and the knowledge-only scope remain. Only 100 questions per subject are re-annotated, so per-subject estimates are noisy, and it inherits MMLU's contamination exposure."
citation: "https://arxiv.org/abs/2406.04127"
worked_examples:
  - task_type: "Undergraduate biology (re-audited, verified key)"
    input: "Which enzyme unwinds the DNA double helix at the replication fork?"
    options:
      - "DNA polymerase"
      - "Helicase"
      - "DNA ligase"
      - "Primase"
    success:
      answer: "B — Helicase"
      why: "Helicase breaks the hydrogen bonds between base pairs, separating the two strands at the fork."
    failure:
      answer: "A — DNA polymerase"
      why: "Polymerase is central to replication so it is a tempting pick, but it synthesizes the new strand — it does not unwind the helix."
    checks: "Precise recall of enzyme roles, scored against a hand-verified key so the result reflects knowledge, not a faulty label."
    scoring: "Exact-match accuracy on the selected option (over corrected labels)."
  - task_type: "High-school chemistry (re-audited, verified key)"
    input: "What is the pH of a neutral aqueous solution at 25 °C?"
    options:
      - "0"
      - "7"
      - "14"
      - "1"
    success:
      answer: "B — 7"
      why: "At 25 °C a neutral solution has [H⁺] = [OH⁻] = 10⁻⁷ M, giving pH = 7."
    failure:
      answer: "C — 14"
      why: "14 is the pH of a strongly basic solution and the top of the ordinary scale — confusing the scale's endpoint with its neutral midpoint."
    checks: "Knowing the neutral point of the pH scale and not conflating it with the basic extreme."
    scoring: "Exact-match accuracy on the selected option (over corrected labels)."
  - task_type: "US history (re-audited, verified key)"
    input: "Which document, drafted in 1787, established the framework of the United States federal government?"
    options:
      - "The Declaration of Independence"
      - "The U.S. Constitution"
      - "The Articles of Confederation"
      - "The Bill of Rights"
    success:
      answer: "B — The U.S. Constitution"
      why: "Drafted at the 1787 Philadelphia Convention, it set up the three-branch federal structure of government."
    failure:
      answer: "C — The Articles of Confederation"
      why: "The Articles (1781) were the earlier, weaker framework the Constitution replaced — a plausible but chronologically-off pick."
    checks: "Matching a founding document to its date and role, and ordering it against its predecessor."
    scoring: "Exact-match accuracy on the selected option (over corrected labels)."
  - task_type: "Statistics (re-audited, verified key)"
    input: "For a standard normal distribution, approximately what percentage of values fall within one standard deviation of the mean?"
    options:
      - "50%"
      - "68%"
      - "95%"
      - "99.7%"
    success:
      answer: "B — 68%"
      why: "The empirical (68–95–99.7) rule places about 68% of a normal distribution within ±1 standard deviation of the mean."
    failure:
      answer: "C — 95%"
      why: "95% is the fraction within ±2 standard deviations, not ±1 — a swap of the empirical-rule tiers."
    checks: "Recalling the 68–95–99.7 rule and mapping each percentage to the correct number of standard deviations."
    scoring: "Exact-match accuracy on the selected option (over corrected labels)."
  - task_type: "College medicine — virology (a subject the re-annotation flagged for key errors)"
    input: "What type of nucleic acid makes up the genome packaged in a human immunodeficiency virus (HIV) particle?"
    options:
      - "Double-stranded DNA"
      - "Single-stranded RNA"
      - "Double-stranded RNA"
      - "Single-stranded DNA"
    success:
      answer: "B — Single-stranded RNA"
      why: "HIV is a retrovirus: its virion carries a positive-sense single-stranded RNA genome that reverse transcriptase later copies into DNA."
    failure:
      answer: "A — Double-stranded DNA"
      why: "HIV does form a dsDNA provirus that integrates into the host genome, but that is the intermediate, not the packaged genome — conflating the provirus with the virion genome, exactly the kind of subtlety a verified key must get right."
    checks: "Distinguishing a retrovirus's packaged genome from its DNA intermediate, against a hand-verified label."
    scoring: "Exact-match accuracy on the selected option (over corrected labels)."
tags: [knowledge, multiple-choice, data-quality, mmlu]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2406.04127", title: "Are We Done with MMLU? (Gema et al., 2024)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/edinburgh-dawg/mmlu-redux", title: "MMLU-Redux dataset (Edinburgh DAWG)", trust: 0.9 }
  - { url: "https://github.com/aryopg/mmlu-redux", title: "MMLU-Redux GitHub repository", trust: 0.85 }
related: [benchmark/mmlu, benchmark/mmlu-pro, benchmark/gpqa]
---

## What It Tests
The same broad academic knowledge as MMLU across 57 subjects — but the point is measurement
quality, not new content. MMLU-Redux asks whether a model's MMLU score is even trustworthy once
the wrong answer keys are removed.

The authors found roughly **6.49% of MMLU questions contain errors**, with some subjects far worse
(over half of the sampled virology items were flawed). On near-saturated leaderboards that error
floor is enough to reorder models.

## How It Scores
Accuracy over the 5,700 re-annotated questions (100 per subject × 57 subjects). Each item was
audited under a two-level error taxonomy:

- **Question Assessment** — bad question clarity, or bad/ambiguous answer options.
- **Ground-Truth Verification** — no correct answer, multiple correct answers, or a wrong ground
  truth in the original key.

Items with correct labels are scored normally; flawed items are corrected or excluded, so the
resulting accuracy is a cleaner signal than raw MMLU.

## Format
Unchanged from MMLU: 4-way multiple-choice, run 0-shot or 5-shot depending on the harness. The
contribution is the corrected labels and the annotation protocol, not a new prompt format.

## Limitations
It fixes the answer keys but not the design: four options keep the random baseline at 25%, and the
scope stays knowledge-only (no generation, tools, or long context). Only 100 questions per subject
are re-checked, so per-subject error estimates carry real sampling noise, and the underlying items
remain as exposed to training-data contamination as MMLU.

## Example
Take an original MMLU item whose answer key marks option C as correct. A domain-expert re-annotator
reviewing it under the taxonomy determines that C is actually wrong and B is the defensible answer —
tagging it **Wrong Ground Truth**. In plain MMLU, a model that answered B is penalized; in
MMLU-Redux the key is corrected (or the item dropped), so the score reflects knowledge instead of
the benchmark's own mistake.
