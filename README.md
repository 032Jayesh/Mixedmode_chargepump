# Mixed-Signal Circuit Design
Documentation of the design project for Mixed-Signal Circuits

# Design of Charge Pump With Low Current Mismatch and Low Glitches

This repository contains the design documentation, theory, and implementation details of a **high-performance Charge Pump (CP)** used inside a **Phase-Locked Loop (PLL)**. The objective is to achieve **low current mismatch**, **reduced glitches**, and **stable output voltage**, suitable for modern mixed-signal and high-speed PLL systems.

---

## 📘 Overview

A **Phase-Locked Loop (PLL)** is a closed-loop negative feedback system used to synchronize the phase of its output with a reference signal. PLLs are essential in:

- Frequency synthesizers  
- Communication systems  
- High-speed digital circuits  
- Clock generation and recovery  

The **Charge Pump** is a critical sub-block of the PLL that converts phase error signals into control voltage for the VCO (Voltage-Controlled Oscillator). Its performance directly affects **jitter**, **locking time**, **phase noise**, and **loop stability**.

---

## 🔧 PLL Architecture

A typical PLL consists of:  
- **Phase-Frequency Detector (PFD)**  
- **Charge Pump (CP)**  
- **Loop Filter (LF)**  
- **Voltage-Controlled Oscillator (VCO)**  
- Optional frequency divider  

The PFD outputs **UP** and **DOWN** signals based on phase difference.  
The CP converts these signals into precise charge/discharge currents which are filtered into a smooth VCO control voltage.

---

## ⚡ Role of the Charge Pump

The Charge Pump:  
- Converts digital **UP/DOWN** pulses into analog current  
- Charges/discharges the loop filter  
- Generates a stable control voltage (*Vctrl*) for the VCO  
- Determines locking behavior, noise, and overall PLL performance

**UP pulse → charge current → Vctrl increases**  
**DOWN pulse → discharge current → Vctrl decreases**

---

# Project Design: Visual Asset and Device Calculations



## Given specifications

- `µn * Cox = 200 µA/V² = 200e-6 A/V²`  
- `µp * Cox = 80 µA/V²  = 80e-6 A/V²`  
- `V_ov = 0.25 V`  
- `W_n = 120 nm`  
- `W_p = 480 nm`  
- `L   = 45 nm`

---

## 1) Width-to-length ratios

- `(W/L)_n = 120 / 45 = 2.6666667`  
- `(W/L)_p = 480 / 45 = 10.6666667`

---

## 2) Saturation current formula (square-law approximation)

We use the simplified (long-channel) saturation expression:
### NMOS arithmetic (step-by-step)

1. `V_ov^2 = (0.25)^2 = 0.0625`  
2. `µn C_ox * (W/L)_n = 200e-6 * 2.6666667 = 5.3333334e-4 A/V²`  
3. multiply by `V_ov^2`: `5.3333334e-4 * 0.0625 = 3.333333375e-5 A`  
4. apply 1/2:  
   `I_D_n = 0.5 * 3.333333375e-5 = 1.6666666875e-5 A ≈ 16.67 µA`

### PMOS arithmetic (step-by-step)

1. `µp C_ox * (W/L)_p = 80e-6 * 10.6666667 = 8.53333336e-4 A/V²`  
2. multiply by `V_ov^2`: `8.53333336e-4 * 0.0625 = 5.33333335e-5 A`  
3. apply 1/2:  
   `I_D_p = 0.5 * 5.33333335e-5 = 2.666666675e-5 A ≈ 26.67 µA`

---

## 3) Small-signal approximate `R_on`

Use the linearized approximation around the operating point:


### NMOS

- Denominator: `200e-6 * 2.6666667 * 0.25 = 1.33333335e-4 A/V`  
- `R_on,n ≈ 1 / 1.33333335e-4 ≈ 7_500 Ω` (≈ 7.5 kΩ)

### PMOS

- Denominator: `80e-6 * 10.6666667 * 0.25 = 2.13333334e-4 A/V`  
- `R_on,p ≈ 1 / 2.13333334e-4 ≈ 4_687.5 Ω` (≈ 4.6875 kΩ)

---

## 4) Final numeric summary

- `I_D,n ≈ 16.67 µA`  
- `I_D,p ≈ 26.67 µA`  
- `R_on,n ≈ 7.5 kΩ`  
- `R_on,p ≈ 4.6875 kΩ`

---

## Notes & assumptions

- These calculations use the long-channel square-law style formulas — good for hand estimates. In modern deep-submicron processes the real device behavior differs (velocity saturation, mobility reduction, channel-length modulation, body effect, etc.). For production-accurate numbers, run SPICE with the foundry compact model.  
- Units: A, V, Ω unless noted.  
- All arithmetic displayed explicitly for clarity.

---



## Simulation Waveforms

### **Transient Response**
![Transient Response](./op.jpg)

---

## Cadence Schematic — Testbench View
![Charge Pump Testbench](./cp_test.png)

---

## Cadence Schematic — Charge Pump Circuit
![Charge Pump Schematic](./cp.png)


---


## 📊 Performance Summary

From simulated behavior based on the documented architecture:

- **Technology:** 45 nm CMOS  
- **Supply Voltage:** 1.1 V  
- **Output Swing:** ~0 to 1.08 V  
- **Current Mismatch:** < 0.3%  
- **Stable output with minimal ripple**  
- **Reduced glitch and noise sensitivity**

These characteristics satisfy the stringent requirements of modern high-speed PLLs.
