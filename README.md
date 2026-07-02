# Design and Harmonic Analysis of a Fault-Tolerant 5-Level Cascaded H-Bridge Inverter using LSPWM-PD for EV Traction

<p align="center">

![MATLAB](https://img.shields.io/badge/MATLAB-R2024b-orange?style=for-the-badge)
![Simulink](https://img.shields.io/badge/Simulink-Model-blue?style=for-the-badge)
![IEEE-519](https://img.shields.io/badge/IEEE--519-THD%20PASS-brightgreen?style=for-the-badge)

</p>

---

## Overview

This repository presents a MATLAB/Simulink implementation of a **fault-tolerant 5-Level Cascaded H-Bridge (CHB) inverter** for EV traction applications using **Level Shifted PWM (Phase Disposition)**. A passive LC filter reduces harmonic distortion while a limp-home control strategy allows continued operation after a switch fault by reconfiguring the inverter into a 3-level topology.

---

# Repository Contents

- 📄 [APEC IA REPORT UPD.pdf](./5%20Level%20CHB/APEC%20IA%20REPORT%20UPD.pdf)
- 📄 [Tuning of LC filter with comparisions.pdf](./5%20Level%20CHB/Tuning%20of%20LC%20filter%20with%20comparisions.pdf)

---

# Complete Simulink Model

<p align="center">
<img src="./5%20Level%20CHB/Entire%20model%20pic.jpg" width="900">
</p>

---

# Cascaded H-Bridge Topology

<p align="center">
<img src="./5%20Level%20CHB/Close%20up%20view%20of%20H%20bridges.jpg" width="850">
</p>

---

# LSPWM-PD Control Logic

<p align="center">
<img src="./5%20Level%20CHB/Control%20Logic.jpg" width="900">
</p>

---

# Healthy and Faulted Output

<p align="center">
<img src="./5%20Level%20CHB/CHBI%20Scope%20JPEG%20Version.jpg" width="900">
</p>

The inverter operates as a healthy 5-level inverter and automatically transitions into a 3-level limp-home mode after a fault at **0.5 s**.

---

# LC Filter Output

<p align="center">
<img src="./5%20Level%20CHB/Filtered%20Waveform%20(V_Out)%20JPEG%20Version.jpg" width="900">
</p>

The passive LC filter (5 mH, 150 µF) reduces switching harmonics and produces a nearly sinusoidal output.

---

# FFT Analysis

## Healthy Output (Before Filter)

<p align="center">
<img src="./5%20Level%20CHB/Raw%20Staircase%2030.59%20%25%20JPEG%20Version.jpg" width="800">
</p>

**THD = 30.59 %**

---

## Faulted Output (Before Filter)

<p align="center">
<img src="./5%20Level%20CHB/Raw%20Staircase%2078.26%20%25%20JPEG%20Version.jpg" width="800">
</p>

**THD = 78.26 %**

---

## Healthy Output (After Filter)

<p align="center">
<img src="./5%20Level%20CHB/Pure%20Sine%20Wave%202.24%25%20JPEG%20Version.jpg" width="800">
</p>

**THD = 2.24 % (IEEE-519 Compliant)**

---

## Faulted Output (After Filter)

<p align="center">
<img src="./5%20Level%20CHB/Limp%20Home%2042.55%20%25%20JPEG%20Version.jpg" width="800">
</p>

**THD = 42.55 %**

---

# Motor Response

## Voltage & Current

<p align="center">
<img src="./5%20Level%20CHB/Time%20Plot%20Graph%20JPEG%20Version.jpg" width="900">
</p>

---

## XY Characteristics

<p align="center">
<img src="./5%20Level%20CHB/XY%20Graph%20JPEG%20Version.jpg" width="850">
</p>

---

# Simulation Specifications

| Parameter | Value |
|-----------|-------|
| Software | MATLAB / Simulink R2024b |
| Solver | ode23tb |
| PWM | LSPWM-PD |
| Switching Frequency | 4000 Hz |
| Fundamental Frequency | 50 Hz |
| Modulation Index | 0.95 |
| DC Sources | 2 × 100 V |
| Filter | 5 mH + 150 µF |
| Load | Series RL |

---

# Performance Summary

| Condition | THD |
|-----------|----:|
| Healthy (Before Filter) | 30.59 % |
| Healthy (After Filter) | **2.24 %** |
| Faulted (Before Filter) | 78.26 % |
| Faulted (After Filter) | 42.55 % |

---

# Key Features

- 5-Level Cascaded H-Bridge Inverter
- Fault-Tolerant Limp-Home Operation
- LSPWM-PD Control
- LC Harmonic Filter
- FFT Harmonic Analysis
- IEEE-519 Compliance
- EV Traction Drive Application

---

# Software Required

- MATLAB R2024b
- Simulink
- Simscape Electrical

---

# Author

**Het Kansara**

Department of Electrical Engineering

---

⭐ If you found this project useful, consider starring the repository.
