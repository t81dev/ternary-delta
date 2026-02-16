# Delta 01: Signed Symmetry Around Zero

Balanced ternary uses trits in **{-1, 0, +1}**. This digit set is symmetric about zero. The symmetry is not an implementation detail; it is a semantic property of the number system.

This delta matters because many binary systems treat “signedness” as an overlay (sign bit, two’s complement, unsigned defaults). Balanced ternary makes signedness intrinsic.

---

## Evidence tag

**Status:** Proven

---

## Assumptions

This delta assumes:
- **Canonical balanced-ternary form** (digits in {-1, 0, +1})
- **Defined normalization boundaries** (canonicalization occurs at externally visible points)
- Execution within **binary-hosted systems** (binary memory/ABI/OS)

---

## Statement of the delta

### 1) Digit-wise negation is natural
If a value `x` is represented by trits `t_i`, then:

- `-x` is represented by trits `(-t_i)`.

There is no separate “sign bit” concept required at the semantic level.

### 2) Representable ranges are symmetric
For a fixed number of trits, the set of representable values is symmetric:

- If `x` is representable, `-x` is representable.

This holds for any fixed-trit-width representation in canonical form.

---

## Why it matters

### Fewer asymmetric edge cases
Binary representations often carry asymmetries:
- differing signed vs unsigned behaviors
- representation artifacts near min/max
- special casing around sign transitions

Balanced ternary’s symmetry shifts complexity away from “signedness management” and toward explicit overflow policies.

### Error symmetry becomes the default
When discretizing around zero, a symmetric digit set supports symmetric error behavior (assuming symmetric rounding rules). This does not eliminate error, but it removes a structural bias toward one sign.

---

## What this enables

- A clean mental model for signed numerics as the default
- Symmetric discretization grids centered on zero
- Straightforward representations of “positive/neutral/negative” in domains where sign is semantic
- Natural complementarity with ternary quantization schemes that preserve sign symmetry

---

## What this does not enable

- It does not imply faster arithmetic on binary hardware
- It does not eliminate the need for explicit overflow and saturation rules
- It does not guarantee reduced numerical error for arbitrary distributions or rounding rules
- It does not remove the need for canonicalization (non-canonical forms reintroduce ambiguity)

---

## Links

- Definition context: `docs/10-what-changes.md` (§1)
- Constraints that remain: `docs/20-what-doesnt.md`
- Failure modes (canonicalization cost, cliffs): `docs/40-failure-modes.md`

---
