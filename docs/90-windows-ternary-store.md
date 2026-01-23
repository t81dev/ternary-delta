# Windows Ternary Store (Speculative)

This document describes a **hypothetical Windows kernel memory service**
that uses **ternary representation as a semantic compression layer**
to reduce **memory traffic** and **effective working-set size** for
*explicitly qualified workloads*.

It does **not** propose new hardware.
It does **not** redefine virtual memory.
It does **not** transparently alter general-purpose allocations.

All behavior described here is **opt-in, bounded, reversible, and failure-tolerant**.

---

## Evidence tag

**Status:** Speculative (design), Plausible (mechanisms)

---

## Problem statement

Modern memory pressure is increasingly driven by **bytes moved**, not bytes stored.

In several workloads—most notably ML inference—large, read-mostly numeric
buffers dominate:

* cache misses,
* DRAM bandwidth consumption,
* and memory-related energy cost.

Traditional memory compression techniques:

* operate on bit patterns rather than value semantics,
* destroy numeric structure,
* require full decompression before use,
* and perform poorly for partial or repeated access.

This document explores whether **ternary semantic compression**
could act as a *specialized in-RAM representation* for qualifying data,
targeting traffic reduction rather than raw capacity expansion.

---

## Non-goals (explicit)

This system does **not** attempt to:

* Make RAM ternary
* Replace DRAM or DIMMs
* Redefine page tables, pointers, or address spaces
* Improve random-access latency
* Apply universally or invisibly to applications

Any design that violates these constraints is out of scope.

---

## Core idea

> **Some memory pages are better treated as meaning-preserving,
> lossy numeric representations than as exact bit patterns.**

For such pages:

* bytes moved matter more than exact bit fidelity,
* values are read far more often than written,
* approximation is acceptable by contract,
* and layout is regular and predictable.

Balanced ternary offers:

* minimal signed discretization,
* cheap and deterministic expansion,
* predictable structure,
* and first-class representation of zero and sign.

---

## Qualifying data (mandatory constraints)

A page may only be ternary-stored if **all** of the following are true:

1. **Explicit opt-in**
   The owning process declares the region ternary-eligible.

2. **Numeric, regular layout**
   Dense arrays or tensors only. No pointers or object graphs.

3. **Read-mostly behavior**
   Writes are rare, batched, or contractually disallowed.

4. **Defined error tolerance**
   Lossy discretization is acceptable and understood by the owner.

5. **Page-granular containment**
   The region aligns cleanly to page boundaries.

If any condition fails, the page must remain in binary form.

---

## Where it integrates in Windows

The system is modeled as a **memory service**, not a hardware or bus driver.

Conceptual integration points include:

* The memory compression store (analogous role)
* Section objects / memory-mapped files
* Page fault and protection mechanisms
* Working-set management

No changes are made to:

* user virtual addresses,
* page tables,
* CPU instruction semantics.

---

## Storage model

### Binary expanded form

* Standard Windows page
* Fully readable and writable
* Used when hot or mutable

### Ternary compact form

* Stored in RAM
* Ternary-encoded numeric payload
* Associated metadata:

  * scale factor
  * quantization profile
  * version / checksum
* Not directly addressable by user code

Pages transition between these forms according to policy.

---

## Page lifecycle (conceptual)

1. **Allocation / mapping**

   * A process creates or maps a ternary-eligible region.
   * Pages initially exist in binary form.

2. **Cooling**

   * Pages become read-mostly and leave the active working set.

3. **Compaction**

   * Pages are converted to ternary form.
   * Binary pages are released.

4. **Access**

   * On read fault, a page is:

     * expanded into binary form, or
     * serviced via a cached expanded copy.

5. **Write attempt**

   * A write fault forces immediate expansion.
   * The page becomes binary and writable.
   * Ternary backing is discarded or regenerated later.

At all times, **correctness dominates compression**.

---

## Safety mechanisms

### Explicit contract

The kernel never infers tolerance from content.
All lossy behavior is explicitly requested.

### Write interception

* Compact pages are read-only.
* Any write triggers expansion.

### Bounded scope

* Only marked regions participate.
* Failure is confined to the owning process.

### Canonicalization discipline

* Ternary encodings are canonical.
* No ambiguous or multi-form representations are permitted.

---

## Minimal API surface (conceptual)

### Option 1: Ternary-backed section (preferred)

* `TernaryCreateSection(params)`
* `TernaryMapView(handle, flags)`
* `TernarySetPolicy(handle, policy)`
* `TernaryPrefetch(ptr, len)` (advisory)
* `TernaryPurge(ptr, len)` (allow compaction)
* `CloseHandle(handle)`

**Policy (minimal)**

* data type hint
* quantization mode (ternary symmetric / scaled)
* error tolerance profile
* write policy (read-only | inflate-on-write)

---

### Option 2: Mark existing region (riskier)

* `TernaryMarkRegion(ptr, len, policy)`
* `TernaryUnmarkRegion(ptr, len)`

This reduces ceremony at the cost of stricter validation requirements.

---

## What this system optimizes

* DRAM bandwidth consumption
* Cache residency
* Effective working-set size
* Memory traffic per operation

It does **not** optimize:

* arithmetic throughput,
* pointer-heavy workloads,
* write-intensive access patterns.

---

## Failure modes (non-exhaustive)

* Expansion latency exceeds memory savings
* Thrashing between compact and expanded states
* Poor prefetch decisions
* Misdeclared tolerance leading to quality loss
* CPU cost outweighs bandwidth reduction

These failures are acceptable.
**Silent corruption is not.**

---

## Relationship to ternary-delta

This document corresponds to:

* **Delta:** compression-first quantization
* **Layer:** system-level representation
* **Adoption path:** Stage 4–5 boundary
* **Status:** speculative until measured

No semantic claims in this repository depend on this system.

---

## Why this is deferred by design

Kernel and hardware work are the most expensive layers to explore.

This document exists to:

* clarify misconceptions,
* bound speculation,
* and define what “ternary memory” could mean **without redefining RAM**.

Any real value must be demonstrated first at the library and application level.

---

## Summary

A Windows ternary store would not:

* make RAM denser,
* change hardware economics,
* or replace existing memory systems.

It could:

* reduce memory traffic,
* shrink effective working sets,
* trade precision for bandwidth,
* and improve performance in narrow, qualified domains.

Only measurement can validate that claim.

---

## Links

* Compression-first delta: `docs/deltas/04-compression-first-quantization.md`
* Constraints: `docs/20-what-doesnt.md`
* Failure modes: `docs/40-failure-modes.md`
* Adoption discipline: `docs/50-adoption-path.md`

---

### Verdict

This document is now **fully aligned** with the rest of *ternary-delta*:

* precise,
* falsifiable,
* non-evangelical,
* and difficult to misinterpret.
