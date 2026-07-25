# 🚀 LLM Gateway Architecture & Implementation (`llm-gateways-tut`)

This repository contains hands-on code and learning experiments designed to demonstrate how to build and integrate an **LLM Gateway** into production AI applications using **LiteLLM** and **LangChain**[cite: 3].

> **Disclaimer**: This code was built for personal learning and study purposes based on an educational tutorial by **Krish Naik**[cite: 3]. No original authorship of the core gateway architecture concepts is claimed[cite: 3].

---

## 📖 Overview & Architecture

An **LLM Gateway** acts as a smart middleware proxy sitting between your generative AI applications and underlying model providers (OpenAI, Anthropic, Groq, Gemini, etc.)[cite: 3]. It abstracts model integrations behind a unified API, preventing vendor lock-in, eliminating single points of failure, and adding enterprise controls[cite: 3].

                 ┌─────────────────────────────┐
                 │       Your Application      │
                 │  (Chatbot, RAG, Agent, etc) │
                 └──────────────┬──────────────┘
                                │
                                ▼
                 ┌─────────────────────────────┐
                 │       LLM GATEWAY           │
                 │  • Unified Routing          │
                 │  • Automatic Fallbacks      │
                 │  • In-Memory Caching        │
                 │  • Cost & Latency Tracking  │
                 │  • Guardrails & Callbacks   │
                 └──────┬─────┬─────┬─────┬────┘
                        │     │     │     │
                        ▼     ▼     ▼     ▼
                     OpenAI Claude Gemini Groq

---

## ✨ Key Capabilities Implemented

1. **Unified API Abstraction**: Invoke 100+ model endpoints (OpenAI, Anthropic, Groq, etc.) using LiteLLM's standard `completion()` signature[cite: 3].
2. **Automatic Multi-Provider Fallbacks**: Graceful failover to backup models when primary providers experience outages or rate limits[cite: 3].
3. **Exact-Match In-Memory Caching**: Caching identical queries with `litellm.cache` to reduce API spend and lower response times[cite: 3].
4. **Real-Time Cost Tracking**: Token-level cost calculation per call using LiteLLM's built-in pricing engine (`completion_cost`)[cite: 3].
5. **Intelligent Routing Strategies**: Configuring `Router` instances for `least-busy`, `latency-based-routing`, and alias deployment pools[cite: 3].
6. **Observability & Logging Callbacks**: Registering custom `success_callback` and `failure_callback` hooks to generate audit logs with token counts, latency, and user tags[cite: 3].
7. **LangChain & LCEL Integration**: Wrapping LiteLLM models via `ChatLiteLLM` and chaining them with `.with_fallbacks()`[cite: 3].
8. **LiteLLM Native Guardrails**:
   * **PII Redaction**: Regex-based scrubbing for emails, phone numbers, PAN, Aadhaar, and credit cards before prompt dispatch[cite: 3].
   * **Prompt Injection Defense**: Intercepting jailbreaks and instruction overrides using input callbacks[cite: 3].
   * **Forbidden Topic Filtering**: Detecting prohibited keywords and raising custom exceptions prior to API calls[cite: 3].

---

## 📁 Repository & Notebook Breakdown

The notebook (`llm_gateway.ipynb`) is structured into distinct modules[cite: 3]:

| Section | Description | Key Modules/Functions |
| :--- | :--- | :--- |
| **1. Setup & Environment** | Loading environment variables for OpenAI, Anthropic, and Groq[cite: 3]. | `dotenv`, `litellm`[cite: 3] |
| **2. Unified Completion** | Single function call across multiple model vendors[cite: 3]. | `litellm.completion()`[cite: 3] |
| **3. Automatic Fallbacks** | Fallback chain execution during simulated model failures[cite: 3]. | `fallbacks=["gpt-4o-mini", ...]`[cite: 3] |
| **4. Cost & Caching** | Token usage calculation and in-memory response caching[cite: 3]. | `completion_cost`, `litellm.caching.Cache`[cite: 3] |
| **5. Smart Router** | Load balancing across deployment pools and latency routing[cite: 3]. | `litellm.Router`[cite: 3] |
| **6. Custom Callbacks** | Building audit logs via success/failure event handlers[cite: 3]. | `litellm.success_callback`[cite: 3] |
| **7. LangChain Pipelines** | Seamless drop-in replacement for LangChain Chat Models[cite: 3]. | `ChatLiteLLM`, `.with_fallbacks()`[cite: 3] |
| **8. Task-Aware Chatbot** | Query classification routing queries to specialized model chains[cite: 3]. | `smart_chat()`[cite: 3] |
| **9. Gateway Guardrails** | Pre-call inspection hooks for PII, injections, and topic safety[cite: 3]. | `litellm.input_callback`[cite: 3] |

---

---
