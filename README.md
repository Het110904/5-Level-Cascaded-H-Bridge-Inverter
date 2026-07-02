# ⚡ Fault-Tolerant 5-Level CHB Inverter Drive for EV Traction

What happens when an electric vehicle's inverter fails on the highway? Usually, it results in a total loss of power and a stalled car. This project tackles that exact problem. 

This repository contains the design, simulation, and harmonic analysis of a robust 5-level Cascaded H-Bridge (CHB) inverter built specifically for EV traction motors. Developed in MATLAB/Simulink, this drive prioritizes two crucial elements: delivering ultra-clean power during normal driving, and keeping the car moving if the hardware breaks.

---

## 🚀 The Core Innovation: "Limp-Home" Mode
The standout feature of this project is its custom fault-tolerant safety logic. If a hardware switch fails while driving, the EV doesn't just die on the road. Instead, the logic instantly catches the fault, isolates the broken component, and smoothly shifts the system from a 5-level output down to a 3-level backup. 

This allows the car to safely "limp home". The transition is designed to be seamless, preventing violent power jerks that could physically damage the motor during an emergency. 

---

## 🛠️ System Architecture & Control Logic
The 5-level power output is achieved using 8 main Insulated-Gate Bipolar Transistors (IGBTs) split across two H-Bridges. It's a highly efficient topology that delivers clean power without making the physical hardware unnecessarily complicated.

Instead of relying on pre-built software blocks, the Level-Shifted Pulse Width Modulation - Phase Disposition (LSPWM-PD) logic was written entirely from scratch. To prevent calculation glitches during high-speed (4000 Hz) switching, the modulation index was finely tuned to exactly 0.95 and the sine reference amplitude to 1.90.

### Entire Simulink Model
![Entire Model](5%20Level%20CHB/Entire%20model%20pic.jpg)

### Close-Up: The 8-IGBT H-Bridges
![H-Bridge Close Up](5%20Level%20CHB/Close%20up%20view%20of%20H%20bridges.jpg)

### Custom LSPWM-PD Control Logic
![Control Logic](5%20Level%20CHB/Control%20Logic.jpg)

---

## 🧲 LC Filter Design & The Math Behind the Magic
To make this drive viable for the real world, it needs to pass strict power quality standards. A significant amount of time was spent tuning a passive LC low-pass filter to clean up the raw voltage. 

While standard calculations suggested an inductance of 3.3 mH and a capacitance of 48 µF for a 400 Hz resonant target, this filter was intentionally over-specced to **L = 5 mH** and **C = 150 µF**. 

This aggressive tuning shifted the actual resonant frequency down to ~184 Hz. The result? A massive signal attenuation (-53.5 dB) at the switching frequency, which is exactly why the final power output is exceptionally clean.

---

## 📊 Results & Hardware Stability Proof

### 1. The Output Transition (Healthy vs. Faulted)
Below is the voltage output when a short circuit fault occurs at T = 0.5 seconds. The system safely and smoothly drops from 5 levels (+200V, +100V, 0, -100V, -200V) to 3 levels (+100V, 0, -100V).

![CHBI Scope](5%20Level%20CHB/CHBI%20Scope%20JPEG%20Version.jpg)

Here is the pure sine wave generated after that raw voltage passes through the custom LC filter:

![Filtered Waveform](5%20Level%20CHB/Filtered%20Waveform%20(V_Out)%20JPEG%20Version.jpg)

### 2. Proving Motor Stability
To mathematically prove the motor doesn't lose control during the sudden drop into limp-home mode, voltage and current sensors were mapped to an XY Phase Plane graph. These plots confirm the motor remains completely stable and responsive throughout the transition.

![XY Graph](5%20Level%20CHB/XY%20Graph%20JPEG%20Version.jpg)
![Time Plot Graph](5%20Level%20CHB/Time%20Plot%20Graph%20JPEG%20Version.jpg)

### 3. Fast Fourier Transform (FFT) Harmonic Analysis
The LC filter performs exceptionally well. By applying it, the Total Harmonic Distortion (THD) was crushed down to just **2.24%**—officially passing the strict IEEE-519 industry limit of <5%. 

**Healthy State (5-Level)**
* Raw Unfiltered THD: **30.59%** * Filtered Pure Power THD: **2.24%** ![Raw 5-Level THD](5%20Level%20CHB/Raw%20Staircase%2030.59%20%25%20JPEG%20Version.jpg)
![Filtered 5-Level THD](5%20Level%20CHB/Pure%20Sine%20Wave%202.24%25%20JPEG%20Version.jpg)

**Limp-Home State (3-Level)**
* Raw Faulted THD: **78.26%** * Filtered Faulted THD: **42.55%** ![Raw 3-Level THD](5%20Level%20CHB/Raw%20Staircase%2078.26%20%25%20JPEG%20Version.jpg)
![Filtered 3-Level THD](5%20Level%20CHB/Limp%20Home%2042.55%20%25%20JPEG%20Version.jpg)

---

## ⚙️ Key Specifications 

| Parameter | Value / Unit |
| :--- | :--- |
| **Simulation Platform** | MATLAB/Simulink R2024b (Ode23tb Solver) |
| **Fundamental Frequency** | 50 Hz |
| **Carrier Switching Frequency** | 4000 Hz |
| **Filter Resonant Frequency** | ~184 Hz |
| **Output Power** | ~1.81 kW |
| **Load (Traction Motor Stator)** | Series RL (10 Ω, 5 mH) |

---
*Developed by **Het Kansara** (23BEE011) | Advanced Power Electronic Converters IA Project | Even Semester 2026*
