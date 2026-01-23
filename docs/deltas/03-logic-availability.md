# Delta 03: Logic Availability Beyond Boolean Collapse

Balanced ternary introduces a stable neutral value (0) alongside positive (+1) and negative (−1). This expands the set of *available* logical operations without prescribing language defaults or replacing Boolean logic.

This delta concerns **logical availability**, not mandatory adoption.

---

## Evidence tag

**Status:** Proven

---

## Assumptions

This delta assumes:
- **Canonical balanced-ternary form**
- **Explicit interpretation of digit meaning** (ordering or sign semantics)
- Execution within **binary-hosted systems**

No claim is made about programming language semantics or control-flow defaults.

---

## Statement of the delta

### 1) Neutrality becomes a first-class value

Balanced ternary provides a representable state that is neither asserted nor denied:

- `+1` : asserted / positive
- `0`  : neutral / unknown / no-claim
- `-1` : denied / negative

This neutrality is intrinsic to the digit set; it does not require auxiliary flags or sentinel encodings.

---

### 2) Ordering-based logic becomes natural

Because the digit set is ordered (−1 < 0 < +1), certain logical operations arise directly from numeric order:

- **min**: pessimistic aggregation
- **max**: optimistic aggregation
- **median / consensus**: agreement-seeking aggregation

These operations preserve neutrality and expose conflict rather than collapsing it.

---

### 3) Conflict need not collapse to a bit

In Boolean systems, combining opposing claims often collapses to a single value (true/false), losing information about disagreement.

Balanced ternary allows:
- Opposing signs to remain visible
- Neutrality to propagate intentionally
- Conflict to be represented without ad-hoc encoding

---

## Why it matters

### Expressivity without expansion
Many systems already simulate neutrality and indeterminacy using:
- tri-state enums
- NaNs or sentinels
- extra flags and side channels

Balanced ternary makes these states representable *within the value domain itself*.

### Clear separation of availability vs defaults
This delta does **not** argue that all logic should be ternary. It states that ternary logic operations become available *without ceremony* when neutrality is intrinsic.

---

## What this enables

- Neutral-aware aggregation (consensus, voting, confidence fusion)
- Representing unknown or undecided states without extra metadata
- Preserving disagreement rather than forcing collapse
- Cleaner modeling of partial information in narrow domains

---

## What this does not enable

- It does not improve program correctness by itself
- It does not replace Boolean logic
- It does not prescribe control-flow semantics
- It does not remove the need for careful specification

Boolean logic remains sufficient and often preferable for control flow.

---

## Common misinterpretations

- **“This creates a new logic paradigm.”**  
  False. It exposes additional operations; adoption is optional.

- **“Languages should switch to ternary logic.”**  
  False. Language defaults are orthogonal to value availability.

- **“Neutrality means uncertainty is solved.”**  
  False. Neutrality must still be interpreted correctly.

---

## Failure modes

- Ambiguous interpretation of `0`
- Mixing Boolean and ternary logic without clear boundaries
- Overusing neutrality where explicit state machines are clearer
- Assuming neutrality implies correctness

See `docs/40-failure-modes.md` for broader risks.

---

## Links

- Signed symmetry: `docs/deltas/01-signed-symmetry.md`
- Constraints and invariants: `docs/20-what-doesnt.md`
- Failure analysis: `docs/40-failure-modes.md`

---
