# Palm Oil Melting & Retention System

**Industrial thermal systems design and automation architecture for a multi-tank palm oil melting, storage, and trim-heating facility — Mechatronics Engineering Pvt. Ltd. (MEPL), Kathmandu.**

Grounded in the self-authored **VCH Sizing Framework (2nd Ed.)**, the system coordinates a passive solar thermal loop with an auxiliary electric immersion array through PLC automation to deliver demand-driven, high-efficiency process operations.

---

## Table of Contents

1. [Industrial Mandate & Constraints](#1-industrial-mandate--constraints)
2. [Engineering Journey & Design Challenges](#2-engineering-journey--design-challenges)
3. [System Architecture](#3-system-architecture)
4. [Core Technical Deep Dives](#4-core-technical-deep-dives)
5. [The VCH Sizing Framework](#5-the-vch-sizing-framework)
6. [Headline Results & Validation Metrics](#6-headline-results--validation-metrics)
7. [Future Plan](#7-future-plan)
8. [My Contributions](#8-my-contributions)
9. [Appendix: Engineering Notebook](#appendix-engineering-notebook)

---

## 1. Industrial Mandate & Constraints

The objective was to design, size, and architect the automation logic for a raw palm oil processing infrastructure under strict operational and environmental parameters.

| Parameter | Specification |
| :--- | :--- |
| **Production Throughput** | ≥ 2 tonnes of fully processed liquid palm oil per day |
| **Thermal Transition** | Solid-state feedstock at 18.1°C (Kathmandu annual ambient) → 60°C stable liquid process target |
| **Hardware Scope** | Three 10 kL storage/melting vessels, one 5 kL trim-heating replenishment tank, passive solar thermal loop, 36 kW auxiliary electric immersion array |
| **Climate Factor** | Kathmandu solar reliability: **75.9% annually** |

---

## 2. Engineering Journey & Design Challenges

### Challenging the Water-Side Velocity Assumption

The initial hypothesis was that increasing water-side velocity through the submerged hydronic coils would be the primary lever for accelerating the melting phase. Applying the first-principles equations of the VCH Sizing Framework forced a critical course correction.

**The impedance bottleneck.** Modeling the two-phase overall heat transfer coefficient ($U$) revealed that the outer oil-side thermal resistance accounts for over **95%** of total system impedance. Squeezing water faster through a 1-inch bore coil (Case A.B) produced a very high inner-surface heat transfer coefficient ($h_i = 19{,}594\ \text{W/m}^2\text{K}$), yet produced a negligible change in the overall melting rate — while adding hydraulic pump stress and accelerated wear for a mathematically redundant gain.

### Shifting to Algorithmic Design

Once the solid-phase oil-side resistance was confirmed as an unyielding physical bottleneck, the design philosophy shifted from mechanical brute force to **algorithmic throughput recovery**. If a single tank's melting phase cannot be safely accelerated beyond its thermodynamic limit, total plant throughput must be reclaimed by orchestrating a smarter, time-staggered multi-tank queue.

This led directly to the demand-driven "pull" automation architecture. Real-time volume- and temperature-weighted priority matrices ($S_\text{casual}$ and $S_\text{immediate}$) allow the PLC to continuously prepare, melt, and hold oil across all three 10 kL vessels in an overlapping sequence — bypassing single-tank cycle limitations and securing an uninterrupted downstream process stream.

---

## 3. System Architecture

> *Process Flow Diagram (PFD) is available in the repository. Detailed architectural data to be added.*

---

## 4. Core Technical Deep Dives

### Multi-Phase Thermal Modeling

External natural convection profiles for the uninsulated vessels were constructed using rigorous engineering correlations:

- **Vertical tank walls** — Churchill-Chu correlation
- **Horizontal surfaces** — Morgan-McAdams plate equations
- **Internal coil fluid dynamics** — Gnielinski correlation with Kubair-Kuloor enhancements for helical geometries

This approach isolated a **1.07 kW** hold-phase thermal loss profile.

### Dynamic Upper Thermal Boundary ($T_f$)

Rather than a static high-limit cutoff — which risks thermal degradation and pressure transients — the PLC dynamically computes the real-time thermodynamic equilibrium boundary from the instantaneous mass interaction of water, steel, and palm oil:

$$T_{f}(x,\, y,\, m_p) = \frac{239.53\, x + m_p\, y}{239.53 + m_p}$$

| Symbol | Definition |
| :---: | :--- |
| $x$ | Instantaneous temperature of the thermal buffer tank |
| $y$ | Instantaneous temperature of the target melting tank |
| $m_p$ | Estimated remaining mass of solid/liquid oil phase |

This algorithm protects high-pressure line components and prevents product degradation without operator intervention.

### Volume- and Temperature-Weighted Tank Scoring

Two independent PLC calculation blocks evaluate plant state and drive the staggered delivery queue:

- **$S_\text{casual}$ Block** — Manages solar thermal energy distribution to tanks requiring non-urgent pre-heating.
- **$S_\text{immediate}$ Block** — Prioritises auxiliary electric backup (36 kW) to the active processing tank when a downstream demand signal ($\delta$) is high, preventing line starvation.

---

## 5. The VCH Sizing Framework

This system is a full-scale industrial deployment of the **VCH Sizing Framework (2nd Edition)** — a methodology authored specifically for submerged hydronic coil thermal systems. The project served as a definitive verification platform, confirming that unifying coil heat transfer modeling with multi-phase thermal boundary calculations produces highly predictable, field-resilient industrial designs.

---

## 6. Headline Results & Validation Metrics

The finalized engineering proposal was verified against primary operational parameters and absolute worst-case ambient scenarios.

| Performance Metric | Design Target | Primary (75°C HTF) | Worst-Case (70°C HTF) |
| :--- | :---: | :---: | :---: |
| **Daily Plant Throughput** | ≥ 2.0 t/day | **24.23 t/day** (staggered) | **20.48 t/day** (staggered) |
| **Single-Tank Safety Factor** | 1.0× | **3.63×** | **2.42×** |
| **Solar Loop Thermal Coverage** | Maximise | **61.46%** offset (ETC) | Emergency electric override |
| **System Thermal Bottleneck** | Mitigate | Oil-side impedance (>95%) | Oil-side impedance (>95%) |

---

## 7. Future Plan

> *Future expansion and design optimisations to be added.*

---

## 8. My Contributions

> *Personal project contributions, execution details, and internship focus areas to be added.*

---

## Appendix: Engineering Notebook

High-resolution scans of scratchpad derivations, geometric helical coil layout constraints, and early PLC state-machine routing matrices are located in the `engineering_notes/` directory.
