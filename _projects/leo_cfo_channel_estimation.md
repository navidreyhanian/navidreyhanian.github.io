---
layout: page
title: Joint CFO and Channel Estimation for Dual-Polarized Hybrid LEO Multiuser MIMO–OFDM Uplinks
description: Acquiring each user's frequency offset and channel on a satellite broadband uplink with large Doppler, hybrid dual-polarized arrays, and imperfect low-cost terminals.
img: assets/img/projects/leo/card_leo_uplink.png
importance: 3
---

**Manuscript:** N. Reyhanian, P. Ramezani, A. Kalantari, and E. Björnson, "Joint CFO and Channel Estimation for Dual-Polarized Hybrid LEO Multiuser MIMO–OFDM Uplinks," journal manuscript. _The estimation method will be shared after publication. This page describes the problem, why it matters in practice, and the simulation results._

<div class="row justify-content-sm-center">
    <div class="col-md-8 mt-3">
        {% include figure.liquid loading="eager" path="assets/img/projects/leo/card_leo_uplink.png" title="Dual-polarized LEO multiuser uplink" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<div class="row text-center mt-4 mb-1">
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">550&nbsp;km</div>
      <div style="font-size: 0.85rem;">low-Earth orbit; the satellite moves at about 7.6&nbsp;km/s</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">800 ports</div>
      <div style="font-size: 0.85rem;">20×20 dual-polarized satellite array, served by 200 RF chains per polarization</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">10 users</div>
      <div style="font-size: 0.85rem;">small dual-polarized terminals sharing a 20&nbsp;MHz, 128-subcarrier uplink at 2&nbsp;GHz</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">3 pilot symbols</div>
      <div style="font-size: 0.85rem;">per block, with no coarse Doppler pre-compensation applied first</div>
    </div>
  </div>
</div>
<p style="font-size: 0.8rem; text-align: center; opacity: 0.8;">Scenario studied in the manuscript.</p>

### The problem

Low-Earth-orbit constellations promise broadband coverage with far shorter propagation delay than geostationary or medium-Earth-orbit systems. Massive MIMO with OFDM can supply the array gain, spatial multiplexing, and frequency-domain equalization that a broadband multiuser uplink needs. But before the satellite can decode anything, its receiver has to learn two things for every user at once: the **carrier frequency offset (CFO)** and the **channel**. On a LEO uplink, getting both right is unusually hard.

- **Large, user-specific frequency offsets.** The satellite moves at about 7.6&nbsp;km/s, which creates Doppler shifts of tens of kHz at a 2&nbsp;GHz carrier, and the channel ages quickly. Satellite ephemeris and terminal geometry give an approximate Doppler value for pre-compensation, but uncertainty in each terminal's velocity limits its accuracy. The total offset also includes the oscillator mismatch between each terminal and the satellite. So every user arrives with its own net offset, and navigation forecasts can narrow the range but can't pin down the oscillator part.
- **Offsets and channels are entangled.** An uncorrected offset rotates the phase from one OFDM symbol to the next and leaks energy between subcarriers (intercarrier interference). The offsets and the channel gains show up together in the same received pilots, so they should be recovered jointly, not one after the other.
- **Hybrid, dual-polarized arrays hide the channel.** Large apertures favor hybrid RF-digital processing to cut the number of RF chains, but then the receiver only sees the channel through compressed, frequency-flat RF combiners. Dual polarization adds diversity and frequency reuse without a larger aperture. The cost is that each user's channel now couples four co- and cross-polarized components.
- **Wideband, frequency-selective propagation.** The channel changes across the band, so it can't be described by a single gain per antenna.
- **Weak pilots on a tight budget.** The long link makes received pilots weak, so acquisition has to be efficient in both energy and computation. Estimating offsets and channels jointly lets each pilot serve both tasks. That allows fewer, lower-power pilots than classical sequential schemes that repeat pilots, estimate and correct the offset, and only then estimate the channel.
- **Imperfect user hardware.**
  - To overcome the path loss, the terminal's power amplifier is driven near saturation, where its nonlinear distortion becomes significant.
  - Transmit and receive IQ imbalance split each pilot into a direct path and a mirror-image path.
  - Receive IQ imbalance also makes the noise after combining colored and improper.
  - All of these act on the same pilot block, so they must be handled together with the offsets and channels rather than corrected one by one.

### Why it is practical

- **It matches how satellite broadband is built.** It assumes large hybrid arrays on the satellite (fewer RF chains, less power and cost), dual polarization for reuse, and small, low-cost user terminals with 2×2 dual-polarized arrays.
- **It does not depend on precise Doppler pre-compensation.** The offsets are estimated with no coarse Doppler correction applied first, which fits links where terminal velocity is uncertain and oscillators drift. Navigation-based Doppler forecasts become optional aids, not requirements.
- **It saves pilots and terminal energy.** Fewer and weaker pilots mean less uplink overhead and longer battery life, which matters most when the link budget is tight.
- **It models the impairments real terminals have.** These are power-amplifier nonlinearity with no input back-off, transmit and receive IQ imbalance, and colored, improper noise. They are part of the model, not assumed away.
- **It is multiuser at scale.** Ten users in a 50&nbsp;km service region are handled at the same time, each with its own offset and channel.
- **It sits at the front of the modem.** Frequency synchronization and channel estimation are the first blocks of the satellite's uplink receiver. They feed equalization and detection, so their accuracy limits everything downstream.

### Scenario studied

- **Satellite:** 550&nbsp;km orbit at 53° inclination, moving at about 7.6&nbsp;km/s. 20×20 dual-polarized planar array with 200 RF chains per polarization and 31&nbsp;dB antenna gain.
- **Users:** 10 terminals within a 50&nbsp;km region.
  - Each is a 2×2 dual-polarized array with two RF chains and two pilot streams per polarization, and 6&nbsp;dB antenna gain.
  - Transmit power is 10–50&nbsp;dBm per user and polarization.
- **Waveform:** 2&nbsp;GHz carrier, 20&nbsp;MHz bandwidth, 128 subcarriers with a 4-sample cyclic prefix, and 3 pilot OFDM symbols per block.
- **Propagation:**
  - Frequency-selective channel with taps spanning 0.15&nbsp;µs.
  - Cross-polar power 10&nbsp;dB below co-polar.
  - 2&nbsp;dB ionospheric loss and 3&nbsp;dB lognormal shadowing.
  - Noise at −174.6&nbsp;dBm/Hz with a 5&nbsp;dB noise figure.
- **Impairments:**
  - Per-user net frequency offsets of up to 15% of the subcarrier spacing, with no coarse Doppler correction applied beforehand.
  - Transmit and receive IQ imbalance.
  - A cubic power-amplifier nonlinearity with no input back-off.

### Simulation results

<div class="row text-center mt-3 mb-1">
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">35.6&nbsp;dB</div>
      <div style="font-size: 0.85rem;">lower channel NMSE than SAGE-ECM at 50&nbsp;dBm (8.4&nbsp;dB at 40&nbsp;dBm)</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">135×</div>
      <div style="font-size: 0.85rem;">lower CFO error than SAGE-ECM at 50&nbsp;dBm, and 244× lower than Taylor CFO + GD</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">≈7&nbsp;Hz</div>
      <div style="font-size: 0.85rem;">residual CFO error at 50&nbsp;dBm (4.3×10<sup>−5</sup> of the 156&nbsp;kHz subcarrier spacing)</div>
    </div>
  </div>
  <div class="col-6 col-md-3 mb-3">
    <div class="card p-3 h-100">
      <div style="font-size: 1.7rem; font-weight: 700; color: var(--global-theme-color);">3–4 iterations</div>
      <div style="font-size: 0.85rem;">to settle, for 5 to 15 simultaneous users</div>
    </div>
  </div>
</div>
<p style="font-size: 0.8rem; text-align: center; opacity: 0.8;">Values read from the plotted data in the manuscript's figures; 10 users unless noted.</p>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/leo/compare_solvers_NMSE_d.png" title="Channel NMSE versus transmit power for different estimators" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>More accurate channels than the competing estimators.</b> At 50&nbsp;dBm the channel NMSE reaches −61.1&nbsp;dB, compared with −30.5&nbsp;dB for joint ESPRIT, −25.6&nbsp;dB for SAGE-ECM, and −18.2&nbsp;dB for Taylor CFO + GD. Accuracy matches LM-EM, which jointly solves a dense <i>K</i>×<i>K</i> system and is harder to scale with the number of users.
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/leo/compare_solvers_RMSE_eps.png" title="Normalized CFO RMSE versus transmit power for different estimators" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Frequency offsets pinned down to a few hertz.</b> The normalized CFO error falls to 4.3×10<sup>−5</sup> at 50&nbsp;dBm, about 7&nbsp;Hz. The competing estimators level off: SAGE-ECM stays 135× higher and Taylor CFO + GD 244× higher.
        </div>
    </div>
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/leo/convergence_K.png" title="Channel NMSE and CFO RMSE versus iteration" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Fast convergence.</b> Channel NMSE drops sharply over the first two updates and settles by the third or fourth, with the CFO error falling alongside it, for 5 to 15 users (30&nbsp;dBm).
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/leo/num_users_RMSE_eps.png" title="CFO RMSE versus transmit power for different user loads" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Insensitive to user load.</b> For 2 to 15 users, the CFO error curves overlap within their error bars and fall steadily with power. The channel NMSE curves stay within about 0.4&nbsp;dB of one another.
        </div>
    </div>
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/leo/compare_iq_imbalance_NMSE_d.png" title="Channel NMSE under different IQ imbalance profiles" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Robust to IQ imbalance: channel.</b> Low and high IQ imbalance at the transmitter, the receiver, or both change the channel NMSE only slightly. Most of the change comes from transmit imbalance at low power.
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/leo/compare_iq_imbalance_RMSE_eps.png" title="CFO RMSE under different IQ imbalance profiles" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Robust to IQ imbalance: frequency offset.</b> Receive IQ imbalance has almost no visible effect. Transmit IQ imbalance, which also distorts the power-amplifier input, mildly degrades both metrics at low power.
        </div>
    </div>
</div>

<div class="row">
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/leo/compare_ber_multi_seed_pilot16QAM.png" title="BER with 16-QAM pilots" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>End-to-end link check, 16-QAM pilots.</b> With an ICI-aware LMMSE receiver, estimated-CSI BER comes close to true-CSI BER at 40&nbsp;dBm for both 4-QAM and 16-QAM data. With ideal power amplifiers, the estimated- and true-CSI curves coincide at 50&nbsp;dBm.
        </div>
    </div>
    <div class="col-md-6 mt-3">
        {% include figure.liquid path="assets/img/projects/leo/compare_ber_multi_seed_pilot4QAM.png" title="BER with 4-QAM pilots" class="img-fluid rounded z-depth-1" zoomable=true %}
        <div class="caption">
            <b>Pilot design matters.</b> Constant-modulus 4-QAM pilots leave a larger gap to true CSI than 16-QAM pilots. With the nonlinear PA and no back-off, BER rises from 40 to 50&nbsp;dBm even with true CSI: 10&nbsp;dB more drive raises the cubic distortion by about 30&nbsp;dB.
        </div>
    </div>
</div>

### Simulation testbed

Implemented in **Python** with an **object-oriented** design, using:

- **Numerical computing:** NumPy and SciPy on optimized BLAS/LAPACK, with threadpoolctl to control BLAS threading
- **GPU acceleration (optional):** CuPy
- **Parallelization:** multiprocessing, concurrent.futures, threading, and joblib
- **Plotting:** Matplotlib

**Size:** about 64,400 lines of Python across 97 files, of which about 36,500 are code.
