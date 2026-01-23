# Hardware Notes (Speculative)

This document collects **speculative considerations** about hardware support for balanced ternary.

It does **not** claim feasibility, superiority, or inevitability.
It exists solely to prevent repeated misunderstandings about where hardware enters the picture.

Everything in this document is **Speculative** unless stated otherwise.

---

## Evidence tag

**Status:** Speculative

---

## Scope and intent

This document exists to answer one question:

> *If balanced ternary semantics were taken seriously, what hardware directions would even be worth thinking about?*

It does **not** attempt to:
- Propose a concrete CPU design
- Claim near-term feasibility
- Compete with binary hardware
- Predict industry direction

Hardware follows semantics, not the reverse.

---

## What this document is *not*

This document is not:
- A roadmap
- A performance claim
- A call to action
- A replacement argument
- A proof of advantage

All semantic claims live elsewhere in this repository.

---

## 1) Binary hardware assumptions today

Modern hardware is deeply binary at multiple levels:

- Two-state transistors
- Binary voltage thresholds
- Binary memory cells
- Binary instruction encodings
- Binary condition codes

As a result:
- Any ternary computation today is **simulated or encoded**
- Hardware-level advantages of ternary do not automatically apply

This constraint is **non-negotiable** in the present.

---

## 2) Where ternary hardware speculation usually goes wrong

Common errors in ternary hardware discussion:

- Assuming theoretical information density maps directly to performance
- Ignoring noise margins and error rates
- Treating multi-valued logic as a drop-in replacement
- Underestimating tooling and fabrication inertia

Most historical ternary proposals fail at one of these points.

---

## 3) Plausible hardware *directions* (not proposals)

The following are **conceptual directions**, not designs.

### 3.1 Multi-level logic elements
Devices with more than two stable states could, in theory:
- Reduce interconnect count
- Alter switching characteristics
- Change energy tradeoffs

Open questions:
- Stability under noise
- Manufacturability at scale
- Toolchain compatibility

---

### 3.2 Hybrid binary–ternary units
Rather than fully ternary systems, limited ternary units could exist alongside binary logic:

- Ternary ALUs for narrow domains
- Ternary accumulators or reducers
- Domain-specific accelerators

This mirrors how GPUs, TPUs, and NPUs coexist with CPUs.

---

### 3.3 Memory-adjacent ternary representation
Speculation sometimes focuses on:
- Storage density
- Reduced bandwidth
- Fewer memory transactions

In practice:
- Encoding and error correction dominate
- Binary memory models still apply

Any gains here would be **highly workload-specific**.

---

## 4) Why hardware speculation is intentionally deferred

This repository deliberately defers hardware discussion because:

- Semantic deltas must stand on their own
- Software-level evidence is cheaper and safer
- Hardware assumptions are the most fragile layer

Premature hardware focus has historically weakened alternative numeric systems.

---

## 5) Relationship to the adoption path

Hardware speculation corresponds to **Stage 5** in `docs/50-adoption-path.md`.

Reaching that stage requires:
- Demonstrated semantic value
- Reproducible system-level wins
- Clear failure boundaries

Without those, hardware discussion is noise.

---

## 6) How to read speculative claims safely

When encountering ternary hardware claims elsewhere, ask:

1. What semantic delta is being exploited?
2. Is the benefit independent of encoding?
3. What assumptions are made about noise and error?
4. What tooling and fabrication changes are required?
5. Is a binary fallback preserved?

If these questions are unanswered, the claim is incomplete.

---

## Summary

Balanced ternary does not require new hardware to be meaningful.  
New hardware would only matter **after** semantics and systems justify it.

Until then, hardware discussion remains speculative by design.

---

## Links

- Adoption discipline: `docs/50-adoption-path.md`
- Constraints that remain: `docs/20-what-doesnt.md`
- Failure modes: `docs/40-failure-modes.md`

---
