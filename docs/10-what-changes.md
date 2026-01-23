# What Changes

This document describes **semantic deltas** introduced by **balanced ternary** (trits in {-1, 0, +1}). It avoids hardware claims and avoids assuming any particular storage encoding.

For implementation details (bit encodings, tryte layouts, packing), those belong elsewhere. Here we focus on what remains true even if the encoding changes.

---

## Evidence tags

* **Proven**: derivable from math or reproducible with reference code
* **Plausible**: demonstrated in constrained contexts; may not generalize
* **Speculative**: requires new hardware or unproven theory

---

## Assumptions and scope

All semantic deltas described in this document assume:

1. **Canonical balanced-ternary form**
   Values are normalized such that each digit is in {-1, 0, +1} and carries are fully resolved at defined normalization points.

2. **Well-defined normalization boundaries**
   Implementations may use lazy carries or alternative encodings internally, but must specify when canonicalization occurs (e.g., before comparison, serialization, or external observation).

3. **Binary-hosted systems**
   These semantics are discussed independently of storage encoding and assume execution within today’s binary-addressed memory and toolchains.

Violating these assumptions does not invalidate balanced ternary as a numeric system, but it *does* invalidate many of the guarantees described below.

---

## 1) Representation symmetry around zero

Balanced ternary uses digits that are symmetric about zero: -1, 0, +1.

This yields two immediate semantic properties:

1. **Negation is digit-wise**
   If a number is represented by trits `t_i`, then `-x` is represented by `(-t_i)` with no “sign bit” concept.

2. **Ranges are symmetric**
   For a fixed number of trits, the representable set is symmetric: if `x` is representable, so is `-x`.

**Why it matters**

* Signed arithmetic becomes the default; “unsigned as the primitive” is less natural.
* Error behavior tends to be symmetric; drift does not systematically prefer one direction.

**What this enables**

* Cleaner mental models for signed numerics and saturated or clamped schemes
* Symmetric quantization grids around 0 (see §4)

**What this does not**

* It does not automatically make arithmetic faster
* It does not remove the need for careful overflow policies

**Status:** Proven

---

## 2) Canonical digit set and non-uniqueness pitfalls

Balanced ternary has a canonical digit set, but **implementations must actively enforce it**. Redundant representations emerge immediately if carry rules or normalization boundaries are left implicit.

**Canonical form (conceptual)**
A number has a representation where each digit is in {-1, 0, +1} and carries are fully normalized.

**Why it matters**

* Canonicalization is a correctness boundary: comparisons, hashing, serialization, and deterministic behavior depend on it.
* Any performance optimization that permits “lazy” carries must define when normalization occurs.

**What this enables**

* Stable equality and ordering semantics
* Deterministic serialization and cross-system interchange

**What this does not**

* It does not prescribe a carry algorithm
* It does not require eager normalization—only well-defined normalization points

In practice, canonicalization is not an optimization detail; it is a semantic contract. Any system that weakens or defers this contract must explicitly state the consequences.

**Status:** Proven

---

## 3) Carry behavior and rounding bias

Balanced signed-digit systems change the *geometry* of discretization grids, but they do not eliminate the need for explicit rounding rules. Their primary semantic advantage is symmetry, not automatic numerical superiority.

In binary systems, common rounding modes combined with asymmetric signed representations can create subtle drift in iterative processes (depending on rounding and distribution). Balanced ternary’s symmetry helps, but it is not magic: the rounding rule still matters.

**A useful framing**

* Quantization introduces error `e = x - Q(x)`.
* In symmetric grids with symmetric rounding, `E[e]` tends toward 0 for symmetric input distributions.

Balanced ternary provides a symmetric grid around 0 **when canonical form is maintained and rounding rules preserve symmetry**.

**What this enables**

* Cleaner “unbiased around zero” discretization for certain distributions
* A more natural fit for ternary quantization in constrained domains

**What this does not**

* It does not guarantee reduced error for arbitrary distributions
* It does not replace numerical analysis; it changes the available tools

**Status:** Proven (symmetry property), Plausible (system-level drift reduction without measurement)

---

## 4) Ternary as a compression-oriented discretization

Many ML weight distributions are approximately centered near 0, making ternary discretization a *representation-aligned* option for aggressive compression.

Balanced ternary is not merely “three values”; it is a disciplined signed discretization with symmetry and a clear algebra.

**What changes**

* The discretization grid is symmetric and minimal: {-1, 0, +1} (possibly scaled).
* The “zero” state is first-class, not an afterthought.

**What this enables**

* Significant storage and bandwidth reduction in constrained pipelines
* Extremely simple dequantization paths
* Deterministic, inspectable quantization behavior

**What this does not**

* It does not imply universal accuracy retention
* It does not replace higher-bit quantization where fidelity dominates
* It does not make inference faster on binary hardware unless the pipeline is optimized

**Status:** Proven (definition and symmetry), Plausible (net benefits are model- and task-dependent)

---

## 5) Logic expands beyond Boolean defaults

This document makes no claim about programming language defaults; it describes logical *availability*, not mandatory adoption.

Boolean logic forces a two-valued worldview. Balanced ternary introduces a stable neutral value (0), which changes what operations are available as primitives.

This repo uses the following conceptual mapping:

* `+1` : true / asserted / positive
* `0`  : neutral / unknown / no-claim
* `-1` : false / denied / negative

There are multiple ternary logic systems. The point is not to select one, but to note what becomes available as *first-class operations*:

* **Min / Max semantics** (ordering-based logics)
* **Consensus / agreement** operators (where 0 acts as neutrality)
* **Conflict visibility** (opposite signs need not collapse)

**What this enables**

* Representing neutrality or indeterminacy without auxiliary flags
* Logic that can preserve conflict rather than forcing collapse

**What this does not**

* It does not automatically yield better program correctness
* It does not replace Boolean logic; it generalizes it

**Status:** Proven

---

## 6) Information density is a trade, not a slogan

A ternary digit carries `log2(3) ≈ 1.585` bits of information.

This is real—but it is not the end of the story:

* Encoding ternary in binary memory introduces packing overhead.
* CPU datapaths and vector units are binary today.
* Density wins appear only when packing, compute, and memory traffic align.

**What changes**

* The theoretical information-per-digit increases.

**What this enables**

* Potential storage and bandwidth wins if packed efficiently
* Different optimal word sizes and radix choices in specialized domains

**What this does not**

* It does not guarantee smaller files or faster compute in real systems
* It does not imply “ternary is 1.5× better” in general

Information density is a *necessary but insufficient* condition for real-world wins; only end-to-end measurements justify claims.

**Status:** Proven

---

## Summary: deltas you should actually rely on

Reliable deltas you can base designs on:

* symmetry around zero
* canonical digit set and explicit normalization contracts
* ternary as a disciplined 3-level discretization
* expanded logic expressivity
* theoretical information-per-digit increase (with real-world caveats)

Everything else belongs in benchmarks, demos, and failure-mode analysis.

**Next:** `docs/20-what-doesnt.md`

---

If you’re ready, the next file (`20-what-doesnt.md`) should be deliberately blunt and constraint-heavy. That document will *lock in* the credibility this one earns.
