# Related Work and Evidence Probes

This repository defines **semantic deltas** and **constraints** for balanced ternary.
It does not implement a stack.

The projects referenced here are **evidence probes**—each explores whether a specific claim survives contact with reality under modern constraints.

They are not prerequisites, roadmaps, or endorsements.

---

## Framing principle

> **This repository defines the contract.
> Other repositories test whether that contract holds under pressure.**

Each project below answers *one narrow question*. None is presented as complete or production-ready.

---

## Project group: Numeric semantics and invariants

### t81lib

Maintained under **t81dev**.

**Question explored**
Can balanced ternary numeric primitives be made **precise, deterministic, canonical, and testable** inside a modern language ecosystem?

**What it probes**

* Canonicalization rules
* Signed-digit arithmetic behavior
* Deterministic serialization
* Interoperability with binary-hosted systems

**What it does not claim**

* Hardware acceleration
* Universal performance improvement
* Replacement of binary numerics

**Relevance to this repo**
Supports claims in:

* `10-what-changes.md` (§1–§3)
* `40-failure-modes.md` (§2, §6)

---

## Project group: Toolchain and ABI boundaries

### ternary GCC plugin (and related lowering experiments)

**Question explored**
How far can ternary semantics be lowered through an existing **binary C/C++ toolchain** without violating ABI contracts?

**What it probes**

* Compiler IR assumptions
* ABI boundary friction
* Debuggability and optimization loss
* Helper-runtime requirements

**What it does not claim**

* Full language integration
* Stable optimization guarantees
* Production readiness

**Relevance to this repo**
Supports constraints in:

* `20-what-doesnt.md` (§3, §7)
* `40-failure-modes.md` (§4)

---

## Project group: Representation and ML pipelines

### Ternary quantization / GGUF-related tooling

**Question explored**
Can ternary discretization deliver **compression-first wins** in real ML pipelines without breaking compatibility?

**What it probes**

* Weight-only quantization
* Bandwidth and storage effects
* End-to-end model loading and inference
* Compatibility with existing runtimes

**What it does not claim**

* Universal accuracy retention
* Training superiority
* Automatic speedups on binary hardware

**Relevance to this repo**
Supports:

* `10-what-changes.md` (§4)
* `30-why-now.md` (§1–§3)
* `40-failure-modes.md` (§7)

---

## Project group: Architectural speculation

### t81-foundation and related ISA / OS sketches

**Question explored**
If balanced ternary semantics are fixed, what **architectural directions** become imaginable?

**What it probes**

* Long-horizon ISA design
* Ternary-native abstractions
* Conceptual OS layering

**What it does not claim**

* Near-term feasibility
* Superiority over binary hardware
* Direct applicability today

**Relevance to this repo**
Explicitly fenced as:

* **Speculative** (see `50-adoption-path.md`, Stage 5)

---

## What is deliberately excluded

This repository does **not** catalog:

* Historical ternary machines
* Ideological arguments for non-binary computing
* General-purpose “ternary CPUs”
* Performance claims without measurement

Those topics are orthogonal to the goal here.

---

## How to read these projects together

* This repository answers **“what changes, what doesn’t, and why now.”**
* Related projects answer **“does this claim survive implementation?”**
* Failure in an evidence probe does not falsify the semantics—it falsifies an assumption.

That distinction matters.

---

## Closing note

Balanced ternary should be evaluated as a **semantic hypothesis**, not a movement.

Evidence probes are how hypotheses earn or lose credibility.

This repository exists to keep that evaluation precise.
