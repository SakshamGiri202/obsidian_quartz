---
created: 2026-08-06
modified: 2026-08-06
tags:
  - project
  - idea
  - ai
status: in-progress
source: "[[Projects]]"
related:
  - "[[01_my_auth]]"
  - "[[02_grabpic]]"
---

# RAGScope — Monitoring & Observability for RAG Systems

> [!summary] TL;DR
> 70% of production AI work is *watching it not fail*. RAGScope adds tracing, latency (p50/p95), cost-per-request, and answer-quality metrics to a RAG system — then bakes regression gates into CI so every change is proven before it ships.

## 🎯 Problem

- RAG apps fail **silently**: embeddings drift, retrievers return junk, LLM cost spikes, answers degrade — and nobody notices until users complain.
- Teams ship changes that **unknowingly regress quality** (better-looking code, worse answers).
- Existing APMs (Datadog, Sentry) track *infrastructure*, not **answer quality** — the thing users actually care about.
- Nobody puts observability in their portfolio, so it's a **differentiator**.

## 💡 Solution

A monitoring + evaluation layer that measures the *whole* RAG pipeline, end to end:

1. **Tracing** — every request traced across the chain: query → retrieval → context assembly → LLM call → response.
2. **Latency** — p50/p95 per stage (embedding, retrieval, LLM, total) to find the bottleneck.
3. **Cost-per-request** — token accounting per call, per user, per day; burn-rate alerts.
4. **Quality metrics** — offline evals (faithfulness, groundedness, recall@k) + online proxies (thumbs up/down, re-ask rate, "answer not found").
5. **CI regression gating** — every PR re-runs an eval suite against a golden dataset; if quality drops past a threshold → **build fails**.

## 🧱 Build Phases

### 📋 Phase 1 — Tracing & Instrumentation

- [ ] **OpenTelemetry (OTel)** setup: trace spans for every pipeline stage.
- [ ] **Instrument RAG core**: span per `embed`, `retrieve`, `rerank`, `llm.generate` with attributes (query, doc_ids, tokens).
- [ ] **Trace propagation** across services (queue jobs, vector store, LLM provider).
- [ ] **Span export** to a backend (OpenSearch/Tempo/Langfuse/LangSmith).
- [ ] **Correlate** request IDs across logs/metrics/traces.

### 📈 Phase 2 — Metrics & Dashboards

- [ ] **Latency**: histograms → p50/p95/p99 per stage + total.
- [ ] **Cost tracking**: per-call token counts (input/output), per-route, per-user, per-day; cost-per-request metric.
- [ ] **Error & retry rates** per stage; provider timeouts/429s.
- [ ] **Grafana dashboards**: pipeline waterfall, latency heatmaps, cost trends, error budget.

### 🧪 Phase 3 — Quality Evaluation

- [ ] **Golden dataset**: curated Q→(ideal answer, expected docs) pairs, versioned in repo.
- [ ] **Offline metrics**: faithfulness, groundedness, context recall/precision, answer similarity.
- [ ] **Online proxies**: feedback thumbs, follow-up/rewrite rate, "I don't know" rate.
- [ ] **CI eval job**: runs suite on every PR, compares vs. baseline, computes deltas.

### 🚦 Phase 4 — Regression Gating & Alerts

- [ ] **Quality gate in CI**: score delta below threshold (e.g., faithfulness < -5%) → **fail build**.
- [ ] **Cost/latency gates**: enforce budgets (e.g., max cost per request) as alerts not hard fails.
- [ ] **Alerts**: burn-rate, p95 spike, cost anomaly, embedding drift detection.
- [ ] **Drift monitoring**: periodic shadow re-embedding of a fixed doc set to catch embedding/model drift.

## ✅ Pros

- **Huge portfolio signal** — nobody shows this; interviewers see production thinking.
- **Solves a real gap** — infrastructure APMs don't measure *answer quality*.
- **Reusable** — the eval harness + OTel setup transfers to any AI project (GrabPic, RAG, agents).
- **CI-gated quality** is a *hard* skill: you design evals, thresholds, and tradeoffs.
- **Composable** — each phase is independently shippable/demoable.

## ⚠️ Cons / Risks

- **Scope creep** — observability has no "done". **Cut the scope:** golden dataset + 3 core metrics + 1 gate.
- **Requires an existing RAG app** — it's a layer on top; needs Project 1 (RAG) built first or in parallel.
- **Time sink on infra** (deploying OTel + Grafana stack).
- **Golden datasets rot** — must be maintained as the app evolves.
- **Hard to quantify ROI** — no shiny UI; results are metrics and failing CI.

## 💡 Improvements / Considerations

1. **Start minimal**: OTel traces → Langfuse/LangSmith SaaS before self-hosting Grafana. Pay for time, not infra.
2. **One golden dataset, not twenty** — 100–200 high-quality pairs beat 2,000 sloppy ones.
3. **Make the quality gate *informational first***, then hard-block once thresholds stabilize (avoid PR chaos on day one).
4. **Track cost *before* quality** — cost blowups are the most common production RAG incident.
5. **Add a "compare" workflow** — same question through 2 models/retrievers side by side; great for demos and model upgrades.
6. **Document the eval rubric** in-repo so future-you/teammates know why a threshold exists.
7. **Export a weekly report** (latency trend, cost, quality) — looks great in portfolio and gives a demo artifact.

## ❓ Open Questions

- Attach to your existing RAG project, or build a fresh toy RAG (e.g., "Ask My Docs") to instrument?
- SaaS eval store (Langfuse/LangSmith) vs. self-hosted (OpenSearch + Grafana)?
- Hard-block CI on quality drops, or warn-only for the first month?

## 🔗 Connections

| Relation | Link |
|----------|------|
| Related to | [[Projects]] |
| Built on | RAG project (Project 1) |
| Reuses | [[01_my_auth]] |
| Shares AI infra | [[02_grabpic]] |

## 🏷️ Tags

# project # ai # observability # rag # devops
