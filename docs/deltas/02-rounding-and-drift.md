# Delta 02: Rounding Symmetry and Drift Behavior

Balanced ternary changes the *geometry* of discretization grids by centering representable values symmetrically around zero. This alters how rounding error behaves, but only under explicit assumptions.

This delta is frequently overstated. This document states precisely what changes, and what does not.

---

## Evidence tag

**Status:** Proven (grid symmetry), Plausible (system-level drift reduction)

---

## Assumptions

This delta assumes:
- **Canonical balanced-ternary form** (digits in {-1, 0, +1})
- **Explicit rounding rules** (symmetry is not automatic)
- **Defined normalization boundaries**
- Execution within **binary-hosted systems**

If rounding rules are asymmetric, the symmetry advantages described here do not apply.

---

## Statement of the delta

### 1) Discretization grids are symmetric about zero

Balanced ternary provides a minimal signed grid:

```

..., -2, -1, 0, +1, +2, ...

```

where the underlying digit system ensures that:
- For every representable value `x`, `-x` is also representable.
- Zero is a stable, central element, not a boundary case.

This contrasts with many binary discretizations where representation or rounding asymmetries appear near zero or extrema.

---

### 2) Rounding error can be unbiased *if rules preserve symmetry*

Let:
- `Q(x)` be a quantization function
- `e = x - Q(x)` be the quantization error

In symmetric grids with symmetric rounding rules, the expected error satisfies:

```

E[e] ≈ 0

```

for input distributions that are themselves symmetric around zero.

Balanced ternary **enables** such grids. It does not enforce symmetric rounding by default.

---

## Why it matters

### Drift accumulates in iterative systems
In iterative processes (e.g., repeated updates, feedback systems, training loops), small rounding biases can accumulate into systematic drift.

Balanced ternary reduces *structural bias*, making unbiased rounding easier to implement correctly.

### Zero becomes stable, not fragile
When zero is a central representable value, transitions around zero are less likely to introduce asymmetric artifacts caused by representation boundaries.

---

## What this enables

- Cleaner discretization schemes centered on zero
- Reduced directional bias *when rounding is symmetric*
- More predictable behavior in some iterative or feedback-heavy systems
- A principled foundation for ternary quantization schemes

---

## What this does not enable

- It does not eliminate rounding error
- It does not guarantee reduced error for arbitrary input distributions
- It does not improve accuracy without careful rounding design
- It does not replace numerical analysis

Symmetry is a *structural affordance*, not a guarantee.

---

## Common misinterpretations

- **“Balanced ternary is unbiased by default.”**  
  False. Bias depends on rounding rules.

- **“Drift is always reduced.”**  
  False. Drift reduction is distribution- and system-dependent.

- **“This replaces higher-precision arithmetic.”**  
  False. It changes discretization behavior, not precision limits.

---

## Failure modes

- Asymmetric rounding reintroduces bias
- Lazy or inconsistent canonicalization breaks symmetry
- Encoding artifacts dominate theoretical advantages
- Measurement noise masks small effects

See `docs/40-failure-modes.md` for detailed discussion.

---

## Links

- Signed symmetry: `docs/deltas/01-signed-symmetry.md`
- Quantization context: `docs/deltas/04-compression-first-quantization.md`
- Constraints: `docs/20-what-doesnt.md`

---
