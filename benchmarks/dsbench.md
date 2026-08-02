---
id: benchmark/dsbench
type: benchmark
slug: dsbench
schema_version: 1.0
title: "DSBench"
name: DSBench
category: Agent & Tool Use
description: "A data-science agent benchmark with two splits — 466 spreadsheet data-analysis questions and 74 end-to-end Kaggle data-modeling tasks."
what_is_it: "DSBench (Jing et al., ICLR 2025) asks how far data-science agents are from being data-science experts. It has two task splits: a data-analysis split of 466 questions over real spreadsheets (from the ModelOff financial-modeling competition), and a data-modeling split of 74 end-to-end machine-learning tasks (from Kaggle competitions) where the agent must build a pipeline and produce predictions. Tasks are deliberately realistic — long contexts, multimodal backgrounds (text plus images and tables), large multi-table data files."
why_people_use_it: "It measures the full data-science loop — read messy real data, reason over multi-table spreadsheets, then write and run an end-to-end modeling pipeline — rather than isolated code snippets. Because the best agents solve only about a third of it while humans do far better, DSBench is a widely-cited, low-saturation yardstick for autonomous data-science agents, and has been used in evaluations of frontier agent products."
examples:
  - "Data analysis (ModelOff spreadsheet): given an election workbook where voter-district codes 105-194 map to 9 named districts, 'How many voters are in the Delta District?' with multiple-choice options A-I."
  - "Data modeling (Kaggle): given a competition's train/test data and task description, write an end-to-end pipeline and output a prediction file; scored by how close it gets to the best leaderboard submission."
method: "Two splits. Data analysis: 466 questions over spreadsheets and multimodal backgrounds; the model returns a final answer, judged for semantic equivalence to the ground truth by a language-model matcher (validated at 100% agreement with human judgment). Data modeling: 74 Kaggle tasks where the agent produces predictions for a held-out test set, each scored by that competition's own metric (accuracy, F1, RMSE, etc.) and then normalized across competitions."
metric: "Data analysis: accuracy (share of questions answered correctly). Data modeling: Relative Performance Gap (RPG) = mean over competitions of max((p - b)/(g - b), 0), where p is the agent's score, b is a weak baseline and g is the best submission — so 0 means no better than baseline and 1 means matching the best; higher is better. The strongest agent reaches only ~34.1% analysis accuracy and ~34.7% RPG."
format: "0-shot agentic: task instructions (which may include images and tables) plus attached data files (Excel/CSV); the agent returns a final answer (analysis) or a prediction file from an end-to-end pipeline (modeling)."
saturation: low
limitations: "The two splits measure different skills and are not one score — analysis is short-answer accuracy, modeling is a normalized performance gap — so 'DSBench score' must name the split. Data-analysis grading relies on a language-model semantic matcher (validated, but still an automated judge). It is data-science specific and English-centric, and the modeling split reuses public Kaggle competitions, so contamination pressure grows over time. Note: DeepSeek model cards list separate internal 'DSBench-FullStack' / 'DSBench-Hard' coding test sets — those are unrelated proprietary benchmarks, not this public data-science DSBench."
citation: "https://arxiv.org/abs/2409.07703"
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
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2409.07703", title: "DSBench: How Far Are Data Science Agents from Becoming Data Science Experts? (Jing et al., ICLR 2025)", trust: 0.95 }
  - { url: "https://github.com/LiqiangJing/DSBench", title: "DSBench — official GitHub repository (data + evaluation)", trust: 0.9 }
  - { url: "https://iclr.cc/virtual/2025/poster/30458", title: "DSBench — ICLR 2025 poster page", trust: 0.9 }
related: [benchmark/swe-bench, benchmark/bfcl-v4, benchmark/livecodebench]
---

## What It Tests
The two halves of real data-science work, kept as separate splits:

- **Data analysis (466 tasks)** — questions over real spreadsheets from the ModelOff financial-modeling competition, with long contexts, multimodal backgrounds (text, images, tables), and multi-table structure. The agent must read the data and return a specific answer.
- **Data modeling (74 tasks)** — end-to-end machine-learning problems from Kaggle competitions. The agent builds a full pipeline (load, clean, feature-engineer, train, predict) and outputs a prediction file for a held-out test set.

The premise in the title — "how far are data-science agents from becoming experts?" — frames the
whole benchmark as a distance-to-human measurement, and the gap is large.

## How It Scores
The two splits use different, appropriate metrics:

- **Data analysis → accuracy.** The agent's final answer is compared to the ground truth by a language-model matcher that checks *semantic* equivalence (not string match); the authors report 100% agreement with human judgment on their validation sample.
- **Data modeling → Relative Performance Gap (RPG).** Each Kaggle task is scored by its own competition metric (accuracy, F1, RMSE, and so on), then normalized: `RPG = mean of max((p - b)/(g - b), 0)`, where `p` is the agent's score, `b` a weak baseline, and `g` the best submission. **0** means no better than baseline, **1** means matching the best — higher is better.

The best agent evaluated reaches only about **34.1%** analysis accuracy and **34.7%** RPG, versus
roughly **64%** human accuracy on a sampled subset of the analysis split.

## Format
Zero-shot and agentic. Task instructions arrive as text that may include images and tables, along
with attached data files (Excel/CSV). For analysis tasks the agent returns a final answer; for
modeling tasks it must produce a prediction file from an end-to-end pipeline it writes and runs.

## Limitations
The splits are **not a single number** — short-answer accuracy and a normalized performance gap
measure different skills, so any "DSBench score" must state the split. Analysis grading depends on
a language-model semantic matcher; though validated, it is still an automated judge. The benchmark
is data-science specific and English-centric, and the modeling split reuses public Kaggle
competitions, so contamination pressure rises as models train on more of the web. One naming trap:
some **DeepSeek model cards list "DSBench-FullStack" and "DSBench-Hard"** — those are the vendor's
*internal* coding test sets (marked as proprietary), unrelated to this public data-science DSBench.

## Example
A data-analysis item gives an Excel workbook of election results where voter-district codes 105-194
map to nine named districts, and asks "how many voters are in the Delta District?" with options
A-I. The correct answer, **"I. 121"**, requires resolving the code-to-district mapping and summing
the right rows. A plausible miss, **"H. 120"**, comes from mishandling a boundary code at the edge
of the Delta mapping — an off-by-one aggregate that no longer matches the ground truth. On the
data-modeling side, the agent instead writes a full pipeline for a Kaggle task and is scored by how
close its predictions come to the best leaderboard submission via the Relative Performance Gap.
