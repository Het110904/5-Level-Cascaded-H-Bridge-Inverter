# Fault-Tolerant 5-Level CHB Inverter Drive for EV Traction

## Overview
What happens when an electric vehicle's inverter fails on the highway? Usually, it results in a sudden, complete loss of power and a stalled car. This project was built to tackle that exact problem. 

This repository contains the design, simulation, and harmonic analysis of a robust 5-level Cascaded H-Bridge (CHB) inverter built specifically for EV traction motors. Developed in MATLAB/Simulink, this drive focuses on two major goals: delivering exceptionally clean power during normal driving conditions, and keeping the vehicle moving even if critical hardware breaks.

---

## The Core Innovation: "Limp-Home" Mode
The standout feature of this project is its custom fault-tolerant safety logic. 

If a hardware switch fails while driving, the EV does not just die on the road. Instead, the logic instantly detects the fault, isolates the broken component, and smoothly shifts the system from a 5-level output down to a 3-level backup mode. 

This allows the car to safely "limp home." The transition is designed to be completely seamless, preventing violent power jerks that could physically damage the traction motor during an emergency.

---

## System Architecture & Control Logic
The normal 5-level power output is achieved using 8 main Insulated-Gate Bipolar Transistors (IGBTs) split across two H-Bridges. This topology is highly efficient, delivering clean power without making the physical hardware unnecessarily complicated.

Instead of relying on pre-built software blocks, the Level-Shifted Pulse Width Modulation - Phase Disposition (LSPWM-PD) control logic was written entirely from scratch. To prevent calculation glitches during high-speed 4000 Hz switching, the modulation index was finely tuned to exactly 0.95, with the sine reference amplitude set to 1.90.

### Entire Simulink Model
![Entire Model](5%20Level%20CHB/Entire%20model%20pic.jpg)

### Close-Up: The 8-IGBT H-Bridges
![H-Bridge Close Up](5%20Level%20CHB/Close%20up%20view%20of%20H%20bridges.jpg)

### Custom LSPWM-PD Control Logic
![Control Logic](5%20Level%20CHB/Control%20Logic.jpg)

---

## Output Transitions & Motor Stability

### 1. Healthy vs. Faulted Output Transition
Below is the raw voltage output when a short circuit fault is simulated at exactly 0.5 seconds. You can see the system safely and smoothly drop from 5 distinct levels (+200V, +100V, 0, -100V, -200V) down to 3 levels (+100V, 0, -100V).

![CHBI Scope](5%20Level%20CHB/CHBI%20Scope%20JPEG%20Version.jpg)

### 2. Proving Motor Stability During Failure
To mathematically prove the motor does not lose control during the sudden drop into limp-home mode, voltage and current sensors were mapped to an XY Phase Plane graph. These plots confirm the motor remains completely stable, responsive, and safe throughout the transition.

![XY Graph](5%20Level%20CHB/XY%20Graph%20JPEG%20Version.jpg)
![Time Plot Graph](5%20Level%20CHB/Time%20Plot%20Graph%20JPEG%20Version.jpg)

---

## LC Filter Design & Harmonic Analysis (THD)
To make this drive viable for the real world, it needs to pass strict power quality standards. 

Standard calculations suggested an inductance of 3.3 mH and a capacitance of 48 uF for a 400 Hz resonant target. However, to aggressively clean up the power, this passive LC low-pass filter was intentionally over-specced to **L = 5 mH** and **C = 150 uF**. 

This aggressive tuning shifted the actual resonant frequency down to roughly 184 Hz. As a result, the filter provides massive signal attenuation (-53.5 dB) at the switching frequency. By applying it, the Total Harmonic Distortion (THD) was crushed down to just **2.24%**, officially passing the strict IEEE-519 industry limit of <5%.

Here is the pure sine wave generated after the raw voltage passes through the custom LC filter:

![Filtered Waveform](5%20Level%20CHB/Filtered%20Waveform%20(V_Out)%20JPEG%20Version.jpg)

### Fast Fourier Transform (FFT) Data

**Healthy State (5-Level)**
* Raw Unfiltered THD: **30.59%**
* Filtered Pure Power THD: **2.24%**

![Raw 5-Level THD](5%20Level%20CHB/Raw%20Staircase%2030.59%20%25%20JPEG%20Version.jpg)
![Filtered 5-Level THD](5%20Level%20CHB/Pure%20Sine%20Wave%202.24%25%20JPEG%20Version.jpg)

**Limp-Home State (3-Level)**
* Raw Faulted THD: **78.26%**
* Filtered Faulted THD: **42.55%**

![Raw 3-Level THD](5%20Level%20CHB/Raw%20Staircase%2078.26%20%25%20JPEG%20Version.jpg)
![Filtered 3-Level THD](5%20Level%20CHB/Limp%20Home%2042.55%20%25%20JPEG%20Version.jpg)

---

## Technical Specifications 

| Parameter | Value / Details |
| :--- | :--- |
| **Simulation Platform** | MATLAB/Simulink R2024b (Ode23tb Solver) |
| **Fundamental Frequency** | 50 Hz |
| **Carrier Switching Frequency** | 4000 Hz |
| **Hardware Topology** | 8x IGBTs (2x Cascaded H-Bridges) |
| **Modulation Strategy** | LSPWM-PD |
| **Amplitude Modulation Index** | 0.95 |
| **Filter Resonant Frequency** | ~184 Hz |
| **Output Power** | ~1.81 kW |
| **Load (Traction Motor Stator)** | Series RL (10 Ohms, 5 mH) |

---

## Project Documentation
For an in-depth look at the mathematics, filter tuning process, and comprehensive system analysis, please refer to the project reports:
* [APEC IA Project Report](5%20Level%20CHB/APEC%20IA%20REPORT%20UPD.pdf)
* [LC Filter Tuning & Mathematical Comparisons](5%20Level%20CHB/Tuning%20of%20LC%20filter%20with%20comparisions.pdf)

---

## How to Run the Simulation
1. Clone this repository.
2. Open MATLAB R2024b (or a compatible newer version).
3. Open the `hetAPECIAPROJECTMAIN.slx` file located in the root directory.
4. Ensure the solver is set to `ode23tb` with a max step size of `0.0001s`.
5. Run the simulation to view the healthy state, or manually trigger the circuit breaker block to observe the limp-home fault transition.

---
*Developed by **Het Kansara** (23BEE011) | Advanced Power Electronic Converters IA Project | Even Semester 2026*
