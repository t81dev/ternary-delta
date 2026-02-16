# Glossary

---

## Balanced ternary

A radix-3 numeral system using the digit set **{-1, 0, +1}**. Unlike unbalanced ternary (0,1,2), balanced ternary is symmetric around zero and supports digit-wise negation.

---

## Trit

A single balanced ternary digit with value **-1**, **0**, or **+1**.
Analogous to a bit in binary systems.

---

## Tryte

An informal grouping of multiple trits into a convenient unit for storage or computation.
The size and encoding of a tryte are implementation details, not semantic requirements.

---

## Canonical form

A representation in which:

* Every digit is in {-1, 0, +1}
* All carries are fully resolved
* No redundant or alternate representations exist

Canonical form is a **semantic contract**, not an optimization choice.

---

## Canonicalization

The process of transforming a representation into canonical form.
Canonicalization may be eager or lazy, but **normalization boundaries must be explicit**.

---

## Normalization boundary

A defined point at which canonicalization is guaranteed to have occurred (e.g., before comparison, serialization, hashing, or external observation).

---

## Signed-digit representation

A numeric representation in which digits may take both positive and negative values. Balanced ternary is a signed-digit system.

---

## Semantic delta

A change in *meaning* or *behavior* that arises from a different numeric system, independent of storage encoding or hardware implementation.

---

## Encoding

A concrete method for storing ternary values in binary memory (e.g., bit packing, byte alignment).
Encodings do **not** define semantics.

---

## Binary-hosted system

A system in which memory, addressing, OS, ABI, and hardware datapaths are fundamentally binary, even if higher-level representations are not.

---

## Quantization

The process of mapping continuous or high-precision values to a discrete set of representable values.

---

## Ternary quantization

Quantization using the value set {-1, 0, +1} (optionally scaled). Emphasizes symmetry and minimality rather than fidelity.

---

## Compression-first discretization

An approach that prioritizes storage size, bandwidth, and simplicity over maximal numerical accuracy.

---

## Rounding rule

A defined method for mapping values between continuous and discrete representations. Symmetry alone does not determine rounding behavior.

---

## Symmetric grid

A discretization grid centered around zero such that for every representable value `x`, `-x` is also representable.

---

## Logic availability

The set of logical operations that a numeric system naturally supports (e.g., consensus, neutrality, ordering), independent of programming language defaults.

---

## Boolean collapse

The forced reduction of multi-valued logic into two values (true/false), often discarding neutrality or conflict information.

---

## Proven

A claim supported by mathematics, reproducible code, or direct measurement.

---

## Plausible

A claim supported by partial evidence or constrained demonstrations, but not universally validated.

---

## Speculative

A claim that depends on future hardware, new standards, or unproven assumptions.

---

## Failure mode

A predictable way in which a system underperforms, degrades, or violates expectations when assumptions are violated.

---

## Reversibility

The property that an experiment or adoption can be undone without systemic damage or lock-in.

---

## Semantic hypothesis

The framing of an alternative numeric system as a testable idea rather than a belief or identity.

---

## Closing note

If a term is not defined here, it should not be relied upon for argument.

Terminology discipline is part of correctness.
