---
id: benchmark/dsbench
type: benchmark
slug: dsbench
schema_version: 3.1
title: "DSBench"
name: DSBench
category: Agent & Tool Use
description: "A data-science agent benchmark with two splits — 466 spreadsheet data-analysis questions and 74 end-to-end Kaggle data-modeling tasks."
what_is_it: "DSBench (Jing et al.; arXiv, September 2024; ICLR 2025; University of Texas at Dallas and Tencent AI Lab) asks how far data-science agents are from being data-science experts. It has 466 data-analysis tasks collected from ModelOff, a financial data-analysis competition answered in Excel, and 74 data-modelling tasks collected from Kaggle competitions. Tasks include long contexts, multimodal task backgrounds, large data files and multi-table structures."
why_people_use_it: "It covers both answering questions over real spreadsheets and building an end-to-end modelling pipeline. The authors measured human performance on both splits and report a large gap between the most advanced agent system and humans."
examples:
  - "From the paper's Appendix I (Table 9), a data-analysis question over an attached workbook: How many voters are there in the Delta District? Options run from A (113) to I (121); the answer is I (121)."
method: "Data analysis: GPT-4o acts as a semantic matcher, prompted to output True or False for whether the agent's final answer matches the ground truth; the authors checked 100 sampled predictions by hand and found full agreement. Data modelling: the submitted file is scored with each competition's own metric, and the Relative Performance Gap (RPG) normalises the different metrics."
metric: "Data analysis: accuracy = questions answered correctly / questions. Data modelling: Relative Performance Gap (RPG) = for each competition, (agent score - baseline score) / (best score - baseline score), floored at 0, then averaged over competitions; the baseline is the score of the competition's original submission file (higher is better)."
format: "Agentic tasks with attached data files; a final answer (analysis) or a prediction file from an end-to-end pipeline (modelling)."
limitations: "The two splits use different metrics, so a DSBench score must name its split. The modelling split reuses public Kaggle competitions. The paper's human figures are partial: data-analysis human accuracy comes from 10 sampled competitions, and modelling human RPG from 22 competitions whose contestant code still ran. The dataset is released for non-commercial use only."
citation: "https://arxiv.org/abs/2409.07703"
openness: open
openness_note: "All tasks, data files and evaluation code are public in the official GitHub repository, which restricts the dataset to non-commercial use."
worked_examples:
  - task_type: "Spreadsheet aggregate with a code mapping (data-analysis split)"
    input: "Attached: an Excel workbook of election results where voter-district codes 105-194 map to 9 named districts across several sheets. Question: 'How many voters are there in the Delta District?' (multiple-choice values in the 113-121 range)."
    success:
      answer: "Load the workbook, resolve which district codes belong to Delta from the mapping sheet, sum the voter column across exactly those rows → 121."
      why: "The agent correctly resolves the code-to-district mapping and aggregates the right rows, so the answer semantically matches the ground truth."
    failure:
      answer: "Returns 120 by including or dropping a boundary district code at the edge of the Delta mapping."
      why: "Off-by-one: it mishandles a boundary code, so the aggregate is wrong and no longer semantically matches the ground truth."
    checks: "Whether an agent reads a multi-table spreadsheet, applies a code-to-category mapping, and computes an exact aggregate over the right rows."
    scoring: "Accuracy — a language-model matcher marks the final answer correct if it is semantically equivalent to the ground truth."
  - task_type: "Cross-sheet join (data-analysis split)"
    input: "Attached: a workbook with a Sales sheet (product_id, units) and a Products sheet (product_id, unit_price, category). Question: 'What is the total revenue from the Accessories category?'"
    success:
      answer: "Join Sales to Products on product_id, filter to category='Accessories', compute units × unit_price per row, and sum → the correct revenue total."
      why: "It joins on the correct key and applies the category filter before summing, so the total matches ground truth."
    failure:
      answer: "Sum units × unit_price across ALL products (skipping the category filter), or join on the wrong column so prices misalign to products."
      why: "Wrong join/filter: mismatching the key or dropping the Accessories filter yields a total that does not semantically match the ground truth."
    checks: "Whether an agent correctly joins two tables on the right key and applies the stated filter before aggregating."
    scoring: "Accuracy on the data-analysis split via the semantic-equivalence matcher."
  - task_type: "Financial modeling formula (data-analysis split)"
    input: "Attached: a ModelOff-style workbook with a company's revenue for 2019-2024. Question: 'What is the compound annual growth rate (CAGR) of revenue over the period?'"
    success:
      answer: "Read the first and last year's revenue, apply CAGR = (end/start)^(1/n) − 1 with n = 5 growth periods → the correct percentage."
      why: "It uses the compound-growth formula with the right number of periods, matching the ground-truth CAGR."
    failure:
      answer: "Report the simple average of the year-over-year percentage changes as the 'growth rate.'"
      why: "Wrong formula: averaging annual changes is not CAGR (it ignores compounding), so the figure does not match the ground truth."
    checks: "Whether an agent selects the correct financial formula rather than a superficially similar averaging shortcut."
    scoring: "Accuracy on the data-analysis split via the semantic-equivalence matcher."
  - task_type: "End-to-end classification pipeline (data-modeling split)"
    input: "A Kaggle-style task: train/test CSVs and a description for a binary churn-prediction competition scored by accuracy. Produce a submission.csv of predictions for the held-out test set."
    success:
      answer: "load → clean/impute → fit encoders and a scaler on the TRAIN split only → train a gradient-boosted classifier with cross-validation → predict on test → write submission.csv in the required id,prediction format."
      why: "A leak-free pipeline in the correct submission format scores well above the weak baseline, giving a high Relative Performance Gap toward the best submission."
    failure:
      answer: "Fit the scaler and target encoding on the combined train+test data before splitting, then train and predict."
      why: "Data leakage: fitting preprocessing on the test set inflates validation but generalizes poorly on the real held-out metric, so the competition score barely beats baseline and RPG stays near 0."
    checks: "Whether an agent builds a leak-free end-to-end modeling pipeline and emits the exact submission format."
    scoring: "Relative Performance Gap (RPG) — the normalized distance from a weak baseline (0) to the best leaderboard submission (1)."
  - task_type: "Regression pipeline with submission format (data-modeling split)"
    input: "A Kaggle-style house-price task scored by RMSE: train/test CSVs, a target column in train, and a sample_submission.csv with columns id,SalePrice. Produce predictions for the test set."
    success:
      answer: "log-transform the skewed target, engineer features, train a regularized regressor with CV, inverse-transform predictions, and write id,SalePrice for every test row matching sample_submission."
      why: "Sound modeling plus a correctly-formatted submission for all test ids yields an RMSE well below baseline, so RPG moves toward 1."
    failure:
      answer: "Predict the training mean SalePrice for every row (or write a submission missing some test ids / with a mislabeled column)."
      why: "Predicting a constant is no better than the weak baseline (RPG ≈ 0), and a malformed submission can score nothing at all — either way the normalized gap stays at the floor."
    checks: "Whether an agent produces a genuinely predictive model AND a valid, complete submission file."
    scoring: "Relative Performance Gap (RPG) computed from the competition's own RMSE metric, normalized across competitions."
tags: [agent, data-science, code-generation, spreadsheet, kaggle, multimodal]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2409.07703", title: "DSBench: How Far Are Data Science Agents from Becoming Data Science Experts? (Jing et al., ICLR 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2409.07703", title: "DSBench, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://github.com/LiqiangJing/DSBench", title: "DSBench — official GitHub repository (data, evaluation, non-commercial use)", trust: 0.9 }
related: [benchmark/swe-bench, benchmark/bfcl-v4, benchmark/livecodebench]
---

## What It Tests
The two halves of data-science work, as **separate splits**:

- **Data analysis (466 tasks)** — questions over real spreadsheets from ModelOff, with long contexts, multimodal backgrounds and multi-table structures.
- **Data modelling (74 tasks)** — end-to-end machine-learning problems from Kaggle; the agent submits a prediction file.

## How It Scores
Each split has its own metric:

- **Data analysis → accuracy** = correct answers / questions. A GPT-4o matcher decides whether the final answer means the same as the ground truth.
- **Data modelling → Relative Performance Gap (RPG).** Per competition: (agent score - baseline) / (best score - baseline), floored at 0, then averaged; higher is better.
- **Human reference** — the paper measured humans on 10 sampled analysis competitions and 22 modelling competitions.

## Format
Agentic tasks with attached data files. The agent returns a final answer for analysis tasks, or writes and runs a pipeline that produces a prediction file for modelling tasks.

| Split | Tasks | Source | Metric |
|---|---|---|---|
| Data analysis | 466 | ModelOff competition | Accuracy |
| Data modelling | 74 | Kaggle competitions | RPG |

Data and evaluation scripts are in the `LiqiangJing/DSBench` GitHub repository, for **non-commercial use**.

## Limitations
- **Two scores.** Accuracy and RPG measure different things; name the split.
- **Public sources.** The modelling tasks reuse public Kaggle competitions.
- **Partial human baselines.** Human figures cover a sample of competitions.
- **LLM matcher.** Analysis answers are judged by GPT-4o, not exact match.

## Example
From the paper's Appendix I (Table 9), a data-analysis item: the agent receives an election workbook and the question **"How many voters are there in the Delta District?"**, with options from A (113) to I (121).

- **Correct answer** — I (121).
- **Reported failure** — GPT-4o answered wrongly because it treated the district code as a voter identifier.
