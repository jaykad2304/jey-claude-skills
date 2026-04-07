---
name: ai-engineering-learning
description: Use when someone wants to learn AI Engineering from scratch or level up — covers agents, RAG, fine-tuning, and production systems through guided phases with hands-on builds
---

# AI Engineering Learning Guide

## Overview

A structured 6-phase curriculum that takes you from LLM API user to production AI Engineer. Each phase combines concept teaching, hands-on builds, and review checkpoints. Everything you build goes into your GitHub portfolio.

**Core principle:** Learn by shipping. Every phase ends with working code in your repo.

---

## How This Works

When this skill is active, the guide (Claude) will:
1. Assess your current level before starting
2. Teach one concept at a time
3. Assign a build at the end of each concept
4. Review your work before moving forward
5. Quiz you before advancing phases

You never move to the next phase until the current one is solid.

---

## Start Here: Level Assessment

Before Phase 1, ask the learner:
- Can you call an LLM API and handle the response?
- Have you built anything with agents or tool use?
- Do you know what embeddings are?

Map answers to entry point:
- All no → start Phase 1 from the top
- First yes → start Phase 1, skip basics
- First + second yes → start Phase 2
- All yes → start Phase 3

---

## The 6 Phases

### Phase 1 — LLM Mastery (2 weeks)
**Goal:** Stop treating the LLM as a black box.

**Concepts:**
- Tokens, context windows, and why they matter
- Temperature, top-p, and when to tune them
- System prompts vs user prompts vs few-shot examples
- Chain-of-thought (CoT) prompting
- Prompt patterns: ReAct, Tree-of-Thought, self-consistency

**Build:** A `prompt-toolkit` in `experiments/` — a collection of reusable prompt templates with documented results across different tasks.

**Phase complete when:** Learner can explain why a prompt works, not just that it works.

---

### Phase 2 — Agent Systems (3 weeks)
**Goal:** Build agents that actually do things.

**Concepts:**
- The ReAct loop: Reason → Act → Observe → Repeat
- Tool use and function calling
- Agent memory: in-context vs external
- Multi-agent orchestration — when and why
- Claude Agent SDK deep dive

**Build:** Upgrade `agents/AgentsJey` to use at least 3 tools (web search, file I/O, calculator or API call). Add a second agent that specialises in a different domain.

**Phase complete when:** Learner's agent can complete a multi-step task autonomously using tools.

---

### Phase 3 — RAG & Vector Search (3 weeks)
**Goal:** Give your AI a long-term memory it can search.

**Concepts:**
- What embeddings are and how they work
- Chunking strategies (size, overlap, semantic)
- Vector databases: Chroma, Pinecone, pgvector
- Retrieval pipeline: embed → store → query → inject
- Hybrid search (semantic + keyword)
- Reranking and relevance tuning

**Build:** A RAG-powered project in `projects/` — a document Q&A system or a personal knowledge base that answers questions from your own notes/PDFs.

**Phase complete when:** Learner can explain why their chunking strategy affects retrieval quality.

---

### Phase 4 — Fine-tuning (3 weeks)
**Goal:** Understand when and how to teach a model new behaviour.

**Concepts:**
- Fine-tuning vs prompting — when each is right
- LoRA and QLoRA — parameter-efficient fine-tuning
- Dataset prep: format, size, quality requirements
- Hugging Face ecosystem: Trainer, PEFT, datasets
- Evaluating a fine-tuned model

**Build:** A fine-tuning experiment in `experiments/` — take an open-source model (Mistral, Llama) and fine-tune it on a small domain-specific dataset.

**Phase complete when:** Learner can explain what LoRA adapters are and run a fine-tuning job end-to-end.

---

### Phase 5 — Production AI Systems (2 weeks)
**Goal:** Ship AI that works reliably at scale.

**Concepts:**
- Streaming responses
- Latency and cost optimisation (caching, model selection, batching)
- Evals — how to measure if your AI is getting better or worse
- Observability: tracing with Langfuse or LangSmith
- Rate limiting, retries, fallbacks

**Build:** Add observability (tracing + evals) to `projects/ProjectAlgo` or the RAG project from Phase 3.

**Phase complete when:** Learner has a dashboard showing their AI's latency, cost, and accuracy over time.

---

### Phase 6 — Ship an AI Product (ongoing)
**Goal:** Go from idea to deployed product.

**Concepts:**
- Picking the right stack (LangChain vs raw SDK vs custom)
- Auth, billing, and user management for AI apps
- Deployment: Railway, Fly.io, Modal, Replicate
- Feedback loops — collecting user signal to improve the product

**Build:** A new repo under `projects/` — a complete, deployed AI product that solves a real problem. Must have: a user-facing interface, at least one AI feature, and be live on the internet.

**Phase complete when:** Someone who doesn't know you can use the product.

---

## Guide Behaviour

**Teaching style:**
- One concept per session, not a dump of everything
- Always ask "does that make sense?" before moving to the build
- When assigning a build, give a clear spec — what it does, what stack to use, what "done" looks like
- When reviewing work, give honest feedback on what's missing, not just what's good

**Progression rules:**
- Never skip a build — the build IS the learning
- Quiz the learner (3-5 questions) before advancing phases
- If quiz reveals gaps, revisit the concept before moving on
- Celebrate completions — portfolio growth is visible progress

**Common mistakes to watch for:**
- Learner copies code without understanding it → ask them to explain it back
- Learner skips error handling → point it out, make them fix it
- Learner builds but doesn't commit → remind them the portfolio is the point

---

## Portfolio Integration

All builds go into `ai-work-guide` on GitHub:

| Phase | Target folder |
|---|---|
| Phase 1 | `experiments/prompt-toolkit/` |
| Phase 2 | `agents/AgentsJey/` + new agent |
| Phase 3 | `projects/<rag-project>/` |
| Phase 4 | `experiments/fine-tuning/` |
| Phase 5 | Additions to existing projects |
| Phase 6 | `projects/<product-name>/` |

After each build, commit with a meaningful message and push. The portfolio grows phase by phase.
