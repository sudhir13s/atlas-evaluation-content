---
id: benchmark/bfcl-v4
type: benchmark
slug: bfcl-v4
schema_version: 1.0
title: "BFCL-V4"
name: BFCL-V4 (Berkeley Function-Calling Leaderboard v4)
category: Agent & Tool Use
description: "The Berkeley Function Calling Leaderboard from UC Berkeley's Gorilla team, which tests function (tool) calling; version 4 adds agentic web search, memory and format-sensitivity tests."
what_is_it: "The Berkeley Function Calling Leaderboard (BFCL), from the Gorilla LLM team at UC Berkeley (Patil et al., ICML 2025), evaluates a model's ability to call functions (tools) accurately. It covers serial and parallel calls, expert-curated and user-contributed functions, multi-turn interaction and, in version 4, agentic web search, memory management and format sensitivity."
why_people_use_it: "Its Abstract Syntax Tree (AST) evaluation checks a call without running it and scales to thousands of functions. The authors write that since its preview BFCL has become the de facto standard for evaluating function calls (ICML 2025 paper)."
examples:
  - "Given schemas for get_weather(location, unit) and book_flight(origin, dest, date), and the request 'what's the weather in Paris in celsius and book me a flight there from London next Friday', emit the correct sequence of calls with correctly typed arguments."
  - "Agentic web search: answer a 3-hop multihop question by issuing search-tool calls and returning a normalized answer string."
method: "Single-turn categories (non-live and live) are checked with AST matching against the expected call; multi-turn categories run multi-step interactions. The v4 web-search category gives the model a search API and a page-fetch function and checks the normalized \"answer\" field by exact match. Memory and format-sensitivity categories were added in v4."
metric: "Overall score in percent. The v4 blog's category table weights it Agentic 40%, Multi-Turn 30%, Live 10%, Non-Live 10% and Hallucination Measurement 10%; format sensitivity has no score impact. The leaderboard also estimates the cost (USD) of the whole benchmark and latency in seconds."
format: "The model receives the user query and the function definitions and must emit calls; multi-turn and agentic categories run multi-step tool loops."
limitations: "AST matching compares a call's structure with the expected call rather than running it. The web-search category depends on a live search service: the v4 blog describes the DuckDuckGo Search API, while the current repository README configures SerpAPI. The leaderboard changed from v1 to v4, so cite the version with any score."
citation: "https://gorilla.cs.berkeley.edu/leaderboard.html"
worked_examples:
  - task_type: "Single-turn function calling (correct tool + typed arguments)"
    input: "Schemas available: get_weather(location, unit), book_flight(origin, destination, date). Request: 'What's the weather in Paris in celsius, and book me a flight there from London next Friday?'"
    success:
      answer: 'get_weather(location="Paris", unit="celsius") ; book_flight(origin="London", destination="Paris", date="2026-08-07")'
      why: "Both functions are selected, argument names match the schema, and unit is the schema-valid \"celsius\"."
    failure:
      answer: 'get_weather(location="Paris", unit="C") ; book_flight(origin="London", destination="Paris", date="2026-08-07")'
      why: "Wrong argument value: unit=\"C\" is a hallucinated enum — the schema expects \"celsius\", so the AST matcher marks the call incorrect even though the function and other args are right."
    checks: "Whether a model maps a natural-language request onto correctly-structured, correctly-typed function calls."
    scoring: "Overall accuracy via deterministic AST matching against a reference call signature."
  - task_type: "Parallel function calling"
    input: "Schema: get_stock_price(ticker). Request: 'Get the current prices for AAPL, MSFT, and NVDA.'"
    success:
      answer: 'get_stock_price(ticker="AAPL") ; get_stock_price(ticker="MSFT") ; get_stock_price(ticker="NVDA")'
      why: "The request maps to three independent calls; all three are emitted in one turn with a correctly-typed single ticker each."
    failure:
      answer: 'get_stock_price(ticker="AAPL, MSFT, NVDA")'
      why: "Wrong argument type — ticker expects one symbol, but three were crammed into a comma string as a single call, so the AST matcher fails it against the reference parallel-call set."
    checks: "Whether a model decomposes a batched request into correctly-typed parallel calls."
    scoring: "AST match against the reference set of parallel calls."
  - task_type: "Function relevance / hallucination detection"
    input: "The only schema available is get_weather(location, unit). Request: 'Book me a table for two at a sushi place tonight.'"
    success:
      answer: "No function call — reply that none of the available tools can make a restaurant reservation."
      why: "None of the provided functions fit the request, so the correct behavior is to abstain rather than invent a call."
    failure:
      answer: 'book_restaurant(cuisine="sushi", party_size=2, time="tonight")'
      why: "Hallucinated tool: book_restaurant is not in the provided schema list — the hallucination track scores fabricating an unavailable function as incorrect."
    checks: "Whether a model abstains when no provided function matches the request."
    scoring: "Hallucination-track accuracy (correct = no fabricated call)."
  - task_type: "Multi-turn function calling (dependent calls)"
    input: "Turn 1 already ran get_user(name='Ada') → {user_id: 4821}. Turn 2 request: 'Cancel her most recent order.' Schemas: get_orders(user_id), cancel_order(order_id)."
    success:
      answer: "get_orders(user_id=4821) → take the newest order_id from the result → cancel_order(order_id=<that id>)"
      why: "It threads the real user_id from the prior turn, fetches orders first, and cancels using the returned order_id in the required order."
    failure:
      answer: "cancel_order(order_id=4821)"
      why: "Wrong argument source and skipped step: it reuses the user_id as an order_id and never calls get_orders, so it cancels nothing valid."
    checks: "Whether a model carries state across turns and orders dependent calls correctly."
    scoring: "Multi-turn AST plus execution match across the turn sequence."
  - task_type: "Agentic web search (multi-hop)"
    input: "3-hop question: 'What is the capital of the country whose team won the 2022 FIFA World Cup?' A search tool is provided; return a normalized answer string."
    success:
      answer: 'web_search("2022 FIFA World Cup winner") → Argentina → web_search("capital of Argentina") → return "Buenos Aires"'
      why: "It chains the search hops and returns the normalized answer field the exact-match grader checks."
    failure:
      answer: 'Returns "Buenos Aires" from memory with no search-tool calls, or answers "Qatar" (the host, not the winner).'
      why: "Acting without searching / wrong hop: the agentic track requires actually issuing the tool calls and following the hop chain, so an unsearched or mis-hopped answer fails."
    checks: "Whether the agent runs a multi-hop search loop instead of guessing from parametric memory."
    scoring: "Exact-match on the normalized answer field over the agentic web-search track."
tags: [agent, tool-use, function-calling, leaderboard]
updated: 2026-11-21
sources:
  - { url: "https://gorilla.cs.berkeley.edu/leaderboard.html", title: "Berkeley Function Calling Leaderboard (BFCL) V4", trust: 0.95 }
  - { url: "https://proceedings.mlr.press/v267/patil25a.html", title: "The Berkeley Function Calling Leaderboard (BFCL): From Tool Use to Agentic Evaluation of Large Language Models (Patil et al., ICML 2025)", trust: 0.95 }
  - { url: "https://gorilla.cs.berkeley.edu/blogs/15_bfcl_v4_web_search.html", title: "BFCL V4 Part 1: Agentic Web Search (Gorilla blog)", trust: 0.9 }
  - { url: "https://gorilla.cs.berkeley.edu/blogs/16_bfcl_v4_memory.html", title: "BFCL V4 Part 2: Agentic Memory Management (Gorilla blog)", trust: 0.9 }
  - { url: "https://github.com/ShishirPatil/gorilla/blob/main/berkeley-function-call-leaderboard/README.md", title: "BFCL README — Gorilla repository", trust: 0.9 }
related: [benchmark/tau2-bench, benchmark/vita-bench, benchmark/swe-bench]
---

## What It Tests
Whether a model **calls functions (tools) correctly** from a request and a set of function definitions. Each version added tests, per the leaderboard's blog list:

- **v1** — AST evaluation of single calls.
- **v2** — enterprise and open-source-contributed functions.
- **v3** — multi-turn interactions.
- **v4** — holistic agentic evaluation: web search, memory management, format sensitivity.

## How It Scores
An **overall score** from weighted categories (v4 blog table): Agentic 40%, Multi-Turn 30%, Live 10%, Non-Live 10%, Hallucination Measurement 10%.

- **AST matching** — single-turn calls are compared structurally with the expected call.
- **Web search** — exact match on the normalized "answer" field.
- **Not scored** — the 26 format-sensitivity configurations (200 cases each) have no score impact.
- **Also reported** — estimated cost of the whole benchmark (USD) and latency (seconds).

## Format
The model gets the query and function definitions and emits calls. The v4 web-search category has 100 human-crafted multihop questions, run as 200 cases (with and without snippets); the memory category has 465 cases across vector-store, key-value and recursive-summarization back ends.

## Limitations
- **Structural check.** AST matching compares a call's structure with the expected one.
- **Live search.** The web-search category depends on an outside search service (DuckDuckGo in the v4 blog, SerpAPI in the current README).
- **Versioned.** v1 to v4 differ; cite the version with any score.

## Example
The v4 web-search blog gives this multihop question: "Who is the founder of the US business that experiences the most growth during 2024?" The model has to search for the business first, then for its founder, and return the result in the "answer" field, which is checked by exact match.
