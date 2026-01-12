# SynthCore

![Version](https://img.shields.io/badge/version-0.1.0-blue)
![Python](https://img.shields.io/badge/python-3.10%2B-brightgreen)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

**SynthCore** is the deterministic orchestration engine of the Nexus Client, responsible for prompt assembly, token budgeting, identity and mood injection, and coordinating memory-aware model behavior under strict context constraints.

---

## Overview

SynthCore is **not** a chatbot loop and **not** a memory store.

It is the **control plane** that governs how a primary language model:
- Receives context
- Allocates tokens
- Integrates memory
- Maintains identity and behavioral continuity
- Degrades gracefully when constraints are exceeded

SynthCore transforms an LLM from a reactive text generator into a **budget-aware, stateful, policy-driven system component**.

---

## Core Responsibilities

SynthCore sits at the center of the Nexus architecture and coordinates multiple subsystems:

- **Prompt Assembly**
  - Constructs structured prompts with deterministic section ordering
  - Enforces system > identity > mood > memory > user priority
- **Token Budgeting**
  - Allocates context budget per section
  - Guarantees output reservation
  - Prevents overflow by controlled degradation
- **Memory Coordination**
  - Requests relevant memory fragments from SynthMemory
  - Enforces diversity and relevance constraints
  - Treats memory as a bounded resource, not an append-only log
- **Identity Injection**
  - Injects stable identity and policy context
  - Ensures behavioral continuity across turns
- **Mood / Affect Injection**
  - Applies decaying PAD (Valence, Arousal, Dominance) signals
  - Provides strategy bias without emotional role-play
- **Graceful Degradation**
  - Drops or compresses lower-priority context under pressure
  - Never sacrifices system or identity guarantees

---

## What SynthCore Is *Not*

SynthCore deliberately avoids responsibilities handled elsewhere:

- ❌ No vector storage (handled by SynthMemory)
- ❌ No embedding generation
- ❌ No tool execution or plugins (Phase 2+)
- ❌ No emotional simulation or persona acting
- ❌ No direct database persistence

This strict separation keeps the orchestration layer predictable and testable.

---

## Architecture Position

```

User Input
│
▼
SynthCore Orchestrator
├── TokenBudget
├── PromptAssembler
├── Identity Provider
├── Mood Engine
└── SynthMemory (retrieval only)
│
▼
Final Structured Prompt
│
▼
Primary LLM

````

SynthCore always remains **in control of context shape and size**, regardless of upstream or downstream complexity.

---

## Key Components

### `SynthCoreOrchestrator`
The central coordinator responsible for:
- Requesting memory
- Applying identity and mood state
- Enforcing token policy
- Producing the final prompt

### `TokenBudget`
- Tracks total context limits
- Reserves output tokens
- Allocates per-section budgets
- Enforces hard limits deterministically

### `PromptAssembler`
- Counts tokens using the target model’s tokenizer
- Appends sections in priority order
- Omits or truncates sections when budgets are exhausted

### Identity Module
- Provides stable system identity and policy constraints
- Injected early and never degraded

### Mood / Affect Module
- Uses a PAD (Valence, Arousal, Dominance) model
- Applies exponential decay toward baseline
- Injects **behavioral bias**, not emotions

---

## Design Principles

SynthCore is built around a few non-negotiable principles:

- **Determinism over cleverness**
- **Explicit budgets beat implicit truncation**
- **Memory is a resource, not a dump**
- **Behavioral continuity without emotional theater**
- **Fail safe, not fail loud**

Every design choice favors predictability and debuggability.

---

## Phase Status

### Phase 1 (Current)
- Deterministic prompt assembly
- Static identity injection
- Decaying mood state
- Episodic memory retrieval (via SynthMemory)
- Token budgeting with graceful degradation
- No tools, no plugins, no agent autonomy

### Phase 2 (Planned)
- Tool orchestration
- Plugin lifecycle management
- Multi-model routing
- Dynamic policy switching
- Advanced telemetry and introspection

---

## Requirements

- Python **3.10+**
- Tokenizer compatible with target LLM (e.g. `tiktoken`)
- Designed to integrate with Nexus Client subsystems

---

## Installation

SynthCore is intended to be used as part of the Nexus Client and is not currently distributed as a standalone package.

Clone and integrate directly:

```bash
git clone https://github.com/otectus/synth_core.git
````

---

## Usage (Conceptual)

```python
orchestrator = SynthCoreOrchestrator(
    memory_service=memory,
    identity_provider=identity,
    mood_engine=mood,
    token_budget=budget
)

prompt = await orchestrator.build_prompt(
    user_input="Explain the architecture.",
    session_id=session_id,
    user_id=user_id
)
```

---

## License

MIT License.
See `LICENSE` for details.

---

## Philosophy

SynthCore exists to answer one question:

**“How do we keep a powerful model coherent, bounded, and intentional when everything around it wants to grow unbounded?”**

This repository is one answer to that problem.
