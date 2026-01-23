# Ternary Delta

**Balanced ternary: what changes, what doesn’t, and why now.**

This repository is a delta-oriented reference. It does not argue that ternary replaces binary. It documents *exactly where balanced ternary alters semantics*, *where it does not*, and *why present-day constraints make experimentation rational rather than nostalgic*.

The goal is clarity, not advocacy.

---

## Executive Summary

Balanced ternary introduces a symmetric signed digit set {-1, 0, +1}. This single change has cascading effects in representation, rounding behavior, logic, and quantization. At the same time, most of modern computing remains structurally binary: memory addressing, operating systems, ABIs, filesystems, and network protocols are unchanged.

This repository separates:

* **Semantic deltas** (what mathematically changes)
* **Implementation choices** (how those deltas are encoded)
* **Compatibility constraints** (what must stay binary today)
* **Speculation** (what would require new hardware)

Claims are tagged as **Proven**, **Plausible**, or **Speculative**.

---

## The Delta Table (living index)

| Layer             | Binary Default            | Ternary Delta                  | Status      | Evidence               |
| ----------------- | ------------------------- | ------------------------------ | ----------- | ---------------------- |
| Integer math      | Unsigned/signed asymmetry | Natural symmetry around zero   | Proven      | proofs/representations |
| Rounding          | Bias toward +∞ or −∞      | Unbiased rounding at zero      | Proven      | proofs/rounding        |
| Logic             | Boolean AND/OR/XOR        | Ternary consensus/min/max      | Proven      | proofs/logic           |
| ML weights        | Continuous → binary quant | Natural 3-level discretization | Proven      | demos/quantization     |
| Memory addressing | Binary                    | No change                      | Proven      | docs/20-what-doesnt    |
| Floating point    | IEEE-754                  | No change                      | Proven      | docs/20-what-doesnt    |
| ISA               | Binary ops                | Ternary ops                    | Plausible   | demos/gcc-plugin       |
| Hardware          | Binary transistors        | Multi-state devices            | Speculative | references/papers      |

---

## What Changes (Semantic Deltas)

### 1. Symmetry Around Zero

Balanced ternary eliminates the signed/unsigned split. Zero is the center, not a boundary. This simplifies negation, subtraction, and error symmetry.

**Status:** Proven

### 2. Carry and Rounding Behavior

Balanced digit sets reduce systematic rounding bias. In iterative numerical processes, this can reduce drift.

**Status:** Proven

### 3. Logic Beyond Boolean

Ternary logic naturally supports *indeterminate* and *neutral* states. Operations such as min, max, and consensus become first-class rather than layered constructs.

**Status:** Proven

### 4. Quantization Effects

Three-level discretization aligns well with ML weight distributions. Compression emerges from representation, not heuristics.

**Status:** Proven

---

## What Does Not Change (Constraints)

### 1. Memory and Addressing

All practical systems today assume binary addressing. Ternary values live *within* binary-addressed memory.

### 2. Operating Systems and ABIs

Syscalls, calling conventions, and binaries remain binary. Ternary computation must interoperate, not replace.

### 3. Floating Point

IEEE-754 remains dominant. Ternary does not invalidate it and does not currently replace it.

### 4. Performance Myths

Ternary does **not** automatically improve speed or density. Benefits are domain-specific.

**Status:** Proven

---

## Why Now

### 1. Bandwidth Is the Bottleneck

Model size, memory traffic, and cache pressure dominate modern workloads. Representation efficiency matters again.

### 2. Quantization Is Mainstream

ML systems already accept reduced precision. Ternary provides a mathematically grounded option rather than an ad-hoc one.

### 3. Tooling Makes Experimentation Cheap

Compiler plugins, simulators, and open model formats allow exploration without custom silicon.

### 4. Energy and Edge Constraints

Lower precision and fewer transitions directly map to power savings in constrained environments.

**Status:** Plausible

---

## Failure Modes and Myths

* “Ternary is always denser.” False. Encoding overhead matters.
* “Ternary replaces binary.” False. It coexists.
* “Hardware is required for any benefit.” False for compression and simulation; true for peak throughput.

---

## Adoption Path

1. Simulation and libraries
2. Compiler lowering and IR experimentation
3. Domain-specific wins (ML, signal processing)
4. Optional hardware exploration

---

## Evidence Taxonomy

* **Proven:** reproducible math, code, or benchmarks
* **Plausible:** demonstrated in constrained contexts
* **Speculative:** requires new hardware or theory

---

## Philosophy

Binary is not wrong. It is contingent.

Balanced ternary is not destiny. It is possibility.

This repository exists to keep that possibility prec
