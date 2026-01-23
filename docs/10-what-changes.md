# What Changes

This document describes **semantic deltas** introduced by **balanced ternary** (trits in {-1, 0, +1}). It avoids hardware claims and avoids assuming any particular storage encoding.

For implementation details (bit encodings, tryte layouts, packing), those belong elsewhere. Here we focus on what is true even if the encoding changes.

---

## Evidence tags

- **Proven**: derivable from math or reproducible with reference code
- **Plausible**: demonstrated in constrained contexts; may not generalize
- **Speculative**: requires new hardware or unproven theory

---

## 1) Representation symmetry around zero

Balanced ternary uses digits that are symmetric about zero: -1, 0, +1.

This yields two immediate semantic properties:

1. **Negation is digit-wise**  
   If a number is represented by trits `t_i`, then `-x` is represented by `(-t_i)` with no “sign bit” concept.
2. **Ranges are symmetric**  
   For a fixed number of trits, the representable set is symmetric: if `x` is representable, so is `-x`.

**Why it matters**
- Signed arithmetic becomes the default; “unsigned as the primitive” is less natural.
- Error behavior tends to be symmetric; drift does not systematically prefer one direction.

**What this enables**
- Cleaner mental model for signed numerics and saturated/clamped schemes
- Symmetric quantization grids around 0 (see §4)

**What this does not**
- It does not automatically make arithmetic faster
- It does not remove the need for careful overflow policies

**Status:** Proven

---

## 2) Canonical digit set and non-uniqueness pitfalls

Balanced ternary has a canonical digit set, but *implementations can accidentally introduce non-uniqueness* if they allow redundant forms (e.g., mixed-radix carry conventions).

**Canonical form (conceptual)**
A number has a representation where each digit is in {-1, 0, +1} and carries are fully normalized.

**Why it matters**
- Canonicalization is a correctness boundary: comparisons, hashing, serialization, and deterministic behavior depend on it.
- Any performance optimization that permits “lazy” carries must define when normalization occurs.

**What this enables**
- Stable equality and ordering semantics
- Deterministic serialization and cross-system interchange

**What this does not**
- It does not prescribe a carry algorithm
- It does not require eager normalization—only well-defined normalization points

**Status:** Proven

---

## 3) Carry behavior and rounding bias

Balanced signed-digit systems can reduce *systematic rounding bias* when mapping continuous values to discrete grids, particularly around 0.

In binary, common rounding modes combined with asymmetric signed representations can create subtle drift in iterative processes (depending on rounding and distribution). Balanced ternary’s symmetry helps, but is not magic: the rounding rule still matters.

**A useful framing**
- Quantization introduces error `e = x - Q(x)`.
- In symmetric grids with symmetric rounding, `E[e]` tends toward 0 for symmetric input distributions.

Balanced ternary naturally provides a symmetric grid around 0.

**What this enables**
- Cleaner “unbiased around zero” discretization for certain distributions
- A more natural fit for ternary quantization in ML

**What this does not**
- It does not guarantee reduced error for arbitrary distributions
- It does not replace numerical analysis; it changes the tools

**Status:** Proven (as symmetry property), Plausible (as a system-level drift claim without measurement)

---

## 4) Ternary as a native discretization for ML weights

Many ML weight distributions are approximately centered near 0. A 3-level quantizer (negative, zero, positive) can align well with such distributions.

Balanced ternary is not merely “three values”; it is a disciplined signed discretization with symmetry and a clear algebra.

**What changes**
- The discretization grid is symmetric and minimal: {-1, 0, +1} (possibly scaled).
- The “zero” state is first-class, not an afterthought.

**What this enables**
- Compression and bandwidth reduction in weight storage
- Potential regularization-like effects depending on training/finetuning regime
- Extremely simple dequantization paths

**What this does not**
- It does not guarantee accuracy retention
- It does not make inference faster on binary hardware unless the pipeline is optimized

**Status:** Proven (definition and symmetry), Plausible (net accuracy/throughput benefits depend on model/task)

---

## 5) Logic expands beyond Boolean defaults

Boolean logic forces a two-valued worldview. Balanced ternary introduces a stable neutral value (0), which changes the “default” meanings of operations.

This repo uses the following conceptual mapping:

- `+1` : true / asserted / positive
- `0`  : neutral / unknown / no-claim
- `-1` : false / denied / negative

There are multiple ternary logic systems. The point here is not to pick one, but to note what becomes available *as first-class operations*:

- **Min/Max semantics** (ordering-based logics)
- **Consensus / agreement** operators (where 0 acts as neutrality)
- **Conflict visibility** (opposite signs do not need to collapse to a single bit)

**What this enables**
- Representing “unknown/neutral” without additional flags
- Logic that can preserve conflict or neutrality rather than forcing collapse

**What this does not**
- It does not automatically yield better program correctness
- It does not replace Boolean logic; it generalizes it

**Status:** Proven

---

## 6) Information density is a trade, not a slogan

A ternary digit carries `log2(3) ≈ 1.585` bits of information.

This is real—but it is not the end of the story:
- Encoding ternary in binary memory introduces packing overhead.
- CPU datapaths and vector units are binary today.
- Density wins only appear when packing + compute + memory traffic align.

**What changes**
- The theoretical information-per-digit increases.

**What this enables**
- Potential storage and bandwidth wins if packed efficiently
- Different optimal word sizes and radix choices in specialized domains

**What this does not**
- It does not guarantee smaller files or faster compute in real systems
- It does not imply “ternary is 1.5× better” in general

**Status:** Proven

---

## Summary: deltas you should actually rely on

Reliable deltas you can base designs on:
- symmetry around zero
- canonical digit set and normalization requirements
- ternary as a disciplined 3-level discretization
- expanded logic expressivity
- theoretical information-per-digit increase (with real-world caveats)

Everything else belongs in benchmarks, demos, and “failure modes.”

Next: `docs/20-what-doesnt.md`
