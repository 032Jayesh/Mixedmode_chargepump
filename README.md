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

---

## Applications of the Charge Pump

A charge pump that provides low current-mismatch and stable charge transfer is useful in many mixed-signal and power-management contexts. Typical applications include:

- **On-chip voltage generation (boost/negative rails):** Generate higher (e.g., 2×/3×) or negative supply voltages from a single supply for gate drivers, EEPROM/Flash programming, or bias networks.

- **Non-volatile memory programming:** Provide the high programming and erase voltages required by flash and EEPROM without an external DC-DC converter.

- **LCD / OLED driver supplies:** Produce the voltages needed for display gate drivers and contrast biasing in small-panel displays.

- **Bias generation for analog blocks:** Create stable internal bias rails (substrate, well, reference biases) for op-amps, comparators, and ADCs where matching and low mismatch matter.

- **Level shifters and gate drivers:** Drive transistors whose gates require voltages beyond the core supply (e.g., high-side/low-side drivers in power stages).

- **On-chip DC-DC conversion for ultra-low-power systems:** Replace an external regulator in energy-harvesting or sensor nodes where area and quiescent current are critical.

- **PLL/VCO tuning and supply:** Provide local supply or bias voltages for RF blocks, phase-locked loops, and voltage-controlled oscillators.

- **Charge redistribution and sampling circuits:** Improve accuracy of switched-capacitor circuits by supplying matched, low-mismatch charge to sampling capacitors.

- **Battery-powered devices and portable electronics:** Small-area, efficient charge pumps help extend battery life by avoiding large off-chip converters.

- **Test and calibration circuits:** Provide known, repeatable voltages for on-chip calibration, test-mode programming, and factory trim operations.


### Notes

- The usefulness of a charge pump depends on its efficiency, output ripple, current-matching, and the target process. For deep-submicron technologies, consider effects such as switching loss, leakage, and charge injection when integrating a charge pump.

- For production designs, validate with SPICE and characterise across process, voltage, and temperature (PVT) corners.

---


## References

[1] W. Rhee, “Design of high-performance CMOS charge pumps in phase locked loops,” Proc. Int. Symp. Circuits Syst., vol. 2, pp. 545–548, 1999.

[2] J.-S. Lee and M.-S. Keel, “Charge pump with perfect current matching characteristics in phase-locked loops,” Electron. Lett., vol. 36, pp. 1907–1908, 2000.

[3] P. Acco, M. P. Kennedy, C. Mria, B. Morley, and B. Frigyik, "Behavioral modeling of charge pump phase locked loops," Proc. IEEE/ISCAS, pp. 375–378, Orlando, FL, May 1999.

[4] H. Rategh, H. Samavati, T. Lee, “A CMOS frequency synthesizer with an injection locked frequency divider for a 52 GHz wireless LAN receiver,” IEEE J. Solid-State Circuits, vol. 35, pp. 780–787, 2000.

[5] R. A. Baki, M. N. ElGamal, “A new CMOS charge pump for low voltage high speed PLL applications,” Proc. 2003 Int. Symp. Circuits and Systems (ISCAS’03), vol. 1, pp. I-2657–I-2660, 2003.

[6] J. S. Lee, M. S. Keel, S. I. Lim, “Charge pump with perfect current matching characteristics in phase locked loops,” Electronics Letters, vol. 36, pp. 1907–1908, 2000.

[7] J. A. Starzyk, Ying-Wei Jan, Fengjing Qiu, “A DC–DC charge pump design based on voltage doublers,” IEEE Trans. Circuits Syst. I, vol. 48(3), pp. 350–359, 2001.

[8] P. K. Hanumolu, M. Brownlee, K. Mayaram, and U.-K. Moon, "Analysis of charge pump phase locked loops," IEEE Trans. Circuits Syst., vol. 51, no. 9, pp. 1665–1674, Sept. 2004.

[9] Shanfeng Cheng, Jose Silva-Martinez, Aydin Ilker Karsilayan, “Design and Analysis of an Ultrahigh-Speed Glitch-Free Fully Differential Charge Pump With Minimum Output Current Variation and Accurate Matching,” IEEE Trans. Circuits Syst., vol. 53(9), pp. 843–847, 2006.

[10] N. D. Dalt and C. Sandner, “A Subpicosecond Jitter PLL for Clock Generation in 0.12 μm Digital CMOS,” IEEE J. Solid-State Circuits, vol. 38, no. 7, pp. 1275–1278, July 2003.

[11] J. F. Parker and D. Weinlader, “A 15 mW 3.125 GHz PLL for serial backplane transceivers in 0.13 μm CMOS,” Proc. Int. Solid-State Circuits Conf., pp. 412–413, 2005.

[12] B. Terlemez and J. P. Uyemura, “The design of a differential CMOS charge pump for high performance phase-locked loops,” Proc. Int. Symp. Circuits Syst., vol. 4, pp. IV–561–564, 2004.

[13] E. Juarez-Hernandez and A. Diaz-Sanchez, “A novel CMOS charge pump circuit with positive feedback for PLL applications,” Proc. Int. Conf. Electron., Circuits Systems, vol. 1, pp. 349–352, 2001.

[14] R. C. H. Beek and C. S. Vaucher, “A 2.5–10 GHz clock multiplier unit with 0.22-ps RMS jitter in standard 0.18 μm CMOS,” IEEE J. Solid-State Circuits, vol. 39, no. 11, pp. 1862–1872, 2004.

[15] T. S. Cheung and B. C. Lee, “A 1.8–3.2 GHz fully differential GaAs MESFET PLL,” IEEE J. Solid-State Circuits, vol. 36, no. 4, pp. 605–601, 2001.

[16] B. Razavi, *Design of Analog CMOS Integrated Circuits.* New York: McGraw-Hill, ch. 15, pp. 550–556, 2001.
