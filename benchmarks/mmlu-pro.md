---
id: benchmark/mmlu-pro
type: benchmark
slug: mmlu-pro
schema_version: 1.0
title: "MMLU-Pro"
name: MMLU-Pro
category: Knowledge & Academic Reasoning
description: "An extension of MMLU with 12,032 more reasoning-focused questions in 14 disciplines and up to ten answer options."
what_is_it: "MMLU-Pro was introduced by Wang et al. (2024). It extends the mostly knowledge-driven MMLU with more challenging, reasoning-focused questions, removes trivial and noisy MMLU questions, and expands the choices from four to ten. It has 12,032 questions in 14 disciplines, including mathematics, physics, chemistry, law, engineering, psychology and health; 83% of questions have ten options, and the average is 9.47."
why_people_use_it: "The authors report that models score 16% to 33% lower than on MMLU, and that sensitivity to prompt wording falls from 4-5% on MMLU to 2% on MMLU-Pro across 24 prompt styles, making it more discriminative."
examples:
  - "In quantum mechanics, which best describes the Born rule? (one correct answer among 10 options, e.g. 'it gives the probability of a measurement outcome from the squared amplitude of the wavefunction')"
method: "The paper's reference setup is 5-shot chain-of-thought (CoT) prompting; the answer letter is extracted from the response."
metric: "Accuracy (% correct)."
format: "5-shot CoT multiple choice, up to ten options."
limitations: "Answer extraction from a CoT response and the prompt template affect scores, so runs must use the same protocol to be compared. It tests knowledge and reasoning in multiple-choice form only, not generation, tool use or long context."
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2406.01574", title: "MMLU-Pro: A More Robust and Challenging Multi-Task Language Understanding Benchmark (Wang et al., 2024)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/2406.01574", title: "MMLU-Pro paper, full text (ar5iv)", trust: 0.95 }
related: [benchmark/mmlu, benchmark/gpqa]
---

## What It Tests
Broad **academic knowledge with a heavier reasoning load** than MMLU, across 14 disciplines.

## How It Scores
**Accuracy** over 12,032 questions. With up to ten options, random guessing scores about 10%.

## Format
The reference protocol is 5-shot chain-of-thought; the final letter is parsed from the response.

## Limitations
- **Protocol-sensitive.** Template and answer parsing change scores.
- **Multiple choice only.** No generation, tool use or long context.

## Example
"In the context of quantum mechanics, which of the following best describes the significance of the
Born rule?" among ten options. The correct choice states that the Born rule gives the probability of
a measurement outcome as the squared modulus of the corresponding amplitude of the wavefunction — a
distractor that "it determines the energy levels of bound states" (that is the Schrödinger equation)
is designed to catch shallow pattern-matching.
