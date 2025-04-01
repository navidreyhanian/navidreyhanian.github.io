---
layout: default
title: "Projects"
permalink: /projects/
---

# Projects

## Table of Contents

- [Joint Spectral and Spatial Precoding for Multi-User MIMO-OFDM Systems](#joint-spectral-and-spatial-precoding-for-multi-user-mimo-ofdm-systems)

---

- [Precoding for Uplink RIS-Assisted Cell-Free Massive MIMO-OFDM Systems with Hardware Impairments](#precoding-for-uplink-ris-assisted-cell-free-massive-mimo-ofdm-systems-with-hardware-impairments)


---

## Joint Spectral and Spatial Precoding for Multi-User MIMO-OFDM Systems {#joint-spectral-and-spatial-precoding-for-multi-user-mimo-ofdm-systems}

<img src="{{ site.baseurl }}/assets/img/MIMO_OFDM.jpg" alt="Figure 1: Hybrid MIMO OFDM systems." style="display: block; margin: 0 auto 20px auto; max-width:550px;">

In this paper, we study the true time delay (TDD)-assisted hybrid precoding design for MU-MIMO-OFDM systems with partially-connected transmitter structures under PAPR, clipping, and OOB emission constraints. We formulate the problem of user sum-rate maximization through the optimization of digital and RF precoders at the transmitter, digital and analog combiners at users, and the phase shifts applied via TDDs. The joint optimization problem is non-convex with respect to all variables, making it difficult to solve. We propose a WMMSE-based block coordinate descent (BCD) algorithm to efficiently solve the problem by alternatively optimizing different variable blocks. We separately discuss the optimization with respect to each block as follows. 

The subproblem with respect to the digital precoder is made of a convex objective function subject to one non-convex and three convex constraints. The non-convex constraint arises from the PAPR limitation, while the convex constraints are obtained from the maximum transmit budget, notching spectrum at specific frequencies, and signal amplitude clipping. User symbols are random; therefore, digital precoders are designed such that the above constraints are satisfied with high probability. We show that the non-convex constraint can be relaxed since the WMMSE method promotes precoders with low PAPR (depending on the oversampling factor). To solve the large dimensional subproblem with three constraints, we decompose it and propose a low-cost and scalable alternating direction method of multipliers (ADMM) to sequentially solve three smaller problems for which solutions can be obtained via closed-form expressions or bisection-searches. 

The subproblems with respect to partially-connected RF precoder and fully (or partially)-connected analog combiners at the transmitter and users, respectively, are non-convex, although the objective function is convex with respect to these variable blocks. This non-convexity is due to the fact that there is one unit modulus constraint for each phase shifter. To optimize the phase shifts, we propose a Riemannian manifold method with Polak-Ribiere step-size to ensure a monotonic decrease in the objective function and solve the problem to a local optimum. Moreover, we derive closed-form solutions for digital combiners at users.

The subproblem with respect to phase shifts by TDDs is non-convex and difficult to solve due to its trigonometric nature. A robust projected gradient descent method with adaptive step-size control and stagnation handling is designed. Each iteration calculates a normalized gradient direction and uses an Armijo condition line-search to identify step-sizes, enhancing performance in nonlinear landscapes. To address stagnation, the method uses local perturbations with increasing intensity.

In the proposed BCD algorithm, the above blocks are optimized alternatively with the Gauss-Seidel rule until all blocks converge. The convergence of the proposed method to solve each subproblem is theoretically supported as well as the overall BCD approach. Extensive simulation results are provided to demonstrate the efficiency of the proposed algorithms in reducing OOB emissions and enhancing achievable rates.

<img src="{{ site.baseurl }}/assets/img/TDD.jpg" alt="Figure 2: The achievable per-subcarrier sum-rate of the classical MMSE and WMMSE methods against the TDD-assisted WMMSE approach." style="display: block; margin: 0 auto 20px auto; max-width:450px;">

We compare the performance of our TDD-assisted WMMSE method against the classical WMMSE and MMSE methods in an MU scenario with four users, \(32\) subcarriers, \(N_t = 64\) transmit antennas, and \(N_r = 4\) receive antennas for different transmit power levelsin the above figure. The number of RF chains is increased from \(16\) to \(32\) in steps of \(8\). It is observed that the WMMSE method achieves a higher sum-rate compared to the MMSE algorithm, while it yields a lower sum-rate than the TDD-assisted WMMSE method.

## Precoding for Uplink RIS-Assisted Cell-Free Massive MIMO-OFDM Systems with Hardware Impairments {#precoding-for-uplink-ris-assisted-cell-free-massive-mimo-ofdm-systems-with-hardware-impairments}

<figure style="display: block; margin: 0 auto 20px auto; max-width:450px;">
  <img src="{{ site.baseurl }}/assets/img/Fixed_vs_Optimized.jpg" alt="Figure 3: Comparison of Optimization Schemes.">
  <figcaption>Figure 3: Comparison of Optimization Schemes.</figcaption>
</figure>

In this work, we investigate the maximization of the uplink sum-rate in fully centralized CF-mMIMO-OFDM networks by proposing a novel joint optimization framework. Our approach simultaneously optimizes user precoding, multi-RIS coefficients, and combining at APs while accounting for IQ imbalance (IQI) impairments at both UEs and APs. The non-convex precoding problem is tackled via a block coordinate descent (BCD) algorithm based on the weighted minimum mean square error (WMMSE) method, where subproblems for UE precoders and AP combiners are efficiently solved using bisection-search and closed-form solutions. Moreover, we introduce several reformulations of the multi-RIS coefficient subproblem, which are globally solved using a cost-effective gradient projection method (GPM) with adaptive step-size. The convergence of these methods is theoretically supported, and extensive numerical tests demonstrate that our approach outperforms heuristic methods that decouple the problems and ignore hardware impairments.

Figure 3 evaluates the efficacy of our proposed "Optimized" scheme against two alternatives for different receive antennas. The first alternative ("Random") employs fixed, randomly assigned RIS values, while the second ("Blind") uses the conventional uplink WMMSE approach that neglects IQI effects. Under a scenario with four users sharing twelve subcarriers, with path-loss exponents of 2.2 along UE–RIS–AP paths and 3.8 for UE–AP paths, two RISs deployed at (−8, 12, 4) and (8, 12, 4), and users distributed in a small area, our joint optimization yields a significantly higher average sum-rate—e.g., with \(N_r = 16\) and \(K = 4\) at 0 dBm, achieving 9.07 bit/s/Hz versus 6.14 bit/s/Hz for the random approach.
