---
created: 2026-06-25 14:14
modified: 2026-08-08 21:31
tags:
  - note
  - project-ideas
  - ai
status: in-progress
source: ""
related:
  - "[[backend_project_idea]]"
  - "[[03_ragscope]]"
  - "[[04_echostream]]"
---

# AI Project Ideas

> [!summary] TL;DR
> _5 AI-focused projects covering the full production AI stack — RAG, offline inference, observability, fine-tuning, and real-time multimodal — to build a standout portfolio._

## 📝 About This Note

This note lists **5 AI project ideas**. Unlike typical "hello world" AI demos, these cover the patterns that matter in production: hybrid retrieval, local model inference, monitoring and eval, efficient fine-tuning, and real-time streaming. Together they demonstrate the 70% of production AI work that most portfolios ignore.

## 🗂️ Project Overview

| # | Project | Focus | Main Skills |
|---|---------|-------|-------------|
| 1 | Production RAG Application | Enterprise "Ask My Docs" | Hybrid retrieval, reranking, citations, CI eval |
| 2 | Local SLM App with Ollama | Fully offline inference | Model benchmarking, quality-vs-speed tradeoffs |
| 3 | Monitoring & Observability | RAG telemetry & quality | Tracing, latency budgets, regression gating |
| 4 | Fine-Tuning with LoRA & DPO | Efficient task tuning | LoRA/QLoRA, preference tuning, metric evals |
| 5 | Real-Time Multimodal App | Streaming voice assistant | WebRTC/streaming, latency budgets, degradation |

---

# 🤖 Project 1: Production RAG Application

**Difficulty:** Core enterprise pattern — the most common in AI right now

### 📌 Topic

Build a domain-specific **"Ask My Docs"** system that answers questions grounded in your own documents.

### 📝 Description

A Retrieval-Augmented Generation (RAG) system with:
- **Hybrid retrieval** — BM25 (keyword) + vector search combined
- **Cross-encoder reranking** — re-rank top results for relevance
- **Citation enforcement** — every answer is traceable to its source documents
- **CI-gated evaluation pipeline** — automated quality checks that block bad changes

This is the single most common pattern in enterprise AI today, and it's the pattern most job descriptions expect you to know.

### 🛠️ What I'm Going to Build

- Document ingestion + chunking pipeline
- Hybrid retriever: BM25 index + vector embeddings (pgvector / OpenSearch)
- Cross-encoder reranker
- LLM generation with citation enforcement
- Golden dataset + eval suite wired into CI (recall@k, faithfulness, groundedness)

**Goal:** A production-grade RAG system where every claim is source-grounded and quality is enforced automatically.

**Related active project:** [[03_ragscope]]

---

# 🤖 Project 2: Local SLM App with Ollama

**Difficulty:** Offline inference & benchmarking

### 📌 Topic

Build an app powered by a **Small Language Model (SLM)** that runs entirely offline via Ollama.

### 📝 Description

Run models fully on your own hardware — no cloud calls. Benchmark inference performance and compare **3 different models on the same hardware**, documenting the quality-vs-speed tradeoffs with real numbers.

Privacy, latency, and cost constraints are real-world concerns. This project shows you understand them and can make engineering tradeoff decisions — something most AI demos never address.

### 🛠️ What I'm Going to Build

- Ollama setup with multiple small models
- Offline SLM-backed app (no external API calls)
- Inference benchmark harness (latency, tokens/sec, memory)
- Side-by-side model comparison on the same hardware
- Documented quality-vs-speed analysis

**Goal:** An offline-first AI app with benchmark-backed model choices.

---

# 🤖 Project 3: Monitoring & Observability

**Difficulty:** The unsung 70% of production AI work

### 📌 Topic

Add full **monitoring and observability** to your RAG system.

### 📝 Description

Production AI isn't just about accuracy — it's about tracing, cost, latency, and quality over time. This project adds:
- **End-to-end tracing** — query → retrieval → rerank → LLM → response
- **Latency tracking** — p50/p95 per pipeline stage
- **Cost-per-request** tracking
- **Quality metrics** — offline eval suites + online proxies
- **Regression gating in CI** — PRs that fail quality/latency thresholds cannot merge

This is 70% of production AI work that nobody puts in their portfolio.

### 🛠️ What I'm Going to Build

- OpenTelemetry tracing across the RAG pipeline
- Dashboards (Grafana/Langfuse) for latency, cost, and quality
- Offline eval suites (faithfulness, groundedness, recall) + online proxies
- CI regression gates on quality/latency/cost thresholds
- Embedding/model drift detection via shadow re-embedding

**Goal:** A RAG system you can trust, debug, and budget for.

**Related active project:** [[03_ragscope]]

---

# 🤖 Project 4: Fine-Tuning with LoRA & DPO

**Difficulty:** Advanced — efficient training and preference tuning

### 📌 Topic

Fine-tune a model for a **specific task** (e.g., JSON extraction or tool-calling) using efficient methods.

### 📝 Description

- Use **LoRA/QLoRA** for efficient training — tiny parameter count, full control
- Add **preference tuning with DPO** to align outputs with what users actually want
- Show **before-and-after metrics with actual numbers** — this is what makes the project credible

### 🛠️ What I'm Going to Build

- Task-specific dataset (JSON extraction / tool-calling)
- LoRA/QLoRA fine-tuning pipeline
- DPO preference tuning stage
- Benchmark suite comparing base vs fine-tuned vs DPO-tuned models
- Documented before-and-after metrics

**Goal:** A fine-tuned model with measurable, evidence-backed improvement.

---

# 🤖 Project 5: Real-Time Multimodal Application

**Difficulty:** Real-time systems engineering

### 📌 Topic

Build a **real-time voice assistant** or streaming pipeline (mic → STT → LLM → TTS → speaker).

### 📝 Description

The value here isn't the model — it's the engineering:
- **Decompose end-to-end latency** into a detailed per-stage budget (e.g., ≤1.5s reply)
- **Graceful degradation** — fallbacks when a stage fails (text-only, canned reply, re-ask)
- **Timeout handling** per stage
- Optional **barge-in/interrupt** support

### 🛠️ What I'm Going to Build

- Streaming pipeline: mic → STT (Whisper) → LLM → TTS → speaker
- Per-stage latency budget with p50/p95 tracking
- Timeouts + graceful degradation fallbacks
- Optional: barge-in with preemptive cancellation, chunked TTS
- Replay harness for deterministic latency/quality testing

**Goal:** A voice assistant that feels instant and never hangs, with real-time systems skills to show for it.

**Related active project:** [[04_echostream]]

---

## 📌 Key Points

- These 5 projects build on each other: RAG → local inference → observability → fine-tuning → real-time.
- Production AI is about evaluation, cost, latency, and reliability — not just calling an API.
- Show real numbers and measurable improvements; that's what makes a portfolio stand out.

## 🔗 Connections

| Relation | Link |
|----------|------|
| Related to | [[backend_project_idea]] |
| Active RAG | [[03_ragscope]] |
| Active Voice | [[04_echostream]] |
| Follow-up | [[ ]] |

## 📎 Attachments

- 

## 🏷️ Tags

#project-ideas #ai #backend
