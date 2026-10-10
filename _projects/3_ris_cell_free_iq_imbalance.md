---
layout: page
title: Precoding for Uplink RIS-Assisted Cell-Free MIMO-OFDM Systems with IQ Imbalance
description: Joint UE precoding, multi-RIS configuration, and BS combining that account for transmit and receive IQ imbalance and the image-subcarrier leakage it causes in OFDM. 28 GHz.
img: assets/img/projects/ris_iqi/card_ris_uplink.png
importance: 4
---

**Paper:** N. Reyhanian, R. Ghaderi Zefreh, P. Ramezani, and E. Björnson, "Precoding for Uplink RIS-Assisted Cell-Free MIMO-OFDM Systems with IQ Imbalance," journal manuscript.

<div class="row text-center mt-4 mb-1">
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">11–47×</div>
      <div style="font-size: 0.85rem;">sum-rate over the conventional WMMSE design that ignores IQ imbalance</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">up to +40%</div>
      <div style="font-size: 0.85rem;">sum-rate over classical MMSE precoding and combining, at about the same per-iteration cost</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">up to 3.4×</div>
      <div style="font-size: 0.85rem;">sum-rate over an IQ-imbalance-aware design with unoptimized RIS settings</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">±30°</div>
      <div style="font-size: 0.85rem;">phase and ±30% amplitude IQ imbalance at every UE and BS, handled by design</div>
    </div>
  </div>
</div>
<p style="font-size: 0.8rem; text-align: center; opacity: 0.8;">Gains computed from the plotted data in the paper's figures. Setup: 28&nbsp;GHz, 12 subcarriers, 4 BSs with 16–32 antennas, 2 RISs with 81 elements each, 4-antenna UEs, level-3 IQ imbalance.</p>

### Why it matters for modem and RF system design

- **IQ imbalance is a real transceiver impairment.** Low-cost wideband radios at the UEs and BSs mismatch the in-phase and quadrature branches. In OFDM, this leaks each subcarrier _s_ into its mirror image _−s_. Ordinary precoders and combiners ignore that leakage.
- **Impairments on both ends of the link.** Earlier work typically assumes an ideal transmitter. This design models IQ imbalance at both the transmitting UEs and the receiving BSs, and builds the image-subcarrier coupling into the precoder, the combiner, and the RIS design.
- **Low complexity.** UE precoders are found by bisection and BS combiners in closed form. The multi-RIS configuration, which couples each subcarrier pair (_s_, _−s_) through one frequency-flat reflection matrix, is solved globally by a block coordinate descent with closed-form bisection updates.
- **Robust in practice.** With imperfect channel estimates the design stays robust, and the gain from extra BS antennas is preserved.

### System architecture

<div class="row justify-content-sm-center">
    <div class="col-md-9 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/ris_iqi/system_model.png" title="RIS-assisted cell-free uplink system" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b>RIS-assisted cell-free uplink.</b> <i>K</i> multi-antenna UEs reach <i>C</i> base stations over direct links and through <i>Q</i> passive RISs. The BSs forward their signals over fronthaul to a central processing unit that performs fully centralized combining. Every UE transmitter and every BS receiver suffers from IQ imbalance.
</div>

### Approach

The design maximizes the **uplink sum-rate** of a fully centralized, multi-user cell-free MIMO-OFDM network. It jointly optimizes UE precoding, multi-RIS coefficients, and BS combining under two-sided IQ imbalance. The problem is nonconvex, so we developed a **WMMSE-based block coordinate descent (BCD)** algorithm:

- **BS combiners:** closed-form updates.
- **MSE weights:** closed-form updates.
- **UE precoders:** solved by bisection search under each UE's power budget.
- **Multi-RIS coefficients:** the subproblem couples subcarrier pairs (_s_, _−s_) through a single frequency-flat reflection matrix. It is reformulated and **solved globally** with a low-cost BCD that uses closed-form, bisection-based updates.
- **Guarantees and complexity:** every block subproblem is convex, so the BCD converges to a stationary solution. The per-iteration complexity is derived.

### Results

<div class="row justify-content-sm-center">
    <div class="col-md-7 mt-3">
        {% include figure.liquid loading="eager" path="assets/img/projects/ris_iqi/methods_comparison.png" title="Sum-rate of the proposed design versus MMSE, random-RIS, and IQI-blind designs" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b>Beats classical, heuristic, and impairment-blind designs.</b> Under level-3 IQ imbalance, the proposed design delivers 11–47× the sum-rate of conventional WMMSE that ignores IQ imbalance, up to 40% more than classical MMSE, and up to 3.4× more than an IQ-imbalance-aware design with random RIS settings. The gains are largest at low transmit power.
</div>

<div class="row">
    <div class="col-12 mt-4">
        {% include figure.liquid path="assets/img/projects/ris_iqi/sumrate_vs_power.png" title="Sum-rate versus transmit power for 4 and 8 UEs" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Scales with antennas and users.</b> Under level-3 IQ imbalance, the sum-rate grows with transmit power and BS antennas (16 to 32). With 8 UEs and 32 antennas it exceeds 80&nbsp;bps/Hz at 10&nbsp;dBm.
        </div>
    </div>
</div>

<div class="row">
    <div class="col-12 mt-4">
        {% include figure.liquid path="assets/img/projects/ris_iqi/iqi_levels.png" title="Sum-rate under three IQ imbalance severities" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Graceful degradation with impairment severity.</b> The sum-rate decreases as IQ imbalance grows from level 1 (±10°, amplitude 0.9–1.1) to level 3 (±30°, amplitude 0.7–1.3). More BS antennas raise the rate at every level.
        </div>
    </div>
</div>

<div class="row">
    <div class="col-12 mt-4">
        {% include figure.liquid path="assets/img/projects/ris_iqi/csi_uncertainty.png" title="Sum-rate under CSI estimation uncertainty" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Robust to imperfect channel knowledge.</b> The design is computed on estimated channels and evaluated on the true ones. Low and high estimation uncertainty reduce the sum-rate, but the design stays robust and keeps the gain from more BS antennas.
        </div>
    </div>
</div>

<div class="row">
    <div class="col-12 mt-4">
        {% include figure.liquid path="assets/img/projects/ris_iqi/convergence.png" title="Convergence of the proposed algorithm" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Converges in about 30 iterations</b> with four UEs and level-3 IQ imbalance at −5&nbsp;dBm.
        </div>
    </div>
</div>

### Simulation testbed

Implemented in **MATLAB**, using:

- **Numerical computing:** MATLAB's built-in linear algebra (BLAS/LAPACK)
- **Figure regeneration:** Python with NumPy, SciPy, and Matplotlib

**Size:**

- About 31,600 lines of MATLAB across 213 files over the project's development (about 20,300 lines of code). The final simulation testbed is about 12,000 lines.
- About 1,100 lines of Python for the figures.

### Simulation parameters

- **Network:**
  - 4 BSs at (±30, ±30, 3)&nbsp;m with 16–32 receive antennas each.
  - 2 RISs at (±40, 175, 10)&nbsp;m with 81 elements each, as uniform planar arrays.
  - UEs placed randomly within 30&nbsp;m of (0, 350, 1.5)&nbsp;m, each with 4 antennas and 4 streams.
- **Band:** 28&nbsp;GHz carrier, 12 subcarriers with 15&nbsp;kHz spacing (180&nbsp;kHz).
- **Channel:** Rician (κ&nbsp;=&nbsp;10) with 4 taps and 4 paths per tap.
- **Path loss:** 3GPP model with exponent 2.2 for links through a RIS and 4.8 for direct UE–BS links.
- **IQ imbalance:** amplitude and phase drawn uniformly from:
  - level 1: 0.9–1.1 and ±10°
  - level 2: 0.8–1.2 and ±20°
  - level 3: 0.7–1.3 and ±30°
