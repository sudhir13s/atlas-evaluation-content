---
id: standard/owasp-top-10-for-llm-applications
type: standard
slug: owasp-top-10-for-llm-applications
schema_version: 3.0
title: "OWASP Top 10 for LLM Applications"
issuer: "OWASP Gen AI Security Project"
kind: guidance
version: "2025"
url: "https://genai.owasp.org/llm-top-10/"
applies_to: "Teams that develop, deploy and manage generative AI and large language model applications."
tags: [security, llm-applications, owasp, prompt-injection]
updated: 2026-11-21
sources:
  - { url: "https://genai.owasp.org/llm-top-10/", title: "OWASP Gen AI Security Project: 2025 Top 10 Risk & Mitigations for LLMs and Gen AI Apps", trust: 0.95 }
related: [standard/mitre-atlas]
---

## What It Is
A ranked list of **ten security risks** for applications built on large language models (LLMs), with mitigations, from the Open Worldwide Application Security Project (OWASP) Gen AI Security Project.
The current edition is the **2025 Top 10 Risk & Mitigations for LLMs and Gen AI Apps**; the earlier edition covered 2023-24.

## Structure
The ten items, by their official IDs and names:

- **LLM01:2025** Prompt Injection
- **LLM02:2025** Sensitive Information Disclosure
- **LLM03:2025** Supply Chain
- **LLM04:2025** Data and Model Poisoning
- **LLM05:2025** Improper Output Handling
- **LLM06:2025** Excessive Agency
- **LLM07:2025** System Prompt Leakage
- **LLM08:2025** Vector and Embedding Weaknesses
- **LLM09:2025** Misinformation
- **LLM10:2025** Unbounded Consumption

## Why It Matters For LLM Applications
The list covers risks across the **development, deployment and management lifecycle** of LLM and generative AI applications.
Several items name parts specific to these systems: system prompts, vectors and embeddings, and the agency an LLM-based system is granted.
