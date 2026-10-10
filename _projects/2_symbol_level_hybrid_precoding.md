---
layout: page
title: Symbol-Level Mask-Compliant Hybrid Precoding for Multi-User MIMO-OFDM Systems
description: Hybrid precoding that guarantees spectral-mask and clipping compliance for every transmitted OFDM symbol and tolerates phase-shifter errors. mmWave MU-MIMO-OFDM, 28 GHz.
img: assets/img/projects/symbol_level/card_mask_compliant_beams.png
importance: 2
---

**Paper:** N. Reyhanian, P. Ramezani, and E. Björnson, "Symbol-Level Mask-Compliant Hybrid Precoding for Multi-User MIMO-OFDM Systems," [arXiv:2604.17853](https://arxiv.org/abs/2604.17853v1) ([PDF](https://arxiv.org/pdf/2604.17853v1)).

<div class="row text-center mt-4 mb-1">
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">65–83×</div>
      <div style="font-size: 0.85rem;">lower sum-MSE than hybrid ZF/MRT, with or without frequency notching</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">46–60&nbsp;dB</div>
      <div style="font-size: 0.85rem;">by which ZF/MRT spectra, even with notching, overshoot the emission mask that this design meets</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">up to 16%</div>
      <div style="font-size: 0.85rem;">lower sum-MSE than RCG, and up to 6.6% lower than Wang et al.'s RF design</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">−69.25&nbsp;dBm</div>
      <div style="font-size: 0.85rem;">out-of-band emissions under a flat −90&nbsp;dBm/100&nbsp;kHz mask, with in-band power held at ≈6.5&nbsp;dBm</div>
    </div>
  </div>
</div>
<p style="font-size: 0.8rem; text-align: center; opacity: 0.8;">Gains computed from the plotted data in the paper's figures. Setup: 28&nbsp;GHz, 20&nbsp;MHz, 64 subcarriers, 32-antenna BS, 4 users × 4 antennas, 64-QAM; 8 or 16 RF chains.</p>

### Why it matters for modem and RF system design

- **Guaranteed compliance, not best-effort suppression.** Many existing designs simply push out-of-band emissions down as far as they can. This design enforces the clipping limit and the emission mask for **every transmitted symbol tuple**, so the spectrum is guaranteed to meet whatever mask is required.
- **Per-symbol peak control.** OFDM peaks drive clipping, distortion, and spectral regrowth. The per-antenna amplitude limit is enforced on every realized waveform, not on average.
- **A receiver split that matches real hardware.** The transmitter's digital precoders adapt to each symbol tuple. The users' analog and digital combiners are symbol-agnostic and are updated from batch statistics.
- **Scalable and impairment-aware.** A four-block ADMM uses closed-form or bisection steps that parallelize across antennas or subcarriers. Each phase shifter gets a closed-form update, with a robust version for phase errors. Convergence and complexity are analyzed.

### System architecture

<div class="row justify-content-sm-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/symbol_level/architecture.png" title="Symbol-level partially-connected MU-MIMO-OFDM architecture" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b>Symbol-level hybrid MU-MIMO-OFDM chain.</b> For each realized symbol tuple, the AP's digital precoder V<sub>k</sub><sup>s</sup>(ω) shapes the waveform so that the power, per-antenna clipping, and emission-mask limits all hold. IFFT + CP, DACs, and RF chains drive one phase shifter per antenna. Each user applies RF combining, ADCs, CP removal + FFT, and a symbol-agnostic digital combiner. The design minimizes the sum-MSE across users and subcarriers.
</div>

### Approach

The design minimizes the downlink **sum mean-squared error** over all users and subcarriers. The problem is nonconvex in every variable, so we developed an **MMSE-based block coordinate descent (BCD)** algorithm that updates the blocks in Gauss-Seidel order:

- **Digital precoder (per symbol tuple):** a large convex problem constrained by the power budget, per-antenna mask and clipping limits, and the linear relations between frequency- and time-domain, per-antenna, and per-RF-chain variables. A **four-block ADMM** solves it without forming the precoders explicitly. Every sub-step has a closed form or a bisection search, and several run in parallel across antennas or subcarriers.
- **RF precoder and analog combiners:** **coordinate descent** with a **closed-form update per phase shifter**. **Robust variants** minimize the expected MSE under random phase errors.
- **Digital combiners:** closed-form MMSE updates computed from batch statistics, so they don't depend on individual symbols.
- **Guarantees:** the regularized objective is monotonically nonincreasing and convergent, and its accumulation points are coordinatewise stationary. The per-block complexity is derived.

### Results

<div class="row justify-content-sm-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/symbol_level/psd_masks.png" title="PSD under five emission masks" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b>Compliant with every mask.</b> The transmitted PSD at antenna 1 stays under five different emission masks. They range from a relaxed −60&nbsp;→&nbsp;−70&nbsp;dBm/100&nbsp;kHz ramp to a flat −90&nbsp;dBm/100&nbsp;kHz limit.
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/symbol_level/psd_vs_zf_mrt.png" title="PSD of the proposed design versus ZF and MRT" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Where conventional precoders fail the mask.</b> Hybrid ZF and MRT, including Taheri et al.'s frequency notching, leave out-of-band sidelobes 46–60&nbsp;dB above the mask. The proposed design keeps the spectrum under it (N<sub>RF</sub>&nbsp;=&nbsp;16, P<sup>s</sup>&nbsp;=&nbsp;30&nbsp;dBm).
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/symbol_level/mse_vs_zf_mrt.png" title="Sum-MSE of the proposed design versus ZF and MRT" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Lower emissions and far lower error.</b> From 15 to 35&nbsp;dBm, the sum-MSE is 65–83× lower than all four ZF/MRT baselines. The gain comes from jointly optimizing the precoders together with each user's combiners.
        </div>
    </div>
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/symbol_level/rf_benchmarks.png" title="Proposed RF design versus benchmarks" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Beats published RF designs.</b> The proposed design has the lowest sum-MSE at every power and RF-chain count. It is up to 15.8% below Riemannian conjugate gradient (RCG), up to 6.6% below Wang et al.'s coordinate descent, and 30–60% below random phases.
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/symbol_level/robust_ps_errors.png" title="Robust versus non-robust designs under phase-shifter errors" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Built for imperfect hardware.</b> With phase-shifter errors of 3°, 6°, and 9°, the robust update rules lower the sum-MSE by 5–14% compared with the non-robust design at every tested power (N<sub>RF</sub>&nbsp;=&nbsp;16).
        </div>
    </div>
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/symbol_level/emission_and_mse.png" title="In-band power, out-of-band power, and sum-MSE versus mask" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Emissions on demand.</b> Moving from Mask 1 to Mask 5 drives out-of-band emissions from −46.93 to −69.25&nbsp;dBm, while in-band power stays at 6.49–6.65&nbsp;dBm. The cost is a sum-MSE rise from 0.0654 to 0.198 (P<sup>s</sup>&nbsp;=&nbsp;25&nbsp;dBm, N<sub>RF</sub>&nbsp;=&nbsp;16).
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/symbol_level/evm_vs_subcarrier.png" title="Per-user EVM versus subcarrier" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Where the mask bites.</b> Per-user EVM is lowest on the central subcarriers and rises at the band edges, where tighter masks demand the strongest suppression.
        </div>
    </div>
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/symbol_level/ser_vs_power.png" title="Per-subcarrier SER versus transmit power" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Link-level check.</b> Per-subcarrier symbol-error rate for 16- to 1024-QAM. The SER falls with transmit power and with lower modulation order.
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/symbol_level/convergence.png" title="Convergence of the proposed BCD approach" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Fast, monotone convergence.</b> The sum-MSE falls by about two orders of magnitude from initialization within 20–30 BCD iterations (25&nbsp;dBm per subcarrier, 64 subcarriers).
        </div>
    </div>
</div>

<div class="row justify-content-sm-center">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/symbol_level/mse_vs_power.png" title="Sum-MSE versus transmit power for different RF-chain counts" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Scales with power and RF chains.</b> The sum-MSE decreases with transmit power and with more RF chains (8, 16, 32).
        </div>
    </div>
</div>

### Simulation testbed

Implemented in **Python** with an **object-oriented** design, using:

- **Numerical computing:** NumPy and SciPy on optimized BLAS/LAPACK, plus scikit-sparse for sparse Cholesky factorizations
- **Channel modeling:** NVIDIA Sionna on TensorFlow
- **Parallelization:** multiprocessing (process pools across all CPU cores), concurrent.futures, and threading
- **Plotting:** Matplotlib

**Size:** about 30,500 lines of Python across 46 modules, of which about 19,000 are code.

### Simulation parameters

- **Arrays and users:** 32-antenna BS; 4 users with 4 antennas, 2 RF chains, and 2 streams each, sharing all subcarriers. Half-wavelength ULAs.
- **Band:** 28&nbsp;GHz carrier, 20&nbsp;MHz bandwidth, 64 subcarriers.
- **Symbols:** 64-QAM unless stated, with batches of 30 symbol realizations.
- **Geometry and channel:** users within a 4&nbsp;m radius, 100&nbsp;m from the BS. Frequency-selective Rician channel ($$\kappa$$&nbsp;=&nbsp;10&nbsp;dB) with 3GPP path loss and shadowing.
- **Noise:** −174&nbsp;dBm/Hz with an 8&nbsp;dB noise figure.
- **Constraints:** clipping limit $$\chi = 3$$. The default mask is inactive below 10.01&nbsp;MHz, ramps from −70 to −80&nbsp;dBm/100&nbsp;kHz between 10.01 and 12.5&nbsp;MHz, and is flat at −80&nbsp;dBm/100&nbsp;kHz beyond that. It is enforced at 90 frequency samples per side.

_All results and figures are from arXiv:2604.17853v1. The system diagram is adapted from the authors' slides._
