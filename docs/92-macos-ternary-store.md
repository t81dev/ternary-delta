# macOS Ternary Store (Speculative, User-Space)

This document describes a **hypothetical macOS user-space memory service**
that uses **ternary representation as a semantic compression layer**
to reduce **memory traffic** and **effective working-set size**
for *explicitly qualified workloads*.

Unlike the Windows and Linux designs, this system is **not a kernel service**.
It is implemented entirely in **user space**, using existing virtual memory
and fault-handling primitives available on macOS.

It does **not** propose new hardware.  
It does **not** redefine virtual memory.  
It does **not** modify or extend the macOS kernel.

All behavior described here is **opt-in, bounded, reversible, and failure-tolerant**.

---

## Evidence tag

**Status:** Speculative (design), Plausible (mechanisms, user-space)

---

## Problem statement

On modern macOS systems, memory pressure is mitigated by aggressive
system-managed compression, but **memory traffic and cache behavior**
remain limiting factors in data-intensive workloads.

In particular, workloads such as ML inference exhibit:
- large, read-mostly numeric buffers,
- high cache miss rates,
- repeated DRAM fetches of cold-but-reused data.

macOS does not provide a supported mechanism for third-party extensions
to its kernel memory compression subsystem.
However, it *does* provide sufficient user-space primitives to emulate
a page-backed representation layer within a single process.

This document explores whether **ternary semantic compression**
can be prototyped as a *user-space paging mechanism* on macOS
to evaluate its impact on memory traffic and working-set behavior.

---

## Non-goals (explicit)

This system does **not** attempt to:

- Integrate with macOS kernel memory compression
- Replace or extend the VM subsystem
- Introduce kernel extensions or DriverKit components
- Transparently alter general-purpose allocations
- Improve random-access latency

Any design requiring kernel modification is out of scope.

---

## Core idea

> **Some memory regions are better treated as meaning-preserving,
> lossy numeric representations than as exact bit patterns,
> even when managed entirely in user space.**

For such regions:
- bytes moved matter more than bit-exact fidelity,
- values are read far more often than written,
- approximation is acceptable by contract,
- and layout is regular and known in advance.

Balanced ternary offers:
- minimal signed discretization,
- cheap deterministic expansion,
- predictable structure,
- first-class representation of zero and sign.

---

## Qualifying data (mandatory constraints)

A region may only participate if **all** of the following are true:

1. **Explicit opt-in**  
   The owning code explicitly allocates the region as ternary-backed.

2. **Numeric, regular layout**  
   Dense arrays or tensors only. No pointers or object graphs.

3. **Read-mostly behavior**  
   Writes are rare, batched, or contractually disallowed.

4. **Defined error tolerance**  
   Lossy discretization is acceptable and understood by the owner.

5. **Page-granular containment**  
   The region aligns cleanly to page boundaries.

If any condition fails, the region must be treated as binary.

---

## Where it integrates on macOS

This system is implemented as a **user-space memory manager** layered on top of:

- `mmap` / `vm_allocate`
- `mprotect` / `vm_protect`
- Fault handling via:
  - POSIX `SIGSEGV` handlers, or
  - Mach exception ports (optional, more complex)

No interaction occurs with:
- the kernel memory compressor,
- page tables,
- or CPU instruction semantics.

The scope is strictly per-process.

---

## Storage model

### Binary expanded form
- Standard memory pages
- Readable (and optionally writable)
- Present only while the page is “hot”

### Ternary compact form
- Stored in user-managed memory
- Ternary-encoded numeric payload
- Associated metadata:
  - scale factor
  - quantization profile
  - version / checksum
- Not directly accessible by application code

Only one form is active for a page at a time.

---

## Page lifecycle (conceptual, user-space)

1. **Allocation**
   - A region is reserved with `mmap`.
   - Pages are initially protected (`PROT_NONE` or `PROT_READ`).

2. **Fault-driven expansion**
   - On first access, a fault is raised.
   - The handler decodes the ternary form into a binary page.
   - Page protections are updated to allow access.

3. **Read-mostly residency**
   - Pages remain readable while hot.
   - Optional prefetch expands adjacent pages.

4. **Purge / compaction**
   - On explicit hint or memory pressure:
     - binary pages are discarded,
     - ternary form is retained.

5. **Write attempt**
   - A write fault triggers:
     - expansion (if needed),
     - promotion to writable binary form,
     - optional discard of ternary backing.

At all times, **correctness favors expansion**.

---

## Safety mechanisms

### Explicit contract
No tolerance is inferred.
All lossy behavior is explicitly requested.

### Write interception
- Pages are kept read-only when compact.
- Writes trigger expansion.

### Bounded scope
- Only ternary-backed regions participate.
- Failure affects only the current process.

### Canonicalization discipline
- Ternary encodings are canonical.
- No ambiguous representations are permitted.

---

## Minimal API surface (conceptual)

- `ternary_map(size, policy)`
- `ternary_set_policy(ptr, len, policy)`
- `ternary_prefetch(ptr, len)`
- `ternary_purge(ptr, len)`
- `ternary_unmap(ptr, len)`

**Policy (minimal)**
- data type hint
- quantization mode (ternary symmetric / scaled)
- error tolerance profile
- write policy (read-only | inflate-on-write)

---

## What this system optimizes

- DRAM traffic per access
- Cache residency
- Effective working-set size
- Bandwidth-related energy use

It does **not** optimize:
- arithmetic throughput,
- pointer-heavy workloads,
- write-intensive access patterns.

---

## Failure modes (non-exhaustive)

- Fault-handling overhead dominates
- Decode cost outweighs traffic savings
- Thrashing between expanded and compact forms
- Poor prefetch heuristics
- Misdeclared tolerance causing quality loss

These failures are acceptable.  
**Silent corruption is not.**

---

## Relationship to ternary-delta

This document corresponds to:

- **Delta:** compression-first quantization
- **Layer:** user-space system representation
- **Adoption path:** Stage 3–4 (falsification)
- **Status:** speculative until measured

No semantic claims depend on this system.

---

## Why this exists

macOS does not support third-party kernel memory services.

This document exists to:
- enable low-cost falsification,
- validate representation-level claims,
- and provide a macOS-native experimentation path
  without privileged access.

If value cannot be demonstrated here,
it should not be pursued at lower layers.

---

## Summary

A macOS ternary store would not:
- integrate with the kernel,
- alter system-wide memory behavior,
- or change hardware economics.

It could:
- reduce memory traffic in qualified workloads,
- shrink effective per-process working sets,
- trade precision for bandwidth,
- and validate or falsify ternary semantic compression cheaply.

Only measurement can justify further exploration.

---
