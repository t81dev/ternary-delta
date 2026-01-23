# Windows Ternary Store (Speculative)

This document describes a **hypothetical Windows kernel memory service**
that uses **ternary representation as a semantic compression layer**
to reduce memory traffic and effective working-set size.

It does **not** propose new hardware.
It does **not** redefine virtual memory.
It does **not** transparently alter all allocations.

Everything here is **opt-in, bounded, and reversible**.

---

## Evidence tag

**Status:** Speculative (design), Plausible (mechanisms)

---

## Problem statement

Modern memory pressure is increasingly driven by **traffic**, not capacity.

In several workloads—most notably ML inference—large, read-mostly numeric
buffers dominate cache misses, DRAM bandwidth, and energy cost.

Traditional memory compression:
- is bit-pattern based
- destroys numeric structure
- requires full decompression
- struggles with partial access

This document explores whether **ternary semantic compression**
could serve as a *specialized in-RAM representation* for qualifying data.

---

## Non-goals (explicit)

This system does **not** attempt to:

- Make RAM ternary
- Replace DRAM or DIMMs
- Redefine page tables or pointer semantics
- Improve random-access latency
- Apply universally or transparently

Any design that violates these constraints is out of scope.

---

## Core idea

> **Some memory pages are better stored as meaning-preserving,
> lossy numeric representations than as exact bit patterns.**

For those pages:
- bytes moved matter more than bits preserved
- values are read far more often than written
- approximation is acceptable
- layout is regular and known

Ternary representation offers:
- extreme simplicity
- cheap expansion
- predictable structure
- first-class zero and sign

---

## Qualifying data (mandatory constraints)

A page may only be ternary-stored if **all** are true:

1. **Opt-in declaration**  
   The owning process explicitly marks the region as ternary-eligible.

2. **Numeric, regular layout**  
   Dense arrays or tensors. No pointers, no object graphs.

3. **Read-mostly behavior**  
   Writes are rare, batched, or prohibited.

4. **Defined error tolerance**  
   Lossy discretization is acceptable by contract.

5. **Page-granular containment**  
   The region aligns cleanly with page boundaries.

If any condition fails, the page must remain binary.

---

## Where it integrates in Windows

The system is modeled as a **memory service**, not a device driver.

Conceptual integration points:

- Memory compression store (analogous role)
- Section objects / memory-mapped files
- Page fault and protection machinery
- Working-set management

No changes to:
- user virtual addresses
- page tables
- CPU instruction semantics

---

## Storage model

### Binary expanded form
- Standard Windows page
- Fully accessible
- Used when hot or writable

### Ternary compact form
- Stored in RAM
- Ternary-encoded numeric payload
- Associated metadata:
  - scale factor
  - quantization profile
  - checksum / version
- Not directly addressable by user code

Pages move between these forms based on policy.

---

## Page lifecycle (conceptual)

1. **Allocation / mapping**
   - Process creates or maps a ternary-eligible region.
   - Pages start in binary form.

2. **Cooling**
   - Page becomes read-mostly and exits active working set.

3. **Compaction**
   - Kernel converts page to ternary form.
   - Binary page released.

4. **Access**
   - On read fault, page is:
     - expanded into binary form, or
     - serviced via cached expanded copy.

5. **Write attempt**
   - Write fault triggers immediate expansion.
   - Page becomes binary and writable.
   - Ternary backing discarded or updated later.

At all times, correctness favors expansion.

---

## Safety mechanisms

### Explicit contract
The kernel never infers tolerance from data.
All lossy behavior is explicitly requested.

### Write interception
- Pages marked read-only while compact.
- Any write triggers expansion.

### Bounded scope
- Only marked regions participate.
- Failure affects only the owning process.

### Canonicalization discipline
- Ternary form is canonical.
- No ambiguous encodings allowed.

---

## Minimal API surface (conceptual)

### Option 1: Ternary-backed section (preferred)

- `TernaryCreateSection(params)`
- `TernaryMapView(handle, flags)`
- `TernarySetPolicy(handle, policy)`
- `TernaryPrefetch(ptr, len)` (advisory)
- `TernaryPurge(ptr, len)` (allow compaction)
- `CloseHandle(handle)`

**Policy (minimal)**
- data type hint
- quantization mode (ternary symmetric / scaled)
- error tolerance profile
- write policy (read-only | inflate-on-write)

---

### Option 2: Mark existing region (riskier)

- `TernaryMarkRegion(ptr, len, policy)`
- `TernaryUnmarkRegion(ptr, len)`

This trades safety for simplicity and would require stricter validation.

---

## What this system optimizes

- Fewer bytes moved from DRAM
- Higher cache residency
- Smaller effective working sets
- Reduced memory bandwidth pressure

It does **not** optimize:
- arithmetic throughput
- pointer-heavy access
- write-heavy workloads

---

## Failure modes (non-exhaustive)

- Expansion latency dominates access
- Thrashing between compact/expanded states
- Poor prefetch decisions
- Misdeclared tolerance causing quality loss
- CPU cost outweighs bandwidth savings

These failures are acceptable; silent corruption is not.

---

## Relationship to ternary-delta

This document corresponds to:

- **Delta:** compression-first quantization
- **Layer:** system-level representation
- **Adoption path:** Stage 4–5 boundary
- **Status:** speculative until measured

No semantic claims depend on this system.

---

## Why this is deferred by design

Hardware and kernel work are the most expensive layers to explore.

This document exists to:
- clarify misconceptions
- bound speculation
- define what “ternary memory” could mean *without* redefining RAM

Actual value must be proven at the library and application level first.

---

## Summary

A Windows ternary store would not:
- make RAM denser
- change hardware economics

It could:
- reduce memory traffic
- shrink working sets
- trade precision for bandwidth
- improve performance in narrow, qualified domains

Only measurement can validate that claim.

---

## Links

- Compression-first delta: `docs/deltas/04-compression-first-quantization.md`
- Constraints: `docs/20-what-doesnt.md`
- Failure modes: `docs/40-failure-modes.md`
- Adoption discipline: `docs/50-adoption-path.md`

---
