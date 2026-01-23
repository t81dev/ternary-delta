# Linux Ternary Store (Speculative)

This document describes a **hypothetical Linux kernel memory service**
that uses **ternary representation as a semantic compression layer**
to reduce **memory traffic** and **effective working-set size**
for *explicitly qualified workloads*.

It does **not** propose new hardware.  
It does **not** redefine virtual memory.  
It does **not** transparently alter general-purpose allocations.

All behavior described here is **opt-in, bounded, reversible, and failure-tolerant**.

---

## Evidence tag

**Status:** Speculative (design), Plausible (mechanisms)

---

## Problem statement

On modern Linux systems, memory pressure is increasingly driven by
**bandwidth and cache behavior**, not raw capacity.

In several workloads—most notably ML inference—large, read-mostly numeric
buffers dominate:
- cache misses,
- DRAM bandwidth consumption,
- and memory-related energy cost.

Existing Linux memory compression mechanisms:
- operate on bit patterns (e.g., LZ4, zstd),
- destroy numeric structure,
- require full decompression,
- and are not value-aware.

This document explores whether **ternary semantic compression**
could serve as a *specialized in-RAM representation* for qualifying data,
targeting traffic reduction rather than capacity illusion.

---

## Non-goals (explicit)

This system does **not** attempt to:

- Make RAM ternary
- Replace DRAM, DIMMs, or NUMA topology
- Redefine page tables, pointers, or address spaces
- Improve random-access latency
- Apply universally or invisibly to processes

Any design that violates these constraints is out of scope.

---

## Core idea

> **Some memory pages are better treated as meaning-preserving,
> lossy numeric representations than as exact bit patterns.**

For such pages:
- bytes moved matter more than exact bit fidelity,
- values are read far more often than written,
- approximation is acceptable by contract,
- and layout is regular and predictable.

Balanced ternary offers:
- minimal signed discretization,
- cheap and deterministic expansion,
- predictable structure,
- and first-class representation of zero and sign.

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

## Where it integrates in Linux

The system is modeled as a **memory-management service**, not a device driver.

Conceptual integration points include:

- zswap / zram (analogous role, different representation)
- Page cache and anonymous memory paths
- Virtual Memory Areas (VMAs)
- Page fault handling
- Write-protection and COW machinery
- Memory reclaim and working-set heuristics
- Optional interaction with DAMON (access-pattern awareness)

No changes are made to:
- user virtual addresses,
- page table formats,
- CPU instruction semantics.

---

## Storage model

### Binary expanded form
- Standard Linux page
- Fully readable and writable
- Used when hot or mutable

### Ternary compact form
- Stored in RAM
- Ternary-encoded numeric payload
- Associated metadata:
  - scale factor
  - quantization profile
  - version / checksum
- Not directly addressable by user code

Pages transition between these forms according to policy.

---

## Page lifecycle (conceptual)

1. **Allocation / mapping**
   - A process creates or maps a ternary-eligible region.
   - Pages initially exist in binary form.

2. **Cooling**
   - Pages become read-mostly and leave the active working set.

3. **Compaction**
   - Pages are converted to ternary form.
   - Binary pages are released.

4. **Access**
   - On read fault, a page is:
     - expanded into binary form, or
     - serviced via a cached expanded copy.

5. **Write attempt**
   - A write fault forces immediate expansion.
   - The page becomes binary and writable.
   - Ternary backing is discarded or regenerated later.

At all times, **correctness dominates compression**.

---

## Safety mechanisms

### Explicit contract
The kernel never infers tolerance from content.
All lossy behavior is explicitly requested.

### Write interception
- Compact pages are write-protected.
- Any write triggers expansion via COW-like behavior.

### Bounded scope
- Only marked VMAs participate.
- Failure is confined to the owning process.

### Canonicalization discipline
- Ternary encodings are canonical.
- No ambiguous or multi-form representations are permitted.

---

## Minimal API surface (conceptual)

### Option 1: Ternary-backed mapping (preferred)

Exposed via a new mapping type or madvise-style extension.

- `ternary_create_mapping(fd | anon, params)`
- `mmap(..., TERNARY_MAP)`
- `ternary_set_policy(addr, len, policy)`
- `ternary_prefetch(addr, len)` (advisory)
- `ternary_purge(addr, len)` (allow compaction)
- `munmap(...)`

**Policy (minimal)**
- data type hint
- quantization mode (ternary symmetric / scaled)
- error tolerance profile
- write policy (read-only | inflate-on-write)

---

### Option 2: madvise-style opt-in (riskier)

- `madvise(addr, len, MADV_TERNARY, policy)`
- `madvise(addr, len, MADV_NOTERNARY)`

This reduces ceremony but increases misuse risk.

---

## What this system optimizes

- DRAM bandwidth consumption
- Cache residency
- Effective working-set size
- Memory traffic per operation

It does **not** optimize:
- arithmetic throughput,
- pointer-heavy workloads,
- write-intensive access patterns.

---

## Failure modes (non-exhaustive)

- Expansion latency exceeds memory savings
- Thrashing between compact and expanded states
- Poor reclaim or DAMON decisions
- Misdeclared tolerance leading to quality loss
- CPU cost outweighs bandwidth reduction

These failures are acceptable.  
**Silent corruption is not.**

---

## Relationship to ternary-delta

This document corresponds to:

- **Delta:** compression-first quantization
- **Layer:** system-level representation
- **Adoption path:** Stage 4–5 boundary
- **Status:** speculative until measured

No semantic claims in this repository depend on this system.

---

## Why this is deferred by design

Kernel and MM work are the most expensive layers to explore.

This document exists to:
- clarify misconceptions,
- bound speculation,
- and define what “ternary memory” could mean **without redefining RAM**.

Any real value must be demonstrated first at the library and application level.

---

## Summary

A Linux ternary store would not:
- make RAM denser,
- change hardware economics,
- or replace existing memory subsystems.

It could:
- reduce memory traffic,
- shrink effective working sets,
- trade precision for bandwidth,
- and improve performance in narrow, qualified domains.

Only measurement can validate that claim.

---

## Links

- Compression-first delta: `docs/deltas/04-compression-first-quantization.md`
- Constraints: `docs/20-what-doesnt.md`
- Failure modes: `docs/40-failure-modes.md`
- Adoption discipline: `docs/50-adoption-path.md`

---
