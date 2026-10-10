---
layout: page
title: Joint Spatial and Spectral Hybrid Precoding for Multi-User MIMO-OFDM Systems
description: Hybrid precoding that meets OFDM spectral-emission masks and clipping limits by design and tolerates phase-shifter errors. mmWave MU-MIMO-OFDM, 28 GHz.
img: assets/img/projects/hybrid_precoding/card_mu_mimo_beams.png
importance: 1
---

**Paper:** N. Reyhanian, R. Ghaderi Zefreh, P. Ramezani, and E. Björnson, "Joint Spatial and Spectral Hybrid Precoding for Multi-User MIMO-OFDM Systems," [arXiv:2509.07229](https://arxiv.org/abs/2509.07229v3) ([PDF](https://arxiv.org/pdf/2509.07229v3)). Part of this work was presented at IEEE ICC 2026, Glasgow, UK.

<div class="row text-center mt-4 mb-1">
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">+14.5%</div>
      <div style="font-size: 0.85rem;">sum-rate over the RF design of Wang et al. (16 RF chains, 25&nbsp;dBm)</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">up to +20%</div>
      <div style="font-size: 0.85rem;">sum-rate over classical hybrid MMSE, and at least +9.5% at every power from 5 to 25&nbsp;dBm</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">up to +48%</div>
      <div style="font-size: 0.85rem;">over non-robust hybrid MMSE with 30° phase-shifter errors</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">−48.9&nbsp;dBm</div>
      <div style="font-size: 0.85rem;">out-of-band emissions under a −60&nbsp;dBm/100&nbsp;kHz emission mask</div>
    </div>
  </div>
</div>
<p style="font-size: 0.8rem; text-align: center; opacity: 0.8;">Gains computed from the plotted data in the paper's figures. Setup: 28&nbsp;GHz, 20&nbsp;MHz, 64 subcarriers, 32-antenna BS, 4 users × 4 antennas; 8 RF chains unless noted.</p>

### Why it matters for modem and RF system design

- **Emission compliance is part of the design, not a check afterward.** Wireless standards limit out-of-band emissions through the ACLR and the spectral emission mask. Here the mask is a constraint inside the precoder optimization, applied block-averaged in the same way 3GPP NR defines ACLR and PSD compliance.
- **Peak control for DACs and PAs.** OFDM peaks put strict requirements on DACs and power amplifiers and cause distortion and spectral regrowth. A per-antenna clipping limit is enforced on the oversampled, CP-inclusive waveform.
- **Hardware-efficient and impairment-aware.** A partially-connected hybrid array uses one RF chain per subarray. The phase-shifter updates are robust to random phase errors.
- **Low complexity.** Every sub-step is closed-form or a bisection search inside a scalable ADMM. Convergence and per-iteration complexity are analyzed.

### System architecture

<div class="row justify-content-sm-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/hybrid_precoding/architecture.png" title="Partially-connected MU-MIMO-OFDM architecture" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b>End-to-end partially-connected MU-MIMO-OFDM chain.</b> The AP applies a per-subcarrier digital precoder, then IFFT + CP, a DAC, and an RF chain feeding one phase shifter per antenna in its subarray. Each user applies phase-shifter RF combining, an ADC, CP removal + FFT, and a per-subcarrier digital combiner. The design variables are the digital and RF precoders and combiners, under power, clipping, and emission-mask limits and with phase-shifter errors.
</div>

<div class="row mt-3" style="font-size: 0.9rem;">
  <div class="col-md-4 mb-2">
    <b>Notation</b><br>
    $\mathbf{H}_k^s$: channel to user $k$ on subcarrier $s$<br>
    $\mathbf{V}_{\mathrm{RF}}, \mathbf{V}_k^s$: RF / digital precoders<br>
    $\mathbf{U}_{\mathrm{RF},k}, \mathbf{U}_k^s$: RF / digital combiners
  </div>
  <div class="col-md-4 mb-2">
    <b>Key property (partial connection)</b><br>
    $\mathbf{V}_{\mathrm{RF}}^H\mathbf{V}_{\mathrm{RF}} = \tfrac{N_t}{N_{\mathrm{RF}}}\mathbf{I}_{N_{\mathrm{RF}}}$<br>
    so the power constraint simplifies
  </div>
  <div class="col-md-4 mb-2">
    <b>Design variables</b><br>
    $\mathbf{V}_k^s$ (digital, power-limited), $\mathbf{V}_{\mathrm{RF}}$ (RF, $|\cdot|=1$)<br>
    $\mathbf{U}_k^s$ (digital, unconstrained), $\mathbf{U}_{\mathrm{RF},k}$ (RF, $|\cdot|=1$)
  </div>
</div>

### Approach

The problem is to maximize the downlink sum-rate over all subcarriers. It is nonconvex in every variable. We developed a **WMMSE-based block coordinate descent (BCD)** algorithm that updates the blocks in Gauss-Seidel order:

- **Digital combiners and MSE weights:** closed-form updates.
- **Digital precoder:** with block-level precoding (precoders fixed over a scheduling block), the subproblem has a convex objective and three convex constraints: power, spectral mask, and clipping. A **three-block ADMM** splits it into smaller problems, each solved in closed form or by bisection.
- **RF precoder and RF combiners:** **coordinate descent** with a **closed-form update per phase shifter**. A **robust variant** maximizes the expected sum-rate under random phase errors $$\Delta\theta \sim \mathcal{N}(0, \sigma_e^2)$$.
- **Guarantees:** the inner solvers are theoretically supported, and the BCD gives a **monotonically convergent** regularized WMMSE objective under exact inner solves.

### Results

<div class="row justify-content-sm-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/hybrid_precoding/psd_vs_mask.png" title="Expected PSD under different emission masks" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b>Spectrum shaped to the mask.</b> The expected PSD of the transmitted 20&nbsp;MHz OFDM symbol stays under every enforced emission mask (red, dashed) from −60 to −20&nbsp;dBm/100&nbsp;kHz. The precoder trades in-band power and occupied width for compliance automatically.
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/hybrid_precoding/emission_and_rate.png" title="In-band power, OOB power, and sum-rate versus mask level" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>A tunable rate–emissions trade-off.</b> Going from a −60 to a −50&nbsp;dBm/100&nbsp;kHz mask raises OOB emissions from −48.9 to −39.1&nbsp;dBm and the sum-rate from 8.69 to 15.54&nbsp;bit/s/Hz (P<sup>s</sup>&nbsp;=&nbsp;25&nbsp;dBm, N<sub>RF</sub>&nbsp;=&nbsp;16).
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/hybrid_precoding/rate_vs_power.png" title="Sum-rate versus transmit power" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>When the mask, not power, is the bottleneck.</b> Under a −20&nbsp;dBm/100&nbsp;kHz mask the rate keeps scaling with power and RF chains, reaching about 60&nbsp;bit/s/Hz with 32 RF chains at 25&nbsp;dBm. Under −40&nbsp;dBm/100&nbsp;kHz it saturates beyond about 15&nbsp;dBm as the mask constraints become active.
        </div>
    </div>
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/hybrid_precoding/rf_benchmarks.png" title="Proposed RF design versus benchmarks" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Beats published RF designs.</b> At 25&nbsp;dBm, the closed-form phase-shifter updates deliver +10.5% (8 RF chains) and +14.5% (16 RF chains) more sum-rate than Wang et al.'s coordinate descent. They gain up to +6.2% over Riemannian conjugate gradient (RCG; 16 RF chains, 20&nbsp;dBm) and +30–66% over random phases across all powers.
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/hybrid_precoding/robust_ps_errors.png" title="Robust versus non-robust designs under phase-shifter errors" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Built for imperfect hardware.</b> With ideal phase shifters, the proposed design beats classical hybrid MMSE by 9.5–20%. With 30° phase-shifter errors, the robust update rules add 23–38% over the non-robust WMMSE design and 40–48% over non-robust hybrid MMSE (N<sub>RF</sub>&nbsp;=&nbsp;8).
        </div>
    </div>
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/hybrid_precoding/convergence.png" title="Convergence of the proposed BCD approach" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Fast, monotone convergence.</b> The sum-rate rises with every BCD iteration, and most of the gain arrives in the first ~10 iterations (25&nbsp;dBm per subcarrier, 64 subcarriers).
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/hybrid_precoding/wmmse_vs_power.png" title="WMMSE objective versus transmit power" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>The objective tracks the rate.</b> The WMMSE cost keeps falling with power under the −20&nbsp;dBm/100&nbsp;kHz mask and levels off once the −40&nbsp;dBm/100&nbsp;kHz mask binds, mirroring the sum-rate.
        </div>
    </div>
</div>

### Simulation testbed

Implemented in **Python** with an **object-oriented** design, using:

- **Numerical computing:** NumPy and SciPy on optimized BLAS/LAPACK
- **Channel modeling:** NVIDIA Sionna on TensorFlow
- **Parallelization:** multiprocessing (process pools across all CPU cores) and concurrent.futures
- **Plotting:** Matplotlib

**Size:** about 23,300 lines of Python across 44 modules, of which about 14,300 are code.

### Simulation parameters

- **Arrays and users:** 32-antenna BS; 4 users with 4 antennas and 2 RF chains each, sharing all subcarriers. Half-wavelength ULAs.
- **Band:** 28&nbsp;GHz carrier, 20&nbsp;MHz bandwidth, 64 subcarriers.
- **Geometry and channel:** users within a 4&nbsp;m radius, 400&nbsp;m from the BS. Frequency-selective Rician channel ($$\kappa$$&nbsp;=&nbsp;10&nbsp;dB) with 3GPP path loss and shadowing.
- **Noise:** −174&nbsp;dBm/Hz with an 8&nbsp;dB noise figure.
- **Constraints:** clipping limit $$\chi = 0.7$$. Clipping and mask constraints are met with at least 90% probability, and the mask is enforced from 10.01 to 20&nbsp;MHz on each side of the band.

_All results and figures are from arXiv:2509.07229v3. The architecture diagram is adapted from the authors' slides._
