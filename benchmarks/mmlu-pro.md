---
id: benchmark/mmlu-pro
type: benchmark
slug: mmlu-pro
schema_version: 1.0
title: "MMLU-Pro"
name: MMLU-Pro
category: Knowledge & Academic Reasoning
description: "A harder, reasoning-focused MMLU with 10 answer choices and cleaned questions."
what_is_it: "A rebuilt MMLU that expands each item to 10 options (instead of 4), removes trivial and noisy questions, and adds more reasoning-heavy problems so the test discriminates again at the frontier."
why_people_use_it: "MMLU saturated; MMLU-Pro restores headroom. It has become the de facto knowledge-reasoning benchmark on leaderboards like HF Open LLM v2 and Artificial Analysis."
examples:
  - "In quantum mechanics, which best describes the Born rule? (one correct answer among 10 options, e.g. 'it gives the probability of a measurement outcome from the squared amplitude of the wavefunction')"
method: "5-shot chain-of-thought multiple-choice over ~12,000 questions across 14 disciplines; the answer letter is parsed from the CoT response."
metric: "Accuracy (% correct)."
format: "5-shot CoT, 10-way multiple-choice."
saturation: medium
limitations: "Newer, so less contamination data exists — but it is not immune. The 10-option format and CoT dependence make scores sensitive to answer-parsing and prompt template. Still knowledge-centric, so it under-tests generation, tool use, and long-context skills."
citation: "https://arxiv.org/abs/2406.01574"
worked_examples:
  - task_type: "College physics — quantum mechanics"
    input: "In quantum mechanics, which statement best describes the Born rule?"
    options:
      - "It gives the probability of a measurement outcome as the squared modulus of the wavefunction amplitude."
      - "It determines the allowed energy levels of a bound state."
      - "It sets the canonical commutation relation between position and momentum."
      - "It describes the deterministic, unitary time evolution of the wavefunction."
      - "It quantizes the electromagnetic field into photons."
      - "It forbids two identical fermions from occupying the same quantum state."
      - "It relates a particle's wavelength to its momentum."
      - "It states that energy is emitted only in discrete quanta."
      - "It couples a particle's spin to an external magnetic field."
      - "It bounds the product of position and momentum uncertainties."
    success:
      answer: "A — It gives the probability of a measurement outcome as the squared modulus of the wavefunction amplitude."
      why: "The Born rule maps |ψ|² (the squared amplitude) to the probability density of a measurement outcome."
    failure:
      answer: "B — It determines the allowed energy levels of a bound state."
      why: "That is the time-independent Schrödinger equation's eigenvalue problem, not the Born rule — a plausible mix-up of two core quantum-mechanics results, and the wide option set removes easy elimination."
    checks: "Distinguishing closely-related quantum-mechanics principles under a 10-option format that penalizes shallow keyword matching."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Mathematics — differential calculus"
    input: "What is the derivative of f(x) = x·ln(x) with respect to x?"
    options:
      - "ln(x) + 1"
      - "1/x"
      - "ln(x)"
      - "x·ln(x) + 1"
      - "ln(x) − 1"
      - "1"
      - "x/ln(x)"
      - "ln(x) + x"
      - "1/(x·ln(x))"
      - "x^x"
    success:
      answer: "A — ln(x) + 1"
      why: "Product rule: f′(x) = (1)·ln(x) + x·(1/x) = ln(x) + 1."
    failure:
      answer: "C — ln(x)"
      why: "It differentiates the first factor but drops the second product-rule term x·(1/x) = 1 — a classic incomplete product-rule error."
    checks: "Correctly applying the product rule, including the often-dropped second term."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Economics — microeconomics"
    input: "A firm in a perfectly competitive market maximizes profit by producing the quantity at which which condition holds?"
    options:
      - "Marginal cost equals marginal revenue (MC = MR)"
      - "Price equals average total cost"
      - "Marginal cost is at its minimum"
      - "Total revenue is at its maximum"
      - "Average variable cost equals price"
      - "Marginal revenue equals average revenue"
      - "Marginal cost equals average total cost"
      - "Price equals the marginal cost of the first unit"
      - "Total cost equals total revenue"
      - "Marginal utility equals price"
    success:
      answer: "A — Marginal cost equals marginal revenue (MC = MR)"
      why: "Profit is maximized where the cost of the last unit equals the revenue it brings; in perfect competition P = MR, so this is MC = MR = P."
    failure:
      answer: "D — Total revenue is at its maximum"
      why: "Maximizing revenue ignores costs; the profit-maximizing output where MC = MR is generally below the revenue-maximizing quantity."
    checks: "Applying the marginal (MC = MR) profit-maximization rule rather than a revenue or price heuristic."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Biology — Mendelian genetics"
    input: "In a dihybrid cross between two heterozygotes (AaBb × AaBb) with independent assortment, what fraction of the offspring display both recessive phenotypes?"
    options:
      - "1/16"
      - "9/16"
      - "3/16"
      - "1/4"
      - "1/2"
      - "1/8"
      - "3/4"
      - "1/9"
      - "2/16"
      - "0"
    success:
      answer: "A — 1/16"
      why: "Each recessive homozygote (aa, bb) occurs with probability 1/4; by independent assortment, 1/4 × 1/4 = 1/16."
    failure:
      answer: "C — 3/16"
      why: "3/16 is the fraction showing one dominant and one recessive trait in the 9:3:3:1 ratio, not both recessive — a misread of which cell of the ratio the question asks for."
    checks: "Computing a joint probability under independent assortment and reading the 9:3:3:1 ratio correctly."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Computer science — algorithm complexity"
    input: "What is the worst-case time complexity of searching for a key in a balanced binary search tree with n nodes?"
    options:
      - "O(log n)"
      - "O(n)"
      - "O(1)"
      - "O(n log n)"
      - "O(n²)"
      - "O(√n)"
      - "O(log² n)"
      - "O(2ⁿ)"
      - "O(n!)"
      - "O(n³)"
    success:
      answer: "A — O(log n)"
      why: "A balanced BST keeps height Θ(log n), so a search descends at most O(log n) nodes."
    failure:
      answer: "B — O(n)"
      why: "O(n) is the worst case for an unbalanced/degenerate BST that has collapsed into a linked list; balancing is exactly what guarantees logarithmic height."
    checks: "Recognizing that the balance invariant bounds the tree height and thus the search cost."
    scoring: "Exact-match accuracy on the selected option."
tags: [knowledge, multiple-choice, reasoning]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2406.01574", title: "MMLU-Pro: A More Robust and Challenging Multi-Task Language Understanding Benchmark (Wang et al., 2024)", trust: 0.95 }
related: [benchmark/mmlu, benchmark/gpqa]
---

## What It Tests
The same broad academic knowledge as MMLU but with a heavier reasoning load and far less room for
lucky guessing. Ten answer options drop the random baseline from 25% to ~10%, and distractors are
written to be plausible.

## How It Scores
Accuracy across ~12,000 questions in 14 disciplines. Because the choice set is wider and questions
are cleaner, a given percentage is meaningfully harder than on MMLU — roughly, 70% on MMLU-Pro is
comparable in difficulty to ~85% on MMLU.

## Format
Five-shot with chain-of-thought is the reference protocol; the model reasons then emits a final
letter that is regex-parsed. Scores shift several points if run 0-shot or with a different template,
so cross-report comparisons must match the protocol.

## Limitations
It is knowledge-and-reasoning only — it says nothing about coding, agentic behavior, or long
context. Contamination pressure is lower than MMLU's today but grows as the set ages. Answer parsing
under CoT is a real source of measurement noise.

## Example
"In the context of quantum mechanics, which of the following best describes the significance of the
Born rule?" among ten options. The correct choice states that the Born rule gives the probability of
a measurement outcome as the squared modulus of the corresponding amplitude of the wavefunction — a
distractor that "it determines the energy levels of bound states" (that is the Schrödinger equation)
is designed to catch shallow pattern-matching.
