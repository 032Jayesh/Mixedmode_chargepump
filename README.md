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


## Given Specifications

- Electron mobility × oxide capacitance: \(\mu_n C_{ox} = 200\ \mu\text{A}/\text{V}^2 = 200\times10^{-6}\ \text{A}/\text{V}^2\).
- Hole mobility × oxide capacitance: \(\mu_p C_{ox} = 80\ \mu\text{A}/\text{V}^2 = 80\times10^{-6}\ \text{A}/\text{V}^2\).
- Overdrive voltage: \(V_{ov}=0.25\ \text{V}\).
- Widths and length: \(W_n = 120\ \text{nm},\ W_p = 480\ \text{nm},\ L = 45\ \text{nm}\).

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
