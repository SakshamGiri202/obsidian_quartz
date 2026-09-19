---
created: 2026-08-06
modified: 2026-08-06
tags:
  - resume
  - project
status: in-progress
source: "[[Projects]]"
related:
  - "[[01_my_auth]]"
  - "[[02_grabpic]]"
  - "[[03_ragscope]]"
  - "[[04_echostream]]"
---

# Resume — Project Section

```
Projects

MyAuth (Auth Service) | TypeScript, Node.js, PostgreSQL, Drizzle ORM, Redis, OAuth 2.0   Aug 2026 – Present
• Built a production-grade authentication service with email/password and session-based auth
• Implemented Argon2id password hashing, secure HttpOnly cookie sessions, and full session revocation
• Added OAuth 2.0/OIDC plugins (Google & GitHub) plus an RBAC role/permission layer
• Hardened with Zod validation, Redis rate limiting, and CSRF/CORS protection
• Shipped email verification/reset flows and a type-safe client SDK (authClient.signIn())
• Built a database migration pipeline for local & production with indexed FKs on high-traffic fields
• Wrote end-to-end API tests covering signup, login, logout, and session expiry flows

AskMyDocs (RAG) | Python, FastAPI, LangChain, PostgreSQL/pgvector, OpenSearch, Docker   Aug 2026 – Present
• Built a domain-specific RAG system with hybrid retrieval (BM25 + vector) and cross-encoder reranking
• Enforced citations in every answer with source-grounded responses and retrieval evaluation
• Gated quality in CI with an automated eval pipeline against a golden dataset
• Containerized the stack with Docker and deployed with a full local + production setup
• Measured retrieval quality with recall@k evals and tuned hybrid weights + reranking for grounding
• Added source-citation enforcement so every claim is traceable back to its document

GrabPic | TypeScript, React, Node.js, PostgreSQL, Redis, Docker, AI face recognition   Aug 2026 – Present
• Built a platform for one-click event photo distribution, replacing manual per-guest photo sending
• Implemented AI face detection and clustering to auto-match guests with their photos across event albums
• Shipped a frictionless claim flow (QR link, no app install) and per-guest photo feeds
• Designed a cost-optimized pipeline matching on thumbnails with time/GPS pre-filtering
• Built per-guest photo feeds with an "request more" refinement loop for missed matches
• Added privacy controls with opt-in claiming and per-photo deletion/opt-out for guests

RAGScope | Python, OpenTelemetry, Langfuse, Grafana, GitHub Actions, Docker   Aug 2026 – Present
• Added end-to-end tracing across the RAG pipeline: query → retrieval → rerank → LLM → response
• Tracked latency (p50/p95 per stage), cost-per-request, and answer-quality metrics
• Built offline eval suites (faithfulness, groundedness, recall) with online proxies for live quality
• Enforced regression gating in CI so PRs failing quality/latency thresholds cannot merge
• Added cost-per-request and burn-rate tracking to catch cost spikes before budgets are hit
• Detected embedding/model drift via periodic shadow re-embedding of a fixed document set

EchoStream | TypeScript, React, WebRTC/WebSocket, Whisper, TTS, Node.js, Redis   Aug 2026 – Present
• Built a real-time voice assistant streaming audio: mic → STT → LLM → TTS → speaker
• Decomposed end-to-end latency into a per-stage budget (≤1.5s reply) with p50/p95 tracking
• Added barge-in (interrupt) support with preemptive cancellation of in-flight LLM/TTS
• Implemented per-stage timeouts and graceful degradation fallbacks (text-only, canned reply, re-ask)
• Streamed chunked TTS so speech starts before the full answer is generated
• Built a recorded-audio replay harness for deterministic latency and quality testing
```

## 🏷️ Tags

# resume # project
