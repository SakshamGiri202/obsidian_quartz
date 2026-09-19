---
created: 2026-08-08 21:31
modified: 2026-08-08 21:31
tags:
  - note
  - project-ideas
  - backend
status: in-progress
source: YouTube (Project Ideas video transcript)
related:
  - "[[Projects]]"
---

# Backend Project Ideas

> [!summary] TL;DR
> _11 project ideas from a YouTube video — alternating between easy and hard — for developers who want to build something that stands out from the herd._

## 📝 About This Note

This note is based on a YouTube video transcript. The video argues that watching endless "project idea" videos won't build confidence or help you stand out — you need to actually build something different. It presents **11 project ideas**, alternating between easy and hard difficulty, that cover backend fundamentals like web crawling, WebSockets, background workers, orchestration, and AI/RAG pipelines.

## 🗂️ Project Overview


| # | Project | Difficulty | Main Skills |
|---|---------|-----------|-------------|
| 1 | Search Engine | Easy | Web crawling, tokenization, TF-IDF/BM25, caching |
| 2 | Scribble Clone | Easy→Medium | WebSockets, game rooms, event-driven systems |
| 3 | Food Delivery App with Reels | Medium | Feed UI, ordering logic, external integrations |
| 4 | AI Workflow Automation Tool | Hard | No-code pipelines, LLM orchestration, integrations |
| 5 | LLM Minecraft | Easy→Medium | LLM instances, DAGs, parallel responses |
| 6 | Banana Trading Company | Medium | Game economy, price algorithms, market dynamics |
| 7 | GrabPics (Face Recognition) | Medium | Computer vision, background pipelines, deployment |
| 8 | AI-Powered Search Engine | Hard | Vector embeddings, RAG, semantic search |
| 9 | Anti-Pollution Routes | Medium | Schedulers, scoring engines, notifications |
| 10 | PR Review Tool | Hard | GitHub apps, OAuth, LLM pipelines |
| 11 | Infra Orchestration Platform | Very Hard | Docker, S3, queues, reverse proxies |

---


# 🖥️ Project 1: Search Engine

**Difficulty:** Easy — but depends on how deep you go

### 📌 Topic

Build a search engine that crawls websites and lets users find content by keyword.

### 📝 Description

All you need is a database of multiple websites' content. Build a web crawler to scrape sites like Wikipedia, Medium, GitHub, Reddit, etc. — or focus on a single domain like a search engine for Reddit. Some websites also offer public APIs to fetch content directly.

Then strip the HTML, lowercase it, remove stop words, tokenize it, and do all the basic text transformation steps. Finally, use an algorithm like TF-IDF, BM25, or vectorization — and that's it, you have a search engine.

### 🛠️ What I'm Going to Build

- A web crawler / scraper (or use public APIs)
- Text preprocessing pipeline: strip HTML → lowercase → remove stop words → tokenize
- Search index using TF-IDF, BM25, or vectorization
- Search UI + API

**Extras:** pagination, rate limiting, sharding, caching popular queries, background reindexing, autocomplete with a prefix tree, search analytics (most searched queries, CTR).

**Goal:** A working search engine in under a month — even from a CRUD-only background.

---

# 🖥️ Project 2: Scribble Clone

**Difficulty:** Easy→Medium — the natural next step after a CRUD app

### 📌 Topic

A real-time multiplayer drawing-and-guessing game (like Skribbl).

### 📝 Description

Each player is given a word to draw on a whiteboard and everyone else has to guess the word based on the drawing. The faster someone guesses, the more points they get.

**Learning value:** WebSockets, game room management, event-driven systems, chatting rooms, and the basics of scalability — the most essential components of backend development.

**Spin-offs:** everyone gets the same prompt and votes on the best drawing, or simple multiplayer games like Pong.

### 🛠️ What I'm Going to Build

- WebSocket-based real-time game server
- Game rooms with join/leave and state management
- Live whiteboard (canvas) broadcasting to all players
- Guess/score system with faster-guess-more-points logic
- Chat room per game

**Goal:** A playable multiplayer drawing game demonstrating real-time backend skills and systems-level thinking.

---

# 🖥️ Project 3: Food Delivery App with Reels

**Difficulty:** Medium

### 📌 Topic

A food ordering app where users discover dishes through short video reels and swipe right to order.

### 📝 Description

Users find what they want to eat in a reel format. When they like something, they swipe right to order it. In the initial prototype, you can either just inform the restaurant (they manage delivery logistics on their own) or redirect the user to another app where they can order.

**Why it works:** Finding food on reels where you can visually see it being cooked and eaten is way more appealing than scrolling through fake photos on a marketplace. It's a recipe of Instagram + Zomato.

### 🛠️ What I'm Going to Build

- Reels feed (video feed UI with swiping)
- Dish/catalog data model
- Order flow: swipe right → confirm order → notify restaurant (or redirect to ordering app)
- User accounts and order history

**Goal:** An MVP that blends social video discovery with food ordering.

---

# 🖥️ Project 4: AI Workflow Automation Tool

**Difficulty:** Hard (anime-level) — the one that lands jobs straight at YC firms

### 📌 Topic

A low-code/no-code platform to build and orchestrate generative AI pipelines for non-technical people.

### 📝 Description

In the builder, you create pipelines using blocks and third-party integrations, e.g. automatically generating summaries for daily standups and mailing them, or a multi-agent tech review / refinement tool. Then you can save the pipeline and deploy it permanently as a background worker, integrate it as an API in your application, or use it directly from the browser.

**Extra features:** save and reuse templates (ironically one of the hardest features to code) and a knowledge base for RAG-like purposes (data will eat your wallets).

**The real challenge:** The coding logic for dynamic pipelines is not the hard part. Integrating third-party applications and creating modular, scalable infrastructure — that is the real hell.

### 🛠️ What I'm Going to Build

- Visual pipeline builder with blocks and third-party integrations
- Dynamic pipeline execution engine (DAG runner)
- Template saving/reuse
- Knowledge base for RAG
- Deploy options: background worker, API integration, browser

**Goal:** A platform with no ceiling on extensibility that lands jobs.

---

# 🖥️ Project 5: LLM Minecraft

**Difficulty:** Easy→Medium — a much easier version of the workflow automation tool

### 📌 Topic

"Farm" LLMs like a real-time strategy game — place, configure, and run LLM instances in parallel.

### 📝 Description

Inspired by the LobChat community on X. Instead of using LLMs the regular way, this community insists on "farming" LLMs like a real-time strategy game. You place new LLM instances, configure them, and streamline their responses in parallel.

Similar to Project 4, you can craft directed acyclic graphs (DAGs) and have a knowledge base for shared memory and RAG-like purposes. Behind all the big buzzwords are simple API wrappers and a dynamic pipeline algorithm that runs your pipeline.

### 🛠️ What I'm Going to Build

- LLM instance manager (spawn/configure/run)
- Parallel response streaming across instances
- DAG pipeline builder with shared memory / knowledge base
- Optionally skip the front end and focus on the backend

**Goal:** A simplified version of Project 4, then extend with auth, integrations, and advanced steps.

---

# 🖥️ Project 6: Banana Trading Company

**Difficulty:** Complex but fun

### 📌 Topic

A stock-market-style game where players invest in cards and prices change based on market demand and randomness.

### 📝 Description

You start with some currency, invest in certain cards, and prices keep changing according to market demand, algorithms, and randomness. Your goal is to become the richest.

**How it works:**
- Once a player earns a certain amount, they can publish their own card into the market
- You decide what proportion of the stock is owned by the creator
- Decide between limited card supply or unlimited assets (like crypto)

**The challenge:** Design the price algorithm so it doesn't create an infinite-money glitch for the whales. Make it fair with consistent rewards so people keep logging in every day.

### 🛠️ What I'm Going to Build

- Currency + card investment system
- Market with dynamic pricing (demand + algorithm + randomness)
- Player-owned cards (creator stake, supply model)
- Anti-exploit / fair pricing logic
- Leaderboard and an in-game newspaper

**Goal:** A fair, replayable trading game with consistent daily rewards.

---

# 🖥️ Project 7: GrabPics (Face-Recognition Photo Finder)

**Difficulty:** Medium — the hard part is deployment

### 📌 Topic

An app that finds all photos of a person from a large event photo collection using face recognition.

### 📝 Description

You organized an event and now everyone's asking for their pics — or you went to an event and have to search a folder of thousands of photos to find yours. This app solves that headache simply:

1. The organizer uploads all pictures into a private space in the app
2. Your app runs a facial recognition classifier to create profiles of every unique face
3. It maps faces to a list of photo IDs where those faces are present
4. When a new user logs in with the space's credentials, ask them to open the front camera and take a picture
5. Detect the class and return all the pictures where their face appears

**The challenge:** Simple in theory, but in practice you need a background pipeline. The hard part is deploying while keeping server cost and latency to a minimum.

### 🛠️ What I'm Going to Build

- Photo upload + private space management
- Background facial recognition pipeline (unique face profiles → photo ID mapping)
- Selfie-based lookup flow
- Optimized deployment (low cost, low latency)

**Goal:** A free-to-run, monetizable photo-finding service people will be impressed by.

---

# 🖥️ Project 8: AI-Powered Search Engine

**Difficulty:** Hard — what's better than a search engine? An AI-powered one

### 📌 Topic

A search engine that answers queries with AI-generated, context-based responses instead of raw links.

### 📝 Description

In addition to the basics from Project 1:
1. **Semantic search (not BM25)** — use vector embeddings. Look into pgvector, Pinecone, ChromaDB, and Qdrant — they'll save you several days.
2. **RAG answering** — after fetching relevant data, feed it to an LLM to generate an answer based on relevant context and the user's query.

**Going further:** Real AI search engines generate multiple search queries from the user's prompt using a lightweight LLM, and recursively dig into links to gather as much data as possible and remove bias. Imagine Grok having direct access to real-time Twitter APIs.

### 🛠️ What I'm Going to Build

- Web crawler + text preprocessing (from Project 1)
- Vector embeddings + semantic search database (pgvector / Pinecone / ChromaDB / Qdrant)
- LLM answer generation over retrieved context (RAG)
- Optional: multi-query generation and recursive link exploration

**Goal:** A sophisticated RAG system where answer quality depends on your implementation.

---

# 🖥️ Project 9: Anti-Pollution Routes

**Difficulty:** Medium — especially good for hackathons

### 📌 Topic

An app that tells you when pollution is lowest on your route and notifies you in real time.

### 📝 Description

This app pings you when pollution on your defined route is lowest in real time. It provides a list of pollution scores against 15-minute time intervals for your defined routes based on the last week's data, and predicts what it will look like in upcoming days.

**Advanced features:** low-exposure route suggestions (longer but cleaner), traffic data to influence pollution metrics, and special routes for joggers and cyclists.

### 🛠️ What I'm Going to Build

- **Database:** user info + defined routes
- **Backend layer 1:** schedulers that fetch AQI, weather, traffic data
- **Business logic:** pollution score engine (route → score)
- **Background worker:** processes all routes every ~15 minutes
- **Notification service:** mobile, email, or WhatsApp alerts
- **Presentation/data access layer:** depends on architecture

**Goal:** A real-time pollution-aware routing MVP.

---

# 🖥️ Project 10: PR Review Tool

**Difficulty:** Hard

### 📌 Topic

An AI tool that reviews GitHub PRs like a senior developer — detecting bugs, security issues, and bad conventions.

### 📝 Description

Analyzes the diff of every PR you raise, detects bugs, security vulnerabilities, inconsistent conventions, and recommends improvements like a senior developer would.

**The core logic:** Not just a ChatGPT wrapper. You orchestrate a pipeline of LLMs, each responsible for different tasks like code readability, maintainability, nitpick comments, potential security vulnerabilities, etc. (You can build such pipelines with Project 4's tool.)

### 🛠️ What I'm Going to Build

- A GitHub app using their OAuth and APIs
- PR diff ingestion
- Pipeline of specialized LLMs (readability, maintainability, nitpicks, security)
- Scalable architecture for thousands of PRs per minute across accounts

**Goal:** A senior-dev-level automated PR reviewer.

---

# 🖥️ Project 11: Infra Orchestration Platform (Vercel/Cloudflare Clone)

**Difficulty:** The final boss — but much simpler than you think

### 📌 Topic

A managed deployment platform that builds and hosts static sites for users (like Vercel or Cloudflare).

### 📝 Description

For an MVP, deploy static sites into an S3 bucket. The user only provides their GitHub repo or zip, a build command, and an output directory.

**Architecture:**
- **Presentation layer / controller:** authenticates the user, accepts deploy requests, creates deployment records, and enqueues jobs
- **Deployment scheduler:** FIFO queue, single-thread worker, retry logic on failure
- **Runtime executor (main logic):** each worker takes a job, spins a Docker container, clones the repo, installs dependencies, and runs the build
- **Deploy:** the server copies the dist directory out of the container and uploads it to an S3 bucket under a deployment-specific prefix (e.g. deployment ID) — keeps the project immutable
- **Cleanup:** the container is destroyed so the worker can proceed to the next task
- **nginx proxy:** redirects traffic to the correct S3 buckets

**Why it's worth it:** This is basically how Vercel's deployment pipeline functions. Understanding the high-level architecture makes everything seem much easier.

### 🛠️ What I'm Going to Build

- Auth + deploy request API
- FIFO job queue with single-thread worker and retries
- Docker-based build executor (clone → install → build)
- Immutable S3 deployments under deployment-ID prefixes
- nginx reverse proxy to route traffic

**Goal:** A working Vercel-style deployment pipeline for static sites.

---

## 📌 Key Points

- The projects alternate between easy and hard; don't be afraid to go deep.
- The point is to build something that makes you stand out from the herd — not another CRUD app.

## 🔗 Connections

| Relation   | Link         |
| ---------- | ------------ |
| Related to | [[Projects]] |
| Source     |              |
| Follow-up  | [[ ]]        |

## 📎 Attachments

- 

## 🏷️ Tags

#project-ideas #backend
