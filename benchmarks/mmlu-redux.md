---
id: benchmark/mmlu-redux
type: benchmark
slug: mmlu-redux
schema_version: 1.0
title: "MMLU-Redux"
name: MMLU-Redux
category: Knowledge & Academic Reasoning
description: "A 5,700-question subset of MMLU, manually re-annotated so that each question carries a label saying whether, and how, it is erroneous."
what_is_it: "MMLU-Redux (Gema et al., 2024, University of Edinburgh and collaborators) is 100 randomly selected questions from each of MMLU's 57 subjects. 14 human experts re-annotated the 5,700 questions under a hierarchical error protocol: is the question clear, is there a single valid answer, and does it match the dataset's ground truth."
why_people_use_it: "From it the authors estimate that 6.49% of MMLU questions contain errors, with 57% of the analysed Virology questions erroneous, and they show that model scores differ from those originally reported once the errors are taken into account."
examples:
  - "A Virology question from MMLU marked 'wrong ground truth': the annotator finds that the dataset's answer key does not match the single valid answer."
method: "Each of the 5,700 questions has an error-type label from the annotation protocol (Question Assessment, then Ground Truth Verification). The paper notes that annotators' suggested correct answers are not used to replace the MMLU labels; models are compared on the questions with and without errors."
metric: "Accuracy (% correct)."
format: "MMLU's four-option multiple choice."
limitations: "Only 100 of the questions in each subject are annotated; the paper notes that the remaining 8,342 MMLU questions are not covered. The suggested corrections do not replace the original answer keys, and the content is MMLU's own."
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2406.04127", title: "Are We Done with MMLU? (Gema et al., 2024)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2406.04127", title: "Are We Done with MMLU?, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/edinburgh-dawg/mmlu-redux", title: "MMLU-Redux dataset (Edinburgh DAWG)", trust: 0.9 }
  - { url: "https://github.com/aryopg/mmlu-redux", title: "MMLU-Redux GitHub repository", trust: 0.85 }
related: [benchmark/mmlu, benchmark/mmlu-pro, benchmark/gpqa]
---

## What It Tests
MMLU's academic knowledge across 57 subjects, on questions whose **errors are labelled**.

- **Paper's estimate** — 6.49% of MMLU questions contain errors.
- **Uneven by subject** — 57% of the analysed Virology questions had errors (wrong ground truth 33%, multiple correct answers 4%, unclear questions 14%).

## How It Scores
**Accuracy**, which can be computed on all 5,700 questions or only on those annotated as error-free. Each question carries a label from the protocol:

- **Question Assessment** — is the question and its options clear.
- **Ground Truth Verification** — no correct answer, multiple correct answers, or a wrong ground truth.

## Format
MMLU's four-option multiple choice; the contribution is the annotation, not a new question format.

## Limitations
- **Partial coverage.** 100 questions per subject; 8,342 MMLU questions are not annotated.
- **Keys not replaced.** Annotators' suggested answers do not replace MMLU's labels.

## Example
An annotator reads an MMLU question, checks that it is clear, that one option is valid and that this option matches the dataset's key; a mismatch is labelled **wrong ground truth**.
