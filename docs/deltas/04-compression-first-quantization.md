# Delta 04: Compression-First Quantization

Balanced ternary provides a minimal signed discretization set: **{-1, 0, +1}**.  
This delta concerns **representation and compression**, not learning theory or universal accuracy gains.

The central claim is narrow:

> Balanced ternary offers a *representation-aligned option* for aggressive compression in domains where bandwidth, storage, or simplicity dominate.

---

## Evidence tag

**Status:** Proven (representation properties), Plausible (end-to-end system benefit)

---

## Assumptions

This delta assumes:
- **Canonical balanced-ternary form**
- **Explicit scaling rules** (ternary values may be scaled to match magnitude ranges)
- **Binary-hosted execution environments**
- **Compression-tolerant domains**

No claim is made that ternary quantization is appropriate for all models or tasks.

---

## Statement of the delta

### 1) A minimal signed discretization set

Balanced ternary discretizes values into three states:

- `-1` : negative
- `0`  : zero / inactive
- `+1` : positive

This is the smallest signed discretization that preserves:
- Sign information
- A true zero state
- Symmetry around zero

---

### 2) Alignment with zero-centered distributions

Many real-world signals cluster near zero:
- Neural network weights
- Residuals and deltas
- Sparse activations
- Control signals

Balanced ternary treats zero as a **first-class value**, not a boundary artifact or sentinel.

This alignment reduces representational friction when aggressively compressing such signals.

---

### 3) Compression precedes performance

On binary hardware, ternary arithmetic is rarely faster.

The primary wins occur earlier in the pipeline:
- Smaller serialized models
- Reduced memory traffic
- Lower cache pressure
- Simpler dequantization paths

Any performance benefit is indirect and workload-dependent.

---

## Why it matters

### Bandwidth and storage dominate many workloads

In modern systems:
- Memory movement is often more expensive than arithmetic
- Model distribution and loading costs matter
- Edge and constrained environments are bandwidth-limited

Balanced ternary offers a disciplined way to trade precision for **representational economy**.

---

## What this enables

- Aggressive weight-only compression
- Deterministic, inspectable quantization behavior
- Simple scaling and dequantization logic
- Compatibility with binary runtimes when carefully integrated

---

## What this does not enable

- It does not guarantee accuracy retention
- It does not outperform higher-bit quantization universally
- It does not accelerate inference by default
- It does not eliminate the need for validation

Compression-first does not mean correctness-agnostic.

---

## Common misinterpretations

- **“Ternary quantization is better than INT4 or INT8.”**  
  False. It occupies a different point in the trade space.

- **“Three levels are enough for all models.”**  
  False. Model sensitivity varies widely.

- **“Compression implies speed.”**  
  False. Compression affects bandwidth; speed depends on the full pipeline.

---

## Failure modes

- Accuracy collapse on sensitive models
- Encoding overhead outweighing storage savings
- Dequantization overhead dominating runtime
- Toolchain incompatibilities

These are documented in `docs/40-failure-modes.md`.

---

## Links

- Signed symmetry: `docs/deltas/01-signed-symmetry.md`
- Rounding and drift: `docs/deltas/02-rounding-and-drift.md`
- Constraints: `docs/20-what-doesnt.md`
- Failure analysis: `docs/40-failure-modes.md`

---
