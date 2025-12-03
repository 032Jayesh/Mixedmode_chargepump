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

A README that embeds the provided design images and documents the transistor sizing and small-signal on-resistance calculations. Copy this file into `README.md` at your project root and place the images in `assets/` as described below.

---

## Preview

<p align="center">
  <img src="assets/design.png" alt="Design Preview" width="900" />
</p>

<p align="center">
  <img src="assets/design2.png" alt="Schematic & Calculations" width="900" />
</p>

> **Note:** Place the image files in your repository at `assets/design.png` and `assets/design2.png`. If you prefer different paths or filenames, update the `src` attributes accordingly.

---

## Given Specifications

- Electron mobility × oxide capacitance: \(\mu_n C_{ox} = 200\ \mu\text{A}/\text{V}^2 = 200\times10^{-6}\ \text{A}/\text{V}^2\).
- Hole mobility × oxide capacitance: \(\mu_p C_{ox} = 80\ \mu\text{A}/\text{V}^2 = 80\times10^{-6}\ \text{A}/\text{V}^2\).
- Overdrive voltage: \(V_{ov}=0.25\ \text{V}\).
- Widths and length: \(W_n = 120\ \text{nm},\ W_p = 480\ \text{nm},\ L = 45\ \text{nm}\).

---

## Step 1 — Compute \(W/L\)

\[
\left(\frac{W}{L}\right)_n = \frac{120}{45} = 2.6666667
\]

\[
\left(\frac{W}{L}\right)_p = \frac{480}{45} = 10.6666667
\]

---

## Step 2 — NMOS Saturation Current (long-channel square-law style approximation)

Use the simplified MOS saturation current expression (square-law style):

\[
I_{D} = \frac{1}{2}\,\mu C_{ox}\left(\frac{W}{L}\right) V_{ov}^2
\]

**NMOS arithmetic (step-by-step):**

1. \(V_{ov}^2 = (0.25)^2 = 0.0625\).
2. \(\mu_n C_{ox} \cdot (W/L)_n = 200\times10^{-6}\cdot2.6666667 = 5.3333334\times10^{-4}\ \text{A}/\text{V}^2\).
3. Multiply by \(V_{ov}^2\): \(5.3333334\times10^{-4}\cdot0.0625 = 3.333333375\times10^{-5}\ \text{A}\).
4. Apply the 1/2 factor: \(I_{D,n} = \tfrac{1}{2}\cdot3.333333375\times10^{-5} = 1.6666666875\times10^{-5}\ \text{A} \approx 16.67\ \mu\text{A}.\)

---

## Step 3 — PMOS Saturation Current

**PMOS arithmetic (step-by-step):**

1. \(\mu_p C_{ox} \cdot (W/L)_p = 80\times10^{-6}\cdot10.6666667 = 8.53333336\times10^{-4}\ \text{A}/\text{V}^2\).
2. Multiply by \(V_{ov}^2\): \(8.53333336\times10^{-4}\cdot0.0625 = 5.33333335\times10^{-5}\ \text{A}\).
3. Apply the 1/2 factor: \(I_{D,p} = \tfrac{1}{2}\cdot5.33333335\times10^{-5} = 2.666666675\times10^{-5}\ \text{A} \approx 26.67\ \mu\text{A}.\)

---

## Step 4 — Approximate \(R_{on}\) Using Small-Signal Approximation

Using the boxed approximation (small-signal channel resistance around the operating point):

\[
R_{on} \approx \frac{1}{\mu C_{ox} (W/L) V_{ov}}
\]

**NMOS:**

- Denominator: \(200\times10^{-6}\cdot2.6666667\cdot0.25 = 1.33333335\times10^{-4}\ \text{A}/\text{V}\).
- \(R_{on,n} \approx \dfrac{1}{1.33333335\times10^{-4}} \approx 7{,}500\ \Omega.\)

**PMOS:**

- Denominator: \(80\times10^{-6}\cdot10.6666667\cdot0.25 = 2.13333334\times10^{-4}\ \text{A}/\text{V}\).
- \(R_{on,p} \approx \dfrac{1}{2.13333334\times10^{-4}} \approx 4{,}687.5\ \Omega.\)

---

## Final Summary (numeric)

- \(I_{D,n} \approx 16.67\ \mu\text{A}\)
- \(I_{D,p} \approx 26.67\ \mu\text{A}\)
- \(R_{on,n} \approx 7.5\ \text{k}\Omega\)
- \(R_{on,p} \approx 4.6875\ \text{k}\Omega\)

---

## Usage & Image Placement

To reproduce this README in your repo:

```bash
mkdir -p assets
# copy the two images into assets/ as design.png and design2.png



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
