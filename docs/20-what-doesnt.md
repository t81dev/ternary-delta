## File 2: `docs/20-what-doesnt.md`

Below is a **complete, ready-to-commit draft**.
This document is intentionally blunt. Its job is to prevent category errors and protect the credibility of the project.

---

# What Does Not Change

This document describes **constraints and invariants** that remain in force when experimenting with balanced ternary today.

Balanced ternary introduces real semantic deltas. It does **not** dissolve the structural realities of modern computing. Any serious work must respect this boundary.

---

## Evidence tags

* **Proven**: enforced by existing systems and specifications
* **Plausible**: breakable only with substantial redesign
* **Speculative**: would require new hardware or OS models

---

## 1) Memory addressing remains binary

All mainstream systems assume **binary-addressed memory**.

* Pointers index bytes, not trits.
* Cache lines, pages, MMUs, and virtual memory are binary constructs.
* Alignment, aliasing rules, and memory models are defined in terms of bytes.

Balanced ternary values therefore exist *within* binary-addressed memory, regardless of their internal representation.

**What this means**

* Ternary representations must be packed into bytes or words.
* Any addressing scheme that treats trits as addressable units is hypothetical.

**What this does not change**

* Pointer arithmetic
* Cache behavior
* Memory consistency models

**Status:** Proven

---

## 2) Operating systems do not become ternary-aware

Operating systems expose binary contracts:

* Syscalls
* File descriptors
* Page tables
* Signals and interrupts
* Process and thread models

Balanced ternary computation runs *on top of* these contracts, not instead of them.

**What this means**

* A “ternary OS” is not implied by ternary arithmetic.
* User-space ternary computation must interoperate with binary kernels.

**What this does not change**

* Process models
* Scheduling semantics
* I/O interfaces

**Status:** Proven

---

## 3) ABIs and calling conventions remain fixed

Application Binary Interfaces define:

* Argument passing
* Register usage
* Stack layout
* Return value conventions

These are binary and architecture-specific. Introducing ternary semantics does not alter them.

**What this means**

* Ternary values must be lowered to ABI-compatible forms at boundaries.
* Any ternary-native calling convention would be non-interoperable.

**What this does not change**

* C/C++ interoperability rules
* Foreign-function interfaces
* Toolchain expectations

**Status:** Proven

---

## 4) Floating-point standards remain IEEE-754

Balanced ternary does not replace or subsume IEEE floating-point.

* IEEE-754 defines representation, rounding, exceptions, and NaNs.
* Hardware FPUs, compilers, and libraries assume this model.

**What this means**

* Ternary work today primarily concerns integers, logic, and quantization.
* “Ternary floating point” is a separate research topic.

**What this does not change**

* Numerical analysis expectations
* FP performance characteristics
* Language-level floating-point semantics

**Status:** Proven

---

## 5) Performance is not automatically improved

Balanced ternary does not inherently make computation faster.

On existing hardware:

* Arithmetic units are binary.
* SIMD and vector units are binary.
* Branch predictors, pipelines, and caches are binary-optimized.

**What this means**

* Many ternary operations lower to more instructions, not fewer.
* Performance wins must come from *reduced bandwidth*, *simpler control*, or *domain alignment*.

**What this does not change**

* Peak FLOPs
* Instruction throughput
* Latency guarantees

**Status:** Proven

---

## 6) Information density alone does not guarantee wins

Although a ternary digit carries more theoretical information than a binary digit, real systems pay costs for:

* Packing and unpacking
* Alignment and padding
* Vectorization barriers

**What this means**

* Density advantages appear only when end-to-end effects dominate overhead.
* Microbenchmarks can be misleading.

**What this does not change**

* The need for profiling
* The need for system-level benchmarks

**Status:** Proven

---

## 7) Compiler support is experimental, not magical

Compilers are deeply binary-biased:

* IRs assume binary bitwidths
* Optimization passes assume two-valued logic
* Backends target binary ISAs

**What this means**

* Ternary compiler work is exploratory and fragile.
* Plugins and custom lowering can demonstrate feasibility, not completeness.

**What this does not change**

* Language standards
* Optimization guarantees
* Debugging assumptions

**Status:** Proven

---

## 8) Ternary does not eliminate failure modes

Balanced ternary introduces *new* failure modes alongside its benefits:

* Non-canonical representations
* Ambiguous normalization boundaries
* Encoding drift across components
* Hidden performance cliffs

**What this means**

* Strong invariants matter more, not less.
* Testing and specification discipline become critical.

**What this does not change**

* The need for validation
* The need for explicit contracts

**Status:** Proven

---

## Summary: constraints you must design around

Stable constraints that do not disappear:

* Binary-addressed memory
* Binary OS and ABI contracts
* IEEE floating point dominance
* Binary hardware datapaths
* Toolchain bias toward two-valued logic

Balanced ternary work that ignores these constraints becomes speculative by default.

**Next:** `docs/30-why-now.md`
