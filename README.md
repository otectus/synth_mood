# SynthMood

![Version](https://img.shields.io/badge/version-0.1.0-blue)
![Python](https://img.shields.io/badge/python-3.10%2B-brightgreen)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

**SynthMood** is the affect and behavioral-bias subsystem of the Nexus Client, providing a bounded, decaying PAD (Valence, Arousal, Dominance) signal used to guide model strategy without simulating emotions.

---

## Overview

SynthMood is **not** an emotion engine and **not** a role-play system.

It exists to supply **lightweight, mathematically stable behavioral metadata** that helps the orchestration layer adjust *how* a model responds, not *what it pretends to feel*.

SynthMood produces:
- A continuous affect signal
- With predictable decay over time
- That converges to a neutral baseline
- And injects clear, machine-usable strategy hints into the prompt pipeline

---

## Core Responsibilities

SynthMood provides the Nexus Client with:

- **Affect Modeling**
  - Uses the PAD model (Valence, Arousal, Dominance)
  - Each dimension bounded in `[-1.0, 1.0]`
- **Time-Based Decay**
  - Exponential half-life decay toward baseline
  - Prevents emotional accumulation or drift
- **Behavioral Bias Injection**
  - Translates PAD state into concise strategy guidance
  - Influences tone, assertiveness, and caution
- **Safety Guarantees**
  - No emotional role-play
  - No fabricated feelings
  - No anthropomorphic claims

SynthMood is deliberately conservative by design.

---

## What SynthMood Is *Not*

SynthMood explicitly avoids:

- ❌ Emotional simulation
- ❌ Personality generation
- ❌ Sentiment analysis
- ❌ User mood inference
- ❌ Memory storage
- ❌ Long-term affect persistence

It is a **control signal**, not a character system.

---

## PAD Model Explained

SynthMood uses the classic **PAD model**:

- **Valence**  
  Negative ↔ Positive bias  
  (e.g., conservative vs optimistic approaches)

- **Arousal**  
  Calm ↔ Energetic bias  
  (e.g., thorough vs concise responses)

- **Dominance**  
  Deferential ↔ Directive bias  
  (e.g., ask questions vs assert recommendations)

All values are:
- Continuous
- Bounded
- Decayed over time
- Anchored to a defined baseline

---

## Decay Mechanics

Mood decay is calculated using exponential half-life decay with inertia:

```

new_state = baseline + (last_state - baseline) * inertia * decay_factor

````

Where:
- `decay_factor = exp(-ln(2) * t / half_life)`
- `inertia` controls persistence
- `half_life` controls decay speed

This guarantees:
- Stability
- Predictable convergence
- No runaway states
- No permanent mood locking

---

## Baseline State

By default, SynthMood converges to:

- Valence: `0.0` (neutral)
- Arousal: `0.0` (neutral)
- Dominance: `0.5` (slightly in-control)

This reflects a calm, professional, mildly assertive assistant as the neutral operating state.

---

## Prompt Injection

SynthMood injects **internal-only context**, clearly labeled:

```text
## Dimensional Mood State (internal context, not for role-play):
- Valence: +0.10
- Arousal: -0.25
- Dominance: +0.60
Behavioral implication: be patient and thorough; take lead on architecture.
````

The injection always includes a guard clause:

> This mood signal is metadata about task approach, NOT an instruction to simulate emotions.

This prevents emotional hallucination while still enabling strategic bias.

---

## Integration Role

Within the Nexus Client, SynthMood is:

* Queried by **SynthCore**
* Applied before memory injection
* Treated as *advisory*, not authoritative
* Overridden by explicit user instructions when conflicts arise

SynthMood never overrides user intent.

---

## Architecture Position

```
Time / Events
   │
   ▼
SynthMood (Decay Engine)
   │
   ▼
Behavioral Bias Metadata
   │
   ▼
SynthCore Prompt Assembly
```

SynthMood has **no direct contact with the LLM**.

---

## Phase Status

### Phase 1 (Current)

* PAD state representation
* Exponential decay with inertia
* Configurable dynamics
* Safe prompt injection
* No learning or persistence

### Phase 2 (Planned)

* Event-driven mood adjustments
* Mode-specific decay profiles
* Optional persistence hooks
* Telemetry and tuning tools

---

## Requirements

* Python **3.10+**
* Standard library only
* Designed for orchestration-layer integration

---

## Design Philosophy

SynthMood follows three rules:

1. **Bias behavior, don’t invent feelings**
2. **Decay everything**
3. **Never surprise the operator**

It exists to make responses *more appropriate*, not *more emotional*.

---

## License

MIT License.
See `LICENSE` for details.
