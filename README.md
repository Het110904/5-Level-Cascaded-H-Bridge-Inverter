# Fault-Tolerant 5-Level CHB Inverter for EV Traction (LSPWM-PD)

## Overview
This repository contains a MATLAB/Simulink model of a 5-Level Cascaded H-Bridge (CHB) Inverter designed specifically for Electric Vehicle (EV) traction motors. 

The primary feature of this project is a custom **"Limp-Home" fault-tolerance logic**. If a hardware switch fails during operation, the system automatically isolates the damaged cell and dynamically transitions from a 5-level output to a 3-level output. This prevents a total system shutdown and allows the EV to continue moving safely. Additionally, an LC filter is implemented to ensure the output power quality strictly adheres to IEEE-519 harmonic standards.

## Overall System Specifications

| Parameter Category | Specific Parameter | Value |
| :--- | :--- | :--- |
| **Hardware Topology** | Inverter Configuration | Cascaded H-Bridge (8x IGBTs) |
| | Normal / Fault Levels | 5-Level (Healthy) / 3-Level (Faulted) |
| **Control Logic** | Modulation Technique | LSPWM-PD |
| | Carrier Switching Frequency | 4000 Hz |
| **Filter Design** | Passive LC Low-Pass | 5 mH, 150 µF |

## Simulink Architecture & Control Logic

### Main Inverter Build
*The complete simulation environment featuring the 5-level Cascaded H-Bridge setup, isolating circuit breakers, and traction motor load.*

![Main Architecture](5%20Level%20CHB/Entire%20model%20pic.jpg)

*A close-up view of the dual H-Bridge cells operating from isolated 100V DC sources.*

![H-Bridge Close Up](5%20Level%20CHB/Close%20up%20view%20of%20H%20bridges.jpg)

### Modulation Logic
*Level-Shifted Phase Disposition (LSPWM-PD) logic, utilizing four carrier waves stacked vertically against a 1.90 amplitude reference sine wave.*

![Control Logic](5%20Level%20CHB/Control%20Logic.jpg)

## Simulation Results: Dynamic Fault Bypass

The most critical test of this system is the dynamic cell bypass. The time-plot graphs below prove the transition is smooth and safe.
* **$t = 0.0$ to $0.5$ seconds:** The system operates in a healthy state, outputting a 5-level staircase ($\pm200$V peak).
* **$t = 0.5$ seconds:** A short-circuit fault is simulated. The system isolates the damaged hardware and instantly drops to a 3-level output ($\pm100$V peak).

![CHBI Scope](5%20Level%20CHB/CHBI%20Scope%20JPEG%20Version.jpg)

*Simulink time-plot showing the motor voltage (Red) and motor current (Blue) during the transition.*

![Time Plot Graph](5%20Level%20CHB/Time%20Plot%20Graph.tif)

*After passing through the LC filter, the load receives this smoothed continuous AC waveform across the fault transition.*

![Filtered Waveform](5%20Level%20CHB/Filtered%20Waveform%20(V_Out).tif)

### Drive Stability
*The Voltage vs. Current XY graph proves that when the car drops from normal driving into the emergency limp-home mode, the motor stays completely stable and does not lose control.*

![XY Graph](5%20Level%20CHB/XY%20Graph%20JPEG%20Version.jpg)

## Harmonic Analysis (THD)

Fast Fourier Transform (FFT) analysis was conducted to measure power quality and verify compliance with IEEE-519 standards (<5% THD).

### 1. Healthy State (5-Level)
*Before filtering, the raw 5-level staircase yields a natural THD of **30.59%**.*

![Raw Healthy THD](5%20Level%20CHB/Raw%20Staircase%2030.59%20%25.tif)

*After passing through the LC filter, the THD is successfully crushed to **2.24%**, passing strict grid and motor safety limits.*

![Filtered Healthy THD](5%20Level%20CHB/Pure%20Sine%20Wave%202.24%25.tif)

### 2. Faulted State "Limp-Home" (3-Level)
*During the emergency bypass, harmonic distortion spikes (Raw THD = **78.26%**) due to the loss of voltage levels.*

![Raw Faulted THD](5%20Level%20CHB/Raw%20Staircase%2078.26%20%25.tif)

*After passing through the LC filter, the post-fault THD is brought to **42.55%**. While higher than normal operation, it successfully maintains fundamental drive stability to allow the EV to limp home safely.*

![Filtered Faulted THD](5%20Level%20CHB/Limp%20Home%2042.55%20%25%20JPEG%20Version.jpg)
