# A Responsible Adoption Path

Balanced ternary should not be adopted wholesale. It should be **contained, measured, and reversible**.

This document outlines a staged approach that minimizes risk while preserving learning value.

---

## Evidence tags

* **Proven**: viable today with existing systems
* **Plausible**: requires disciplined engineering
* **Speculative**: depends on future hardware or standards

---

## Stage 0: Clarify the question being asked

Before writing code, answer one question explicitly:

> *What property of balanced ternary are we testing?*

Examples:

* Compression ratio under fixed accuracy
* Bandwidth reduction in inference
* Error symmetry in discretization
* Logic expressivity for a specific domain

**Failure mode**

> Adopting ternary as an identity instead of a hypothesis.

**Status:** Proven necessity

---

## Stage 1: Library-level experimentation (Proven)

Begin at the **library level**, not the system level.

Characteristics:

* Pure user-space code
* No ABI changes
* Explicit serialization formats
* Clear canonicalization rules

Good fits:

* Numeric primitives
* Quantization utilities
* Representation experiments

**Exit criteria**

* Reproducible tests
* Measurable deltas
* Clear rollback path

---

## Stage 2: Representation-first wins (Proven)

Focus on cases where:

* Data movement dominates compute
* Arithmetic intensity is low
* Precision tolerance exists

Typical examples:

* Weight storage
* Sparse or delta-heavy data
* Model distribution and caching

**Guideline**

> Seek wins from *less data*, not *faster math*.

---

## Stage 3: Compiler-assisted lowering (Plausible)

Only after library-level success should compiler assistance be explored.

Scope:

* Intrinsics or builtins
* Explicit lowering passes
* Restricted semantic domains

Constraints:

* ABI boundaries remain binary
* Debuggability degrades
* Optimization guarantees weaken

**Warning**

> Compiler integration increases fragility faster than performance.

---

## Stage 4: Domain-specific pipelines (Plausible)

At this stage, ternary is treated as a **domain tool**, not a general-purpose representation.

Examples:

* ML inference pipelines
* Signal processing stages
* Control systems with neutral states

Characteristics:

* Narrow scope
* Heavy benchmarking
* Explicit fallbacks

---

## Stage 5: Architectural speculation (Speculative)

Only after semantic and pipeline-level wins are demonstrated should architectural exploration begin.

Examples:

* Ternary-native ISAs
* Multi-valued logic hardware
* Non-binary memory elements

**Rule**

> Architecture follows semantics, not the other way around.

---

## Guardrails for all stages

Across all stages:

* Canonicalization must be explicit
* Benchmarks must be end-to-end
* Binary fallbacks must exist
* Reversibility must be preserved

Balanced ternary should always be **optional**, never mandatory.

---

## Summary: how to explore without breaking things

Responsible ternary exploration:

* Starts small
* Measures aggressively
* Narrows scope
* Accepts failure
* Preserves exit paths

This is not a roadmap.
It is a containment strategy.

**Next:** `docs/60-glossary.md`
