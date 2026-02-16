# Ternary Delta

**Balanced ternary: what changes, what doesn’t, and why now.**

This repository is a **delta-oriented reference**.  
It does not argue that ternary replaces binary.

It documents:
- where **balanced ternary changes semantics**,
- where **modern computing constraints remain fixed**, and
- why **present conditions make experimentation rational**, not nostalgic.

It is part of my broader collection of ternary-computing experiments; each repo under https://github.com/t81dev shares a different slice of the story, and this one is focused on describing what changes, what stays the same, and why experimentation makes sense now. The goal is clarity, not advocacy.

---

## Executive Summary

Balanced ternary introduces a symmetric signed digit set **{-1, 0, +1}**.  
That single change produces real semantic deltas in representation, rounding behavior, logic availability, and compression-oriented quantization.

At the same time, modern computing remains structurally binary:
memory addressing, operating systems, ABIs, floating point, toolchains, and hardware datapaths do not change.

This repository separates:

- **Semantic deltas** — what changes by definition  
- **Constraints** — what does not change in practice  
- **Failure modes** — where ternary underperforms or complicates systems  
- **Adoption discipline** — how to explore without breaking things  

Claims are classified as **Proven**, **Plausible**, or **Speculative**.

---

## The Delta Table (authoritative index)

Each semantic delta links to a standalone definition file.
Constraints and speculation link to dedicated documents.

| Layer | Binary Default | Ternary Delta | Status | Definition |
|------|---------------|---------------|--------|------------|
| Integer math | Unsigned / signed split | Symmetry around zero | Proven | [`01-signed-symmetry.md`](docs/deltas/01-signed-symmetry.md) |
| Rounding | Asymmetric bias | Symmetric grids (rule-dependent) | Plausible | [`02-rounding-and-drift.md`](docs/deltas/02-rounding-and-drift.md) |
| Logic | Boolean collapse | Neutral-aware logic availability | Proven | [`03-logic-availability.md`](docs/deltas/03-logic-availability.md) |
| ML weights | Binary quantization | Compression-first ternary discretization | Plausible | [`04-compression-first-quantization.md`](docs/deltas/04-compression-first-quantization.md) |
| Memory addressing | Binary | No change | Proven | [`20-what-doesnt.md`](docs/20-what-doesnt.md) |
| Floating point | IEEE-754 | No change | Proven | [`20-what-doesnt.md`](docs/20-what-doesnt.md) |
| ISA | Binary ops | Ternary ops | Plausible | [`50-adoption-path.md`](docs/50-adoption-path.md) |
| Hardware | Binary devices | Multi-state devices | Speculative | [`80-hardware-notes.md`](docs/80-hardware-notes.md) |

---

## Core documents

### Semantic deltas
- [`docs/deltas/01-signed-symmetry.md`](docs/deltas/01-signed-symmetry.md)
- [`docs/deltas/02-rounding-and-drift.md`](docs/deltas/02-rounding-and-drift.md)
- [`docs/deltas/03-logic-availability.md`](docs/deltas/03-logic-availability.md)
- [`docs/deltas/04-compression-first-quantization.md`](docs/deltas/04-compression-first-quantization.md)

### Constraints and limits
- [`docs/20-what-doesnt.md`](docs/20-what-doesnt.md)
- [`docs/40-failure-modes.md`](docs/40-failure-modes.md)

### Context and discipline
- [`docs/30-why-now.md`](docs/30-why-now.md)
- [`docs/50-adoption-path.md`](docs/50-adoption-path.md)
- [`docs/60-glossary.md`](docs/60-glossary.md)
- [`docs/70-related-work.md`](docs/70-related-work.md)

---

## Evidence taxonomy

- **Proven** — derivable from mathematics or reproducible measurement  
- **Plausible** — demonstrated in constrained systems, not universal  
- **Speculative** — dependent on future hardware

---

## Philosophy

Binary is not wrong. It is contingent.

Balanced ternary is not destiny. It is possibility.

This repository exists to keep that possibility precise, bounded, and testable.

## How to read this repository
1. Start with the Delta Table
2. Read the linked delta definitions
3. Review constraints and failure modes
4. Decide whether any delta matters for your domain

If none do, that is a valid conclusion.

## Status
The conceptual structure of ternary-delta is complete.
Further additions should either:

reduce misunderstanding, or add evidence to an existing delta.

Nothing else.
