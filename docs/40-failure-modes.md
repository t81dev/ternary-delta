# Failure Modes and Limits

Balanced ternary introduces real semantic advantages in some contexts. It also introduces **new costs, risks, and failure modes**.

This document enumerates where balanced ternary underperforms, complicates systems, or fails outright.

---

## Evidence tags

* **Proven**: observed in practice or derivable from system constraints
* **Plausible**: likely given current architectures
* **Speculative**: dependent on future designs

---

## 1) Encoding overhead can erase theoretical gains

Although a ternary digit carries more information than a binary digit, **encoding ternary inside binary memory can negate or reverse density gains**.

Common pitfalls:

* Padding to byte or word boundaries
* Inefficient packing schemes
* Alignment constraints that waste space

**Failure mode**

> A theoretically denser representation consumes *more* memory once encoded.

**Mitigation**

* Measure end-to-end storage, not abstract digit counts
* Treat packing efficiency as a first-class design concern

**Status:** Proven

---

## 2) Canonicalization costs can dominate runtime

Maintaining canonical form is non-negotiable for correctness, but it is not free.

Costs include:

* Carry propagation
* Normalization passes
* Branching on digit values

**Failure mode**

> Normalization overhead outweighs any semantic advantage.

This is especially acute in:

* Tight loops
* High-throughput numeric kernels
* Poorly batched workloads

**Mitigation**

* Define explicit normalization boundaries
* Amortize canonicalization where possible

**Status:** Proven

---

## 3) Vectorization is difficult

Modern performance relies heavily on SIMD and vector units that assume:

* Fixed-width binary lanes
* Boolean masks
* Two-valued conditionals

Ternary representations often resist efficient vectorization.

**Failure mode**

> Ternary code scalarizes where binary code vectorizes.

**Mitigation**

* Accept scalar execution in bandwidth-dominated paths
* Restrict ternary use to representation-heavy stages

**Status:** Proven

---

## 4) Toolchain friction is high

Compilers, debuggers, profilers, and sanitizers assume binary semantics.

Common issues:

* Poor optimization visibility
* Confusing debug output
* Misleading performance counters

**Failure mode**

> Tooling obscures behavior rather than clarifying it.

**Mitigation**

* Treat ternary tooling as experimental
* Rely on explicit tests and benchmarks

**Status:** Proven

---

## 5) Mental overhead is real

Balanced ternary requires developers to reason about:

* Non-Boolean logic
* Canonicalization rules
* Signed digit behavior

**Failure mode**

> Cognitive cost outweighs technical benefit.

This is especially problematic in:

* Large teams
* Safety-critical code
* Maintenance-heavy systems

**Mitigation**

* Constrain ternary use to narrow, well-documented modules
* Hide ternary internals behind stable APIs

**Status:** Proven

---

## 6) Performance cliffs are common

Small design choices can cause large regressions:

* Slightly different packing
* Slightly different normalization timing
* Different branching patterns

**Failure mode**

> Minor changes cause disproportionate slowdowns.

**Mitigation**

* Profile continuously
* Treat performance regressions as correctness failures

**Status:** Proven

---

## 7) Accuracy losses can be unacceptable

In quantized systems:

* Some models tolerate ternary discretization
* Others fail catastrophically

**Failure mode**

> Compression gains destroy task performance.

**Mitigation**

* Treat ternary quantization as opt-in
* Validate per-model and per-task

**Status:** Proven

---

## 8) Hardware assumptions do not transfer

Many theoretical advantages of ternary assume:

* Multi-valued logic elements
* Different switching costs
* Non-binary noise models

On binary hardware, these assumptions do not hold.

**Failure mode**

> Expected gains never materialize.

**Mitigation**

* Separate semantic exploration from hardware speculation
* Demand measurements, not intuition

**Status:** Proven

---

## Summary: when ternary is a bad idea

Balanced ternary is often a poor choice when:

* Peak throughput matters more than bandwidth
* SIMD utilization is critical
* Tooling maturity is required
* Cognitive simplicity is paramount
* Accuracy cannot be traded for compression

Failure to acknowledge these limits turns exploration into advocacy.

**Next:** `docs/50-adoption-path.md`
