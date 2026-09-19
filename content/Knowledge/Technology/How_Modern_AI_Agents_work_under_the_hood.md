---
created: 2026-06-24 10:04
modified: 2026-06-25 12:00
tags:
  - note
  - ai-agents
  - llm
status: complete
source:
related: /
---

# How Modern AI Agents Work Under the Hood

> [!summary] TL;DR
> AI Agents are autonomous systems that combine LLMs with external tools through iterative feedback loops (Reasoning + Action) to solve complex, multi-step goals.

## 📝 Table of Contents

1. [LLMs: The Core Engine](#llms)
2. [The System Prompt: Setting the Persona](#system-prompt)
3. [Tokens & Streaming: The Communication Layer](#tokens)
4. [Agents: AI with Agency](#agent)
   - [The Agent Loop (ReAct Pattern)](#agent-loop)
5. [Tools & Function Calling: Bridging the Gap](#tools)
6. [LLM Conversation Agents: Memory & State](#llm-conversation-agents)
7. [Coding Agents: RAG & File Systems](#coding-agents)
8. [Provider APIs: The Infrastructure](#provider-apis)
9. [Terminal Projects & Practical Examples](#terminal-projects)

---

## LLMs
- **Definition:** Large Language Models are AI systems trained on massive datasets to predict the next token in a sequence, effectively understanding and generating human-like text.
- **Training:** Models ingest billions of tokens (books, code, web data) to learn semantic patterns, logic, and factual relationships.
- **Architecture:** Built on the **Transformer** architecture, specifically utilizing "Attention" mechanisms to weigh the importance of different parts of the input text relative to each other.
- **Example:** `Input: The capital of India is -> LLM -> Output: New Delhi.`

## System Prompt
- **Definition:** A hidden instruction layer that defines the model's behavior, constraints, and personality before the user interaction starts.
- **Example:**
  ```markdown
  <|system|>
  You are a senior software engineer. 
  Explain concepts simply but accurately.
  Do not mention competitors.
  <|end|>
  <|user|>
  What is a pointer?
  <|end|>
  ```

## Tokens (Streaming vs One-shot)
- **What are they?** The atomic unit of LLM processing. Usually ~4 characters or 0.75 words.
- **Streaming (SSE):** Uses **Server-Sent Events** to send tokens as they are generated.
  - **Why not WebSockets?**
    - **Simplicity:** One-way data flow (LLM -> User) is all that's needed.
    - **Overhead:** WebSockets require persistent connections and complex load balancing.
    - **Cache-friendly:** Standard HTTP protocols work better with existing infra.
- **One-shot:** Waits for the full completion before sending the response. Good for APIs where latency isn't a UI concern.

## Agent
- **The Formula:** `AI (Reasoning) + Tool (Capability) = Agent`
- **Capabilities:**
  1. **Goal-Oriented:** Accepts high-level objectives (e.g., "Research and write a report on X").
  2. **Autonomous:** Decides the sequence of steps without constant user input.
  3. **Interactive:** Uses tools and observes outcomes to correct its path.

### Assistant vs Agent
- **Assistant:** Reactive, answers questions, follows direct instructions in a single turn.
- **Agent:** Proactive, breaks down tasks, executes code, and iterates until the goal is met.

### Agent Loop (ReAct)
The most common pattern is **Reason + Act (ReAct)**:
1. **Thought:** The model analyzes the goal and current state.
2. **Action:** The model selects a tool to call.
3. **Observation:** The system executes the tool and feeds the result back to the LLM.
4. **Iterate:** Repeat until the "Thought" determines the goal is reached.

## Tools (Function Calling)
- **The "Hands" of the AI:** Allows the model to interact with the real world (Web search, DB queries, Python execution).
- **How it works:**
  - The model is provided with a **JSON Schema** of available functions.
  - Instead of text, the model outputs a structured JSON object: `{"tool": "search", "query": "weather in NYC"}`.
  - The orchestrator (your code) runs the function and returns the output to the model.

## LLM Conversation Agents
- **Memory Management:**
  - **Short-term:** The immediate context window (sliding window or summary of previous turns).
  - **Long-term:** Vector databases (RAG) to retrieve relevant past interactions or documents.
- **Statefulness:** Maintaining the "vibe" and specific variables (user preferences, task progress) across a long session.

## Coding Agents
- **Specialized Workflows:**
  - **RAG for Code:** Indexing a codebase so the agent "knows" the project structure.
  - **LSP Integration:** Using Language Server Protocols for real-time linting and type checking.
  - **Sandboxed Execution:** Running generated code in Docker containers to verify fixes before applying them.

## Provider APIs
- **Closed Source:** OpenAI (GPT-4o), Anthropic (Claude 3.5 Sonnet), Google (Gemini 1.5 Pro).
- **Open Source/Local:** Meta (Llama 3), Mistral. Run locally via **Ollama** or **vLLM**.
- **Orchestration Frameworks:** LangChain, CrewAI, AutoGPT, and the Gemini CLI.

## Terminal Projects
- **CLI Agents:** Simple scripts that pipe terminal output to an LLM for debugging.
- **Voice-to-CLI:** Using Whisper (STT) -> LLM -> Bash execution.
- **Personal Indexer:** A local RAG system for searching through personal markdown notes via the terminal.

---

## 📌 Key Points
- Agents are not "magic"; they are loops of text completion intercepted by software.
- The quality of an agent depends heavily on the **System Prompt** and the **Tool Definitions**.
- **Latency** is the biggest bottleneck (Streaming helps UI, but the loop takes time).

## 🔗 Connections
| Relation   | Link                                            |
| ---------- | ----------------------------------------------- |
| Related to | [[LLM Fundamentals]], [[RAG Architectures]]     |
| Source     | Personal Research, DeepLearning.ai Agent Course |
| Follow-up  | Build a simple ReAct loop in Python             |

## 📎 Attachments
- ![[Agent_Loop_Diagram.png]]
- ![[Tool_Calling_Flow.png]]

## 🏷️ Tags
#ai #agents #llm #software-engineering #machine-learning
