---
id: standard/nist-ai-risk-management-framework
type: standard
slug: nist-ai-risk-management-framework
schema_version: 3.0
title: "NIST AI Risk Management Framework (AI RMF 1.0)"
issuer: "National Institute of Standards and Technology (NIST)"
kind: framework
version: "AI RMF 1.0 (NIST AI 100-1)"
published: 2023-01-26
status: "Released; NIST states that AI RMF 1.0 is being revised as part of the White House AI Action Plan"
url: "https://www.nist.gov/itl/ai-risk-management-framework"
applies_to: "Organizations designing, developing, deploying or using AI systems, in any sector, on a voluntary basis."
tags: [risk-management, governance, nist, generative-ai]
updated: 2026-11-21
sources:
  - { url: "https://www.nist.gov/itl/ai-risk-management-framework", title: "NIST: AI Risk Management Framework", trust: 0.95 }
  - { url: "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf", title: "NIST AI 100-1: Artificial Intelligence Risk Management Framework (AI RMF 1.0)", trust: 0.95 }
  - { url: "https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence", title: "NIST AI 600-1: Artificial Intelligence Risk Management Framework: Generative Artificial Intelligence Profile", trust: 0.95 }
  - { url: "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf", title: "NIST AI 600-1 (PDF)", trust: 0.95 }
---

## What It Is
A **voluntary framework** from the US National Institute of Standards and Technology (NIST) for managing risks of AI to individuals, organizations and society. It is non-sector-specific and use-case agnostic.
Its companion, the **Generative AI Profile (NIST AI 600-1)**, was released on 2024-07-26 and applies the framework to generative AI.

## Structure
**Part 1** frames AI risk and lists the characteristics of trustworthy AI systems:

- Valid and reliable; safe; secure and resilient; accountable and transparent; explainable and interpretable; privacy enhanced; fair with harmful biases managed.

**Part 2, the Core,** has four functions, each broken into categories and subcategories:

- **GOVERN** applies to all stages of an organization's AI risk management.
- **MAP**, **MEASURE** and **MANAGE** apply in system-specific contexts and at specific stages of the AI lifecycle.

**The Generative AI Profile** defines twelve risks novel to or exacerbated by generative AI, then suggests actions to govern, map, measure and manage them:

- CBRN (chemical, biological, radiological or nuclear) Information or Capabilities
- Confabulation
- Dangerous, Violent, or Hateful Content
- Data Privacy
- Environmental Impacts
- Harmful Bias or Homogenization
- Human-AI Configuration
- Information Integrity
- Information Security
- Intellectual Property
- Obscene, Degrading, and/or Abusive Content
- Value Chain and Component Integration

## Why It Matters For LLM Applications
The profile names **large language models** among the cross-sector uses it addresses, and its risks include confabulation, which it defines as confidently stated but false content.
Its suggested actions for these risks are organized under the framework's four functions.
