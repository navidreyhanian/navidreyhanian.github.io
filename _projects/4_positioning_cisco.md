---
layout: page
title: Wireless Positioning at Cisco
description: GNSS, Wi-Fi FTM, AoA/PDoA, and UWB TDoA algorithms for access-point and client localization, plus RF front-end impairment analysis.
img: assets/img/projects/positioning/card_positioning.png
importance: 4
---

**Keywords:** GNSS · Kalman filtering · Gauss-Newton · Wi-Fi FTM · AoA/PDoA · phase unwrapping · UWB · TDoA · RF impairments · C/C++ · Python

I am a Wireless Systems Architect in Cisco's Enterprise Networking product group (at Cisco since 2022), developing signal processing algorithms for wireless networks. The work runs from algorithm design through production code in C/C++ and Python.

### Highlights

- **GNSS receiver.** Designed and developed a GNSS receiver that combines a Kalman filter with raw pseudorange measurements and Gauss-Newton optimization for more accurate and robust positioning.
- **AP geolocation from FTM + GNSS.** Created algorithms for real-time localization of access points that jointly process Wi-Fi Fine Time Measurement (FTM) ranging and GNSS signals, enabling dynamic tracking and precise geolocation.
- **Angle of arrival from phase differences.** Built an AoA estimation framework from Phase Difference of Arrival (PDoA) measurements across AP antennas, with phase unwrapping, memory, and ambiguity resolution to improve client and tag localization.
- **RF front-end impairment analysis.** Analyzed RF impairments of transmit front-end modules, including filtering, tuning, and adaptive power control, to meet FTM ranging accuracy requirements.
- **UWB and uplink TDoA positioning.** Designed clustering algorithms for AP grouping and a UWB client positioning engine for downlink TDoA networks. Led the design and development of an uplink TDoA tag positioning system.
- **Anchor-free localization.** Developed block coordinate descent algorithms for large-scale, rank-constrained anchor-free optimization, improving AP localization in GPS-denied environments.
- **Testing and debugging.** Hands-on performance testing of Wi-Fi, BLE, and UWB devices (throughput, coverage, reliability), plus debugging and performance tuning of C/C++ and Python code with tight latency requirements.

### Patents

Seven patent families with Cisco Technology, Inc. ([full list](/publications/#patents)):

- _Null-Steering Using Interference Alignment for Multi-Access Point (AP) Coordination Using Channel State Information_. **Granted:** [US 12,615,076 B2](https://patents.google.com/patent/US12615076B2/en).
- _Guided Anchor-Free Method for Access Point Location Determination_, [US 2026/0040026 A1](https://patents.google.com/patent/US20260040026A1/en)
- _Hybrid FiRa DL-TDoA and UWB Radar Mode_, [US 2026/0029524 A1](https://patents.google.com/patent/US20260029524A1/en)
- _Device Localization Independent of Clock Synchronization_, [US 2025/0220631 A1](https://patents.google.com/patent/US20250220631A1/en)
- _Adaptive Pilot Placement of Distributed-Tone Resource Units_, [US 2025/0202644 A1](https://patents.google.com/patent/US20250202644A1/en)
- _GNSS Driven Dynamic Partitioning of APs_, [US 2025/0093530 A1](https://patents.google.com/patent/US20250093530A1/en)
- _Joint GNSS-Terrestrial AP Geolocating_, [US 2025/0085441 A1](https://patents.google.com/patent/US20250085441A1/en)

Recognized as a Cisco **"Wireless Star."**
