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

## ⚠ Limitations of Traditional Charge Pumps

Traditional MOS-based CP designs suffer from:

- **Charge/Discharge current mismatch**  
- **Glitches due to switching transients**  
- **Leakage currents causing phase error**  
- **Output ripple affecting VCO stability**  
- **Limited swing range under low supply**  
- **Switch timing delay leading to phase error**

These effects degrade loop stability, increase jitter, and slow down the locking process.

## Simulation Waveforms

### **Transient Response**
![Transient Response](./op.jpg)

---

## Cadence Schematic — Testbench View
![Charge Pump Testbench](./Screenshot_from_2025-12-02_07-06-13.png)

---

## Cadence Schematic — Charge Pump Circuit
![Charge Pump Schematic](./Screenshot_from_2025-12-02_07-06-16.png)


---

## 🔁 Improved Charge Pump With Error Amplifier

To address mismatch, an improved charge pump uses:

- A **high-gain error amplifier**  
- Feedback loop to force **I_up = I_down**  
- Better steady-state behavior  
- Lower phase error  
- Reduced mismatch compared to the traditional design  

However, leakage-induced phase error and ripple remain challenges.

---

## 🟦 Differential Charge Pump With Reference Voltage

A more advanced architecture employs a **fully differential charge pump**, designed to:

### ✔ Cancel leakage-induced phase error
Leakage affects both differential outputs equally → differential subtraction removes it.

### ✔ Improve current matching
Using fully symmetrical pull-up and pull-down paths.

### ✔ Achieve wider output voltage swing
Suitable for low-voltage and high-frequency designs.

### ✔ Reduce glitches and switching noise
Through source-side switching and precharge circuitry.

### ✔ Stabilize the output voltage
Using a **reference-voltage follower** plus a protection network to prevent back-injection.

### ✔ Produce stable output without spurious jumps
Critical for high-speed PLL acquisition and locking.

This architecture meets the needs of high-speed communication systems where even small VCO voltage fluctuations can severely affect output frequency.

---

## 📊 Performance Summary

From simulated behavior based on the documented architecture:

- **Technology:** 0.35 μm CMOS  
- **Supply Voltage:** 3.3 V  
- **Output Swing:** ~0 to 3.1 V  
- **Current Mismatch:** < 0.3%  
- **Stable output with minimal ripple**  
- **Reduced glitch and noise sensitivity**

These characteristics satisfy the stringent requirements of modern high-speed PLLs.
