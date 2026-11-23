---
id: benchmark/mmlu-redux
type: benchmark
slug: mmlu-redux
schema_version: 4.0
title: MMLU-Redux
name: MMLU-Redux
category: Knowledge & Academic Reasoning
description: A 5,700-question subset of MMLU, manually re-annotated so that each question carries a label saying whether, and how, it is erroneous.
what_is_it: 'MMLU-Redux (Aryo Pradipta Gema, Pasquale Minervini and 14 co-authors, 2024; University of Edinburgh with Sapienza University of Rome, University College London and others) is 100 randomly selected questions from each of MMLU''s 57 subjects. 14 human experts re-annotated the 5,700 questions under a hierarchical error protocol: is the question clear, is there a single valid answer, and does it match the dataset''s ground truth. The current release on Hugging Face is MMLU-Redux 2.0, under CC BY 4.0.'
why_people_use_it: From it the authors estimate that 6.49% of MMLU questions contain errors, with 57% of the analysed Virology questions erroneous, and they show that model scores differ from those originally reported once the errors are taken into account.
examples:
- 'Abstract algebra (MMLU-Redux 2.0 on Hugging Face, CC BY 4.0): The element (4, 2) of Z_12 x Z_8 has order. Choices: 4, 8, 12, 6. MMLU key: index 2 (12). Error label: ok'
method: Each of the 5,700 questions has an error-type label from the annotation protocol (Question Assessment, then Ground Truth Verification). The paper notes that annotators' suggested correct answers are not used to replace the MMLU labels; models are compared on the questions with and without errors.
metric: Accuracy = correct answers / questions scored, computed over all 5,700 questions or only over those labelled error-free (label 'ok'), always against MMLU's original key.
format: MMLU's four-option multiple choice.
limitations: Only 100 of the questions in each subject are annotated; the paper notes that the remaining 8,342 MMLU questions are not covered. The suggested corrections do not replace the original answer keys, and the content is MMLU's own.
citation: https://arxiv.org/abs/2406.04127
openness: open
openness_note: All 5,700 questions with their MMLU keys, error labels and annotator notes are public on Hugging Face under CC BY 4.0, and the paper describes the annotation protocol.
worked_examples:
- task_type: Undergraduate biology (re-annotated, labelled ok)
  input: Which enzyme unwinds the DNA double helix at the replication fork?
  options:
  - DNA polymerase
  - Helicase
  - DNA ligase
  - Primase
  success:
    answer: B — Helicase
    why: Helicase breaks the hydrogen bonds between base pairs, separating the two strands at the fork.
  failure:
    answer: A — DNA polymerase
    why: Polymerase is central to replication so it is a tempting pick, but it synthesizes the new strand — it does not unwind the helix.
  checks: Precise recall of enzyme roles, on a question annotators labelled error-free so the result reflects knowledge, not a faulty label.
  scoring: Exact-match accuracy against the original MMLU key, optionally counted only on questions labelled error-free.
- task_type: High-school chemistry (re-annotated, labelled ok)
  input: What is the pH of a neutral aqueous solution at 25 °C?
  options:
  - '0'
  - '7'
  - '14'
  - '1'
  success:
    answer: B — 7
    why: At 25 °C a neutral solution has [H⁺] = [OH⁻] = 10⁻⁷ M, giving pH = 7.
  failure:
    answer: C — 14
    why: 14 is the pH of a strongly basic solution and the top of the ordinary scale — confusing the scale's endpoint with its neutral midpoint.
  checks: Knowing the neutral point of the pH scale and not conflating it with the basic extreme.
  scoring: Exact-match accuracy against the original MMLU key, optionally counted only on questions labelled error-free.
- task_type: US history (re-annotated, labelled ok)
  input: Which document, drafted in 1787, established the framework of the United States federal government?
  options:
  - The Declaration of Independence
  - The U.S. Constitution
  - The Articles of Confederation
  - The Bill of Rights
  success:
    answer: B — The U.S. Constitution
    why: Drafted at the 1787 Philadelphia Convention, it set up the three-branch federal structure of government.
  failure:
    answer: C — The Articles of Confederation
    why: The Articles (1781) were the earlier, weaker framework the Constitution replaced — a plausible but chronologically-off pick.
  checks: Matching a founding document to its date and role, and ordering it against its predecessor.
  scoring: Exact-match accuracy against the original MMLU key, optionally counted only on questions labelled error-free.
- task_type: Statistics (re-annotated, labelled ok)
  input: For a standard normal distribution, approximately what percentage of values fall within one standard deviation of the mean?
  options:
  - 50%
  - 68%
  - 95%
  - 99.7%
  success:
    answer: B — 68%
    why: The empirical (68–95–99.7) rule places about 68% of a normal distribution within ±1 standard deviation of the mean.
  failure:
    answer: C — 95%
    why: 95% is the fraction within ±2 standard deviations, not ±1 — a swap of the empirical-rule tiers.
  checks: Recalling the 68–95–99.7 rule and mapping each percentage to the correct number of standard deviations.
  scoring: Exact-match accuracy against the original MMLU key, optionally counted only on questions labelled error-free.
- task_type: Virology (a subject the re-annotation found error-prone)
  input: What type of nucleic acid makes up the genome packaged in a human immunodeficiency virus (HIV) particle?
  options:
  - Double-stranded DNA
  - Single-stranded RNA
  - Double-stranded RNA
  - Single-stranded DNA
  success:
    answer: B — Single-stranded RNA
    why: 'HIV is a retrovirus: its virion carries a positive-sense single-stranded RNA genome that reverse transcriptase later copies into DNA.'
  failure:
    answer: A — Double-stranded DNA
    why: HIV does form a dsDNA provirus that integrates into the host genome, but that is the intermediate, not the packaged genome — conflating the provirus with the virion genome, exactly the kind of subtlety where a wrong key would mislead.
  checks: Distinguishing a retrovirus's packaged genome from its DNA intermediate, on a question labelled error-free.
  scoring: Exact-match accuracy against the original MMLU key, optionally counted only on questions labelled error-free.
tags:
- knowledge
- multiple-choice
- data-quality
- mmlu
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2406.04127
  title: Are We Done with MMLU? (Gema et al., 2024)
  trust: 0.95
- url: https://arxiv.org/html/2406.04127
  title: Are We Done with MMLU?, full text (arXiv HTML)
  trust: 0.95
- url: https://huggingface.co/datasets/edinburgh-dawg/mmlu-redux-2.0
  title: MMLU-Redux 2.0 dataset card (Edinburgh DAWG, CC BY 4.0)
  trust: 0.9
- url: https://huggingface.co/datasets/edinburgh-dawg/mmlu-redux
  title: MMLU-Redux dataset (Edinburgh DAWG)
  trust: 0.9
- url: https://github.com/aryopg/mmlu-redux
  title: MMLU-Redux GitHub repository
  trust: 0.85
related:
- benchmark/mmlu
- benchmark/mmlu-pro
- benchmark/gpqa
---

## What It Tests

MMLU's academic knowledge across 57 subjects, on questions whose **errors are labelled**.

- **Paper's estimate** — 6.49% of MMLU questions contain errors.
- **Uneven by subject** — 57% of the analysed Virology questions had errors (the paper's Figure 3: wrong ground truth 33%, multiple correct answers 4%, unclear questions 14%).
- **Makers** — Gema, Minervini and colleagues, led from the University of Edinburgh, 2024; 14 human experts did the annotation.

## How It Scores

**Accuracy** = correct answers / questions scored, always against **MMLU's original key**. It can be computed on all 5,700 questions or only on those labelled error-free. Each question carries a label from the protocol:

- **Question Assessment** — are the question and its options clear (`bad_question_clarity`, `bad_options_clarity`).
- **Ground Truth Verification** — no correct answer, multiple correct answers, or a wrong ground truth.
- **ok** — no error found.

## Format

MMLU's four-option multiple choice; the contribution is the annotation, not a new question format.

- **Size** — 57 subjects × 100 questions = 5,700.
- **Extra columns** — `error_type`, a suggested `correct_answer` for some errors, and the annotator's `potential_reason`.
- **Data** — MMLU-Redux 2.0 on Hugging Face, CC BY 4.0.

## Limitations

- **Partial coverage.** 100 questions per subject; 8,342 MMLU questions are not annotated.
- **Keys not replaced.** Annotators' suggested answers do not replace MMLU's labels.

## Example

From the abstract-algebra subset of MMLU-Redux 2.0 on Hugging Face (CC BY 4.0):

- **Question** — "The element (4, 2) of Z_12 x Z_8 has order"
- **Choices** — 4, 8, 12, 6; MMLU key index 2, i.e. **12**.
- **Annotation** — `ok`: the question is clear and the key is right (4 has order 3 in Z_12, 2 has order 4 in Z_8, and the least common multiple is 12).
