# Why Now

Balanced ternary is not new. What is new is the **computing environment in which it is being reconsidered**.

This document explains why balanced ternary is *worth exploring now*, without claiming inevitability or superiority.

---

## Evidence tags

* **Proven**: observable industry-wide trends
* **Plausible**: supported by partial evidence and active practice
* **Speculative**: depends on future shifts

---

## 1) Bandwidth dominates compute

For much of modern computing history, arithmetic throughput was the primary constraint. Today, in many domains, **memory bandwidth and data movement dominate cost**.

Examples:

* Large language models and deep neural networks
* Edge inference and embedded systems
* Data-intensive pipelines with poor cache locality

**Why this matters**

* Representation size directly impacts bandwidth, cache pressure, and energy.
* Gains from denser representations can outweigh arithmetic overhead.

Balanced ternary is relevant here not because it is faster, but because it **offers alternative discretization points** with strong symmetry properties.

**Status:** Proven

---

## 2) Quantization is no longer fringe

Reduced-precision computation is now mainstream:

* INT8 and INT4 inference
* Weight-only quantization
* Mixed-precision training
* Model compression as a first-class concern

This represents a shift in values:

> *Exactness everywhere is no longer the default assumption.*

Balanced ternary fits this environment because it is:

* Minimal
* Signed
* Symmetric
* Algebraically disciplined

It offers a mathematically grounded alternative to ad-hoc discretization schemes.

**Status:** Proven

---

## 3) Zero-centered distributions are common

Many real-world signals and learned parameters cluster near zero:

* Neural network weights
* Error terms
* Residuals and deltas
* Control signals

Balanced ternary treats zero as a **first-class state**, not a boundary or exception.

**Why this matters**

* Zero becomes stable, cheap, and semantically meaningful.
* Discretization aligns with distribution shape rather than fighting it.

This does not guarantee performance or accuracy gains, but it **reduces representational friction**.

**Status:** Plausible

---

## 4) Tooling makes experimentation cheap

A decade ago, exploring alternative numeric systems required custom hardware or bespoke toolchains.

Today:

* Compiler plugins allow semantic experiments
* IR-level transformations are accessible
* Simulation is fast and cheap
* Open model formats enable end-to-end testing

This lowers the cost of *being wrong*, which is essential for exploratory work.

Balanced ternary can now be tested as a **semantic hypothesis**, not a hardware gamble.

**Status:** Proven

---

## 5) Energy constraints reframe priorities

Energy per operation increasingly matters:

* Mobile and edge devices
* Data center power limits
* Thermal constraints
* Battery-bound environments

Energy cost is often dominated by:

* Memory access
* Data movement
* Switching activity

Balanced ternary does not automatically reduce energy use, but it **opens new trade spaces** in representation and switching behavior that binary systems do not explore.

**Status:** Plausible

---

## 6) Hardware monoculture creates blind spots

Modern computing has converged on a narrow set of assumptions:

* Binary logic
* Two-valued conditionals
* Fixed radix arithmetic

This monoculture optimizes well locally, but can obscure alternatives that matter under different constraints.

Exploring balanced ternary now is less about replacement and more about **mapping the edges of the design space** while tooling and curiosity still permit it.

**Status:** Plausible

---

## 7) This is a window, not a prophecy

Balanced ternary is not inevitable.
It is not guaranteed to win.
It may remain niche.

But the current moment uniquely combines:

* Pressure on representation efficiency
* Acceptance of reduced precision
* Accessible experimentation tools
* Willingness to trade exactness for throughput or energy

That combination is not permanent.

**Why now**

* Because experimentation is cheap
* Because assumptions are already shifting
* Because the opportunity cost of *not* exploring is low

**Status:** Proven (conditions), Speculative (long-term impact)

---

## Summary: why now, precisely

Balanced ternary is worth exploring now because:

* Bandwidth and energy matter more than raw arithmetic
* Quantization is accepted rather than resisted
* Toolchains allow safe, incremental experimentation
* Zero-centered representations align with real workloads

None of this implies inevitability.

It implies opportunity.

**Next:** `docs/40-failure-modes.md`
