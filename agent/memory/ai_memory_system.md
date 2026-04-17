# AI Agent Memory System Design

## Overview

Modern AI agents typically use a **hybrid memory architecture** composed of:

1. Conversation Memory (Short-term)
2. User Memory (Structured Profile)
3. Knowledge Memory (Long-term / Vector DB)

---

# 1. Conversation Memory (Short-term)

## Definition
Recent chat history within current session.

## Storage
- In-memory (RAM)
- Last N messages

## Trigger
- Always used

## Compression
- Sliding window (keep last N)
- Summarization of older messages

## Usage
- Directly injected into prompt

---

# 2. User Memory (Structured)

## Definition
Stable user profile and preferences.

## Storage
- SQL / structured DB

## Trigger
- Always or rule-based

## Compression
- Structured fields (no need heavy compression)

## Usage
- Inject as structured context

Example:
User uses proxy 127.0.0.1:7890

---

# 3. Knowledge Memory (Long-term / RAG)

## Definition
Important past information extracted from conversations.

## Storage
- Vector DB (embedding + text)

## Trigger
- When context is missing
- When question depends on past

## Compression
- LLM summarization
- Store only meaningful facts

## Usage
- Retrieve via similarity search
- Inject into prompt

---

# Final Architecture

User Query
↓
1. Load recent messages
2. Load user profile
3. Decide if need RAG
4. Retrieve memory if needed
5. Build prompt
6. LLM answer

---

# Key Insight

Short-term memory = immediate context  
User memory = structured facts  
Knowledge memory = semantic retrieval

