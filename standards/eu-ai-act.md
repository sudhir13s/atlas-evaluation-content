---
id: standard/eu-ai-act
type: standard
slug: eu-ai-act
schema_version: 3.0
title: "EU AI Act (Regulation (EU) 2024/1689)"
issuer: "European Union"
kind: regulation
version: "Regulation (EU) 2024/1689, as amended by the AI Omnibus"
published: 2024-07-12
status: "In force since 2024-08-01; applicable from 2026-08-02, with exceptions"
url: "https://eur-lex.europa.eu/eli/reg/2024/1689/oj"
applies_to: "AI developers and deployers, for specific uses of AI, and providers of general-purpose AI models, under EU law."
tags: [regulation, european-union, risk-based, general-purpose-ai]
updated: 2026-11-21
sources:
  - { url: "https://eur-lex.europa.eu/eli/reg/2024/1689/oj", title: "EUR-Lex: Regulation (EU) 2024/1689 (Official Journal L, 12.7.2024)", trust: 0.95 }
  - { url: "https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai", title: "European Commission: AI Act", trust: 0.95 }
---

## What It Is
**Regulation (EU) 2024/1689 laying down harmonised rules on artificial intelligence**, published in the Official Journal on 2024-07-12. It sets risk-based rules for AI developers and deployers regarding specific uses of AI.
It was amended by the **AI Omnibus**, which entered into force on 2026-07-27; EUR-Lex shows a consolidated version of that date.

## Structure
The Commission's page names **four levels of risk**:

- **Unacceptable risk:** banned practices; the page lists nine, including social scoring, untargeted scraping to build facial recognition databases, and emotion recognition in workplaces and education.
- **High risk:** uses that can seriously affect health, safety or fundamental rights, such as AI in employment, education, critical infrastructure and access to essential services; strict obligations apply before market entry.
- **Transparency risk** (shown as limited risk in the page's diagram): disclosure duties, such as telling people they are talking to a machine and making AI-generated content identifiable.
- **Minimal or no risk:** no new rules.

**General-purpose AI (GPAI) models** have their own rules on transparency and copyright, plus risk assessment and mitigation for models that may pose systemic risk.

**Dates as the Commission states them:**

- Prohibited practices and AI literacy: 2025-02-02 (the ninth prohibition, added by the AI Omnibus, applies from December 2026).
- Governance rules and GPAI obligations: 2025-08-02.
- Transparency rules: August 2026.
- General application: 2026-08-02.
- High-risk uses in the listed sensitive areas (Annex III): 2027-12-02.
- High-risk AI embedded in regulated products (Annex I): 2028-08-02.

## Why It Matters For LLM Applications
The Commission names **chatbots** and **generative AI** in the transparency tier: people must be told they are interacting with a machine, and generated content must be identifiable, with deep fakes labelled.
Providers of the general-purpose models that many LLM applications are built on carry separate obligations.
