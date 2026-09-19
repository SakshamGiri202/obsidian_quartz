---
created: 2026-08-06
modified: 2026-08-06
tags:
  - project
  - idea
  - ai
  - realtime
status: in-progress
source: "[[Projects]]"
related:
  - "[[01_my_auth]]"
  - "[[02_grabpic]]"
  - "[[03_ragscope]]"
---

# EchoStream — Real-Time Multimodal Voice Assistant

> [!summary] TL;DR
> A voice assistant with a **latency budget**, not just a hope. EchoStream streams audio → speech-to-text → LLM → text-to-speech in real time, with every millisecond accounted for, timeout/fallback behavior for when parts fail, and graceful degradation so the conversation never hard-crashes.

## 🎯 Problem

- Voice assistants feel "slow" because **latency compounds** across stages (STT → LLM → TTS) with no visibility into where time goes.
- Naive pipelines **fail hard**: a timeout in TTS kills the whole response, or the user speaks again while the bot is still "thinking."
- Most demos measure *total wall time* but can't break it into a **budget** — interviewers see this as the difference between demo code and a real-time system.

## 💡 Solution

A streaming, latency-budgeted voice pipeline:

1. **Stream in** — chunked audio over WebSocket/WebRTC.
2. **STT** (speech-to-text) — streaming partial transcripts (e.g., Whisper streaming, Deepgram).
3. **LLM reasoning** — start generating as soon as partial transcript is stable (or after end-of-turn).
4. **TTS** (text-to-speech) — stream synthesized audio back in chunks (chunked TTS) so speech starts before the full answer exists.
5. **Latency budget** — explicit per-stage budget (e.g., 400ms STT tail, 800ms LLM first-token, 300ms TTS first-audio) with **preemptive cancellation** and **graceful degradation**.

## 🧠 Architecture — The Latency Budget

| Stage | Target budget | Notes |
|---|---|---|
| Audio capture + network | ~100 ms | chunk size, codec (Opus), region placement |
| STT (streaming) | ~300–500 ms | partials streamed; finalize on silence |
| LLM first token | ~500–800 ms | streaming generation, small model fallback |
| TTS first audio | ~200–400 ms | chunked TTS, stream early chunks |
| **Total end-to-end** | **≤ 1.5 s** (verbal reply starts) | budget each stage, measure every stage |

## 🧱 Build Phases

### 📋 Phase 1 — Streaming Core

- [ ] **Transport**: WebSocket/WebRTC (or Socket.IO) for bidirectional audio streaming.
- [ ] **Audio pipeline**: Opus encode/decode, chunking, mic capture (browser) + playback.
- [ ] **STT integration**: streaming speech-to-text with partial results and end-of-utterance detection (VAD).
- [ ] **TTS integration**: streaming text-to-speech with chunked synthesis and playback buffer.

### 📉 Phase 2 — Latency Instrumentation

- [ ] **Per-stage tracing**: timestamp every hop — capture, network, STT, LLM, TTS, playback.
- [ ] **Latency budget table**: p50/p95 per stage; emit every stage delta as a metric.
- [ ] **Budget enforcement**: if a stage exceeds its slice, **cancel early** and fall back (short reply, re-ask).
- [ ] **Waterfall dashboard**: visualize the full request as a Gantt/waterfall (reuse [[03_ragscope]] tracing).

### 🛟 Phase 3 — Graceful Degradation & Timeouts

- [ ] **Timeout handling**: per-stage timeouts with fallback tiers:
    - LLM too slow → smaller/faster model → canned response → "say that again?"
    - TTS fails → text-only response card (multimodal UI)
    - Network drops → local echo of transcript, retry with backoff.
- [ ] **Barge-in**: user can interrupt; pipeline cancels current TTS/LLM immediately.
- [ ] **Partial-answer streaming**: display and speak partial results as they arrive.
- [ ] **Idle/reconnect**: heartbeat, reconnect, queue cleanup.

### 🔁 Phase 4 — Conversation Quality & Eval

- [ ] **Turn-taking logic**: VAD + silence threshold → clean end-of-turn detection.
- [ ] **Eval harness**: replay recorded conversations to test latency + quality deterministically.
- [ ] **Graceful-failure demo**: kill STT/TTS mid-call to show degradation paths work.
- [ ] **Budget report**: generate a latency budget breakdown per conversation (portfolio artifact).

## ✅ Pros

- **Explicit real-time skill** — latency budgets, timeouts, degradation are exactly what production voice products (e.g., real-time agents) need.
- **Great demo** — a talking assistant with a visible latency waterfall is memorable.
- **Barge-in + streaming TTS** show advanced, interview-friendly engineering.
- **Reuses existing infra**: tracing from [[03_ragscope]], auth from [[01_my_auth]].
- **Combines AI + systems** — not just "call the API."

## ⚠️ Cons / Risks

- **Complexity creep** — real-time audio has many moving parts; **cut scope to one modality** (voice-only MVP).
- **Provider latency varies** — LLM/TTS latency is partly out of your control; the budget must be *measured, not promised*.
- **Hard to test** — needs recorded-audio replay harness and mock providers.
- **Provider cost** — streaming STT/TTS/LLM per interaction adds up.

## 💡 Improvements / Considerations

1. **Start voice-only, add vision later** — multimodal is the stretch goal; nail audio latency first.
2. **Mock the LLM/TTS** in CI for deterministic latency tests (no flaky tests).
3. **Use the latency budget as the headline** — "1.5s from speech to reply, each stage tracked."
4. **Add a "pessimistic mode"** — degrade to text chat automatically when audio is unstable; showcase your degradation design.
5. **Record a demo conversation** with the waterfall overlay — strongest portfolio artifact.
6. **Pick providers with known p95s** and document why each was chosen.

## ❓ Open Questions

- WebRTC (lowest latency, harder) vs. WebSocket (simpler) for audio transport?
- STT: cloud (Deepgram/Whisper API) vs. local (faster for some languages, more work)?
- Should the LLM also accept **text + image** later (true multimodal), or voice-first only?

## 🔗 Connections

| Relation | Link |
|----------|------|
| Related to | [[Projects]] |
| Reuses | [[03_ragscope]] (tracing) |
| Reuses | [[01_my_auth]] (auth) |
| Shares AI infra | [[02_grabpic]] |

## 🏷️ Tags

# project # ai # realtime # voice # streaming
