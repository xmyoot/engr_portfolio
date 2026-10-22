---
title: "High-Precision 16-Bit Strain Gauge Analog Front-End (AFE) for M5Stack Core2"
date: 2026-10-22 09:40:00 -0700
published: true
tags: [hardware, kicad, analog, instrumentation, adc, m5stack, pcb, strain-gauge, sensors]
thumbnail: /assets/images/strain-gauge-afe-thumb.png
excerpt: "A laboratory-grade 50 × 50 mm, 4-layer precision analog front-end stackable module for M5Stack Core2: 100 kSps 16-bit SAR ADC (ADS8860), AD8421 in-amp, ADR4550 low-noise reference, and split-rail power."
---

A laboratory-grade, high-speed **50 × 50 mm, 4-layer precision analog front-end (AFE) stackable module** engineered to interface directly with the **M5Stack Core2** (and M5Stack CoreS3 / standard 2×15 2.54 mm M-Bus hosts). Designed for demanding industrial and scientific applications—including dynamic strain testing, load cell weighing, torque sensing, and high-frequency hydraulic pressure monitoring—the system captures Wheatstone bridge transducers at up to **100 kSps with true 16-bit resolution**.

The hardware features a shared ratiometric $5.000\text{ V}$ excitation and ADC reference architecture, an ultra-low-noise instrumentation amplifier input stage ($G = 100$), a 2-pole active Butterworth Multiple Feedback (MFB) anti-aliasing filter ($f_c = 25\text{ kHz}, G = -10$), an on-board 12-bit non-volatile offset-nulling DAC, and a dual-polarity split-rail power tree ($+7.5\text{ V} / -3.3\text{ V}$) ensuring linear amplifier operation all the way down to true zero volts.

**Status: Approved for manufacturing (Fabrication-Ready Rev A).** Schematic, 4-layer PCB layout, and manufacturing packages are fully validated in KiCad 10 with **zero ERC errors, zero DRC violations, zero unrouted nets, and 100% netlist parity**. Complete manufacturing outputs—including RS-274X Gerbers, Excellon NC drill files, JLCPCB Component Placement List (CPL), and structured BOM—are generated and verified.

[View Project Repository](https://github.com/xmyoot/strain-gauge-afe-m5stack) · [Schematic Architecture PDF](https://github.com/xmyoot/strain-gauge-afe-m5stack/blob/main/docs/strain_gauge_afe_schematic.pdf) · [Design Notes & Math Specifications](https://github.com/xmyoot/strain-gauge-afe-m5stack/blob/main/docs/DESIGN.md) · [Fabrication Deliverables](https://github.com/xmyoot/strain-gauge-afe-m5stack/tree/main/production) · [Starter Bring-Up Firmware](https://github.com/xmyoot/strain-gauge-afe-m5stack/tree/main/firmware)

---

## 3D Hardware Overview

<p align="center">
  <img src="{{ '/assets/images/strain-gauge-afe-m5stack/board_iso.png' | relative_url }}" alt="Strain Gauge AFE 3D Isometric View" width="85%" />
</p>

<p align="center">
  <img src="{{ '/assets/images/strain-gauge-afe-m5stack/board_top.png' | relative_url }}" alt="Strain Gauge AFE PCB Top View" width="49%" />
  <img src="{{ '/assets/images/strain-gauge-afe-m5stack/board_bottom.png' | relative_url }}" alt="Strain Gauge AFE PCB Bottom View" width="49%" />
</p>

*Figure 1: Native 3D CAD visualization of the Strain Gauge AFE Module (Rev A). Top: Left-to-right analog signal progression featuring Phoenix Contact 5-pin terminal block (J2), TPD4E05U06 ESD array, AD8421 instrumentation amplifier, ADR4550 5.000 V reference, OPA2197 dual buffers, OPA2197 2-pole Butterworth MFB filter, ADS8860 16-bit SAR ADC, and right-hand 2×15 M5Stack M-Bus stacking header (J1). Bottom: Continuous underside ground plane with comprehensive terminal block pinout and half/full bridge wiring diagram silkscreened directly onto the PCB.*

---

## Complete Schematic Architecture

<p align="center">
  <img src="{{ '/assets/images/strain-gauge-afe-m5stack/schematic_overview.png' | relative_url }}" alt="Strain Gauge AFE Complete Schematic Diagram" width="100%" />
</p>

*Figure 2: Complete single-sheet schematic of the 16-Bit 100 kSps Strain Gauge AFE generated reproducibly from algorithmic netlist source scripts.*

---

## Technical Specifications & Architecture

| Subsystem | Component / Implementation | Key Specifications & Engineering Details |
| :--- | :--- | :--- |
| **PCB Form Factor** | $50.00 \times 50.00\text{ mm}$ square outline | 4-layer FR-4, $1.60\text{ mm}$ thickness, ENIG surface finish. 4× M2 mounting holes on a $45.0\text{ mm}$ square grid matching M5Stack modular enclosures. |
| **Layer Stackup** | 4-Layer `JLC04161H-7628` | Layer 1: Top Signal / Analog components; Layer 2: Solid In1 continuous GND return plane; Layer 3: In2 $+7.5\text{ V}$ power plane; Layer 4: Bottom Signal & ground pour. |
| **Sensor Interface** | Phoenix Contact MC 1,5/5-G-3,81 | Heavy-duty 5-pin pluggable screw terminal block: `1: VEXC (+5V)`, `2: SIG+`, `3: SIG-`, `4: GND (EXC-)`, `5: SHIELD`. |
| **Bridge Compatibility** | Half-Bridge & Full-Bridge | On-board precision $1\text{ k}\Omega / 1\text{ k}\Omega$ thin-film completion divider ($0.1\%$, $25\text{ ppm}/^\circ\text{C}$). Solder jumper `JP1` switches between half-bridge (default 1-2) and full-bridge (2-3). |
| **Input ESD & RFI** | TPD4E05U06 + Matched C0G Filter | $0.5\text{ pF}$ low-capacitance ESD protection array at J2; differential RFI filter ($100\ \Omega + 2.2\text{ nF}$, $f_c \approx 170\text{ kHz}$) and common-mode filter ($220\text{ pF}$, $f_c \approx 7\text{ MHz}$). |
| **Instrumentation Amp** | Analog Devices AD8421ARMZ | Ultra-low noise ($3.2\text{ nV}/\sqrt{\text{Hz}}$ @ $1\text{ kHz}$), $2\text{ MHz}$ bandwidth at $G = 100$, $\text{CMRR} > 114\text{ dB}$, gain configured with precision $100\ \Omega$ ($0.1\%$, $25\text{ ppm}/^\circ\text{C}$) resistor. |
| **Anti-Aliasing Filter** | 2-Pole Butterworth MFB Filter | Multiple Feedback (MFB) inverting low-pass filter with $f_c = 25\text{ kHz}$ cutoff, gain $G = -10$, utilizing TI OPA2197 with C0G dielectric capacitors. |
| **Overall Signal Gain** | Dual-stage $G_\text{total} = 1000\times$ | In-amp ($+100\times$) $\times$ MFB active filter ($-10\times$). Full scale input $\pm 2.5\text{ mV}$ produces $0\text{ V} \text{ to } 5.0\text{ V}$ span at the ADC input. |
| **A/D Conversion** | Texas Instruments ADS8860IDGSR | 16-bit pseudo-differential SAR ADC, true 1 MSPS capability operated at $100\text{ kSps}$, no missing codes, 4-wire CS SPI mode with dedicated hardware `CONVST`. |
| **Voltage Reference** | Analog Devices ADR4550BRZ | Ultra-low noise ($1\ \mu\text{V}_\text{p-p}$ $0.1\text{--}10\text{ Hz}$), high accuracy ($5.000\text{ V} \pm 0.02\%$), $2\text{ ppm}/^\circ\text{C}$ drift, post-filtered through $16\text{ Hz}$ low-pass RC network. |
| **Buffer Amplifiers** | Texas Instruments OPA2197IDGKR | Rail-to-rail I/O on split rails, $5.5\text{ nV}/\sqrt{\text{Hz}}$, $10\text{ MHz}$ GBW, driving $350\ \Omega$ bridge ($14\text{ mA}$) and ADC dynamic switched capacitive load. |
| **Offset Calibration DAC** | Microchip MCP4725A0T-E/CH | 12-bit I²C DAC with integrated non-volatile EEPROM; sums into MFB non-inverting node to provide $\pm 7.7\text{ mV}$ input-referred hardware offset nulling ($\pm 3\times$ full span). |
| **Split-Rail Power Tree** | TI LMR62014 + TPS60403 + TPS7A39 | $+5\text{ V}$ M-Bus input boosted to $+9\text{ V}$ ($1.6\text{ MHz}$ PWM) and inverted to $-5\text{ V}$; post-regulated by TPS7A39 dual tracking LDO to clean $+7.5\text{ V} / -3.3\text{ V}$ rails ($>50\text{ dB}$ PSRR @ $1\text{ MHz}$). |
| **Host M-Bus Connector** | 2×15 2.54 mm Stacking Socket | Standard M5Stack Core2 / CoreS3 M-Bus header carrying SPI (SCK `G18`, MISO `G38`, CS `G27`), hardware conversion clock (`G19`), and I²C Port A (`G32`/`G33`). |

---

## System Architecture Block Diagram

```mermaid
flowchart LR
    subgraph Sensor_Input ["Wheatstone Bridge Sensor (J2 5-Pin Phoenix)"]
        EXC["VEXC (+5.000 V)"] --> SENS["350 Ω Strain Gauge\n(Half or Full Bridge)"]
        SENS --> SIG_P["SIG+"]
        SENS --> SIG_N["SIG-"]
        SHIELD["Cable Shield"] -->|R21 0 Ω| GND["System GND"]
    end

    subgraph Protection_RFI ["Input Protection & RFI Filtering"]
        SIG_P & SIG_N --> ESD["TI TPD4E05U06\n0.5 pF ESD Array"]
        ESD --> RFI["Matched C0G RFI Filter\n(Diff: 170 kHz, CM: 7 MHz)"]
    end

    subgraph Analog_Gain_Chain ["Precision Signal Conditioning (G = 1000)"]
        RFI --> INAMP["AD8421 In-Amp (G = 100)\n3.2 nV/√Hz, CMRR > 114 dB"]
        INAMP --> MFB["OPA2197 Active MFB Filter\n2-Pole Butterworth, fc = 25 kHz, G = -10"]
        DAC["Microchip MCP4725\n12-bit EEPROM Offset DAC"] -->|Summing Network| MFB
    end

    subgraph Digitization ["16-Bit SAR Digitization"]
        MFB --> KICK["200 Ω + 2.2 nF\nSAR Kickback Filter"]
        KICK --> ADC["TI ADS8860 16-Bit SAR ADC\n100 kSps Pseudo-Diff"]
    end

    subgraph Host_Interface ["M5Stack Core2 M-Bus (2x15 Header)"]
        ADC -->|SPI (SCK G18, MISO G38, CS G27)| MBUS["M5Stack Core2 Host"]
        MBUS -->|HW Timer Clock CONVST G19| ADC
        MBUS <-->|I2C Port A (G32 / G33)| DAC
    end

    subgraph Reference_Power ["Ratiometric Reference & Low-Noise Power"]
        MBUS -->|+5 V & +3.3 V| PWR_TREE["LMR62014 Boost (+9 V)\nTPS60403 Inverter (-5 V)\nTPS7A39 Dual LDO (+7.5 V / -3.3 V)\nLP5907 AVDD LDO (+3.3 V)"]
        REF["ADR4550 5.000 V Ref\n(2 ppm/°C, 0.02%)"] --> BUF["Dual OPA2197 Buffers\n(16 Hz Filtered)"]
        BUF -->|VEXC| SENS
        BUF -->|VREF| ADC
    end
```

---

## Deep Dive: Engineering Design Decisions & Calculations

### 1. Ratiometric Excitation & Absolute Drift Cancellation

A primary challenge in high-resolution strain gauge acquisition is eliminating voltage reference thermal drift and low-frequency $1/f$ flicker noise. In conventional architectures, separate supplies excite the bridge and bias the ADC, causing any drift in either source to directly corrupt the strain measurement.

In this architecture, a single ultra-low-noise reference (**ADR4550BRZ**, $5.000\text{ V} \pm 0.02\%$, $2\text{ ppm}/^\circ\text{C}$) is filtered by a $16\text{ Hz}$ passive RC network and buffered by two separate channels of a precision **OPA2197**: one dedicated to driving the bridge excitation voltage ($V_\text{EXC}$), and the second driving the ADC reference input ($V_\text{REF}$).

Because the bridge differential output is inherently proportional to the excitation voltage:

$$V_\text{diff} = V_\text{SIG+} - V_\text{SIG-} = V_\text{EXC} \cdot \left( \frac{\Delta R}{R} \right)$$

And the ADC digital output code for pseudo-differential conversion is:

$$\text{Code} = 65536 \cdot \frac{V_\text{ADC}}{V_\text{REF}}$$

With $V_\text{ADC} = \frac{V_\text{REF}}{2} - G_\text{total} \cdot V_\text{diff}$ (referenced around mid-scale):

$$\frac{V_\text{diff}}{V_\text{EXC}} = \frac{\text{Code}_\text{tare} - \text{Code}}{65536 \cdot G_\text{total}}$$

Since $V_\text{EXC} \equiv V_\text{REF} = 5.000\text{ V}$, the reference voltage cancels out completely in the ratio calculation $\frac{V_\text{diff}}{V_\text{EXC}}$. Thermal drift, aging degradation, and power rail fluctuations in the reference source do not introduce first-order gain errors.

---

### 2. Gain Staging, Dynamic Range & Resolution

The analog signal chain is staged to provide a total voltage gain of **$1000\times$**:

1. **Pre-amplification Stage:** Analog Devices **AD8421** configured for $G = 100$ using a high-precision thin-film resistor $R_G = 100\ \Omega$ ($0.1\%$, $25\text{ ppm}/^\circ\text{C}$):
   $$G_1 = 1 + \frac{9.988\text{ k}\Omega}{R_G} = 1 + \frac{9988}{100} \approx 100.88 \approx 100\times$$
2. **Active Filter Stage:** Multiple Feedback (MFB) inverting low-pass filter providing $G_2 = -\frac{R_{15}}{R_{14}} = -\frac{16.5\text{ k}\Omega}{1.65\text{ k}\Omega} = -10\times$.

$$G_\text{total} = G_1 \cdot |G_2| = 100 \times 10 = 1000\times$$

For a typical $350\ \Omega$ foil strain gauge with Gauge Factor $\text{GF} = 2.0$:
- **Full-Scale Input Range:** $\pm 2.5\text{ mV}$ differential at the bridge terminals maps to a $\pm 2.5\text{ V}$ swing at the ADC, spanning the entire $0\text{ V to } 5.0\text{ V}$ input range around the $2.5\text{ V}$ mid-scale point.
- **Microstrain Span:** $\pm 2.5\text{ mV}$ across a $5.0\text{ V}$ half-bridge corresponds to $\pm 1000\ \mu\varepsilon$ ($\pm 1\text{ mm/m}$).
- **Quantization Step (1 LSB):**
  $$1\text{ LSB} = \frac{5.000\text{ V}}{65536} \approx 76.29\ \mu\text{V at the ADC}$$
  $$\text{Referred to input (RTI): } \frac{76.29\ \mu\text{V}}{1000} = 76.29\text{ nV}$$
  $$\text{Strain Resolution: } \varepsilon_\text{LSB} = \frac{4 \cdot (76.29\text{ nV} / 5.000\text{ V})}{2.0} \approx 0.0305\ \mu\varepsilon\text{ (30.5 nanostrain)}$$

---

### 3. Active 2-Pole Butterworth Anti-Aliasing Filter

To satisfy the Nyquist acquisition criterion at $100\text{ kSps}$ ($f_\text{Nyquist} = 50\text{ kHz}$), an active 2-pole Multiple Feedback (MFB) low-pass filter is implemented around the second channel of the **OPA2197**:

- **Cutoff Frequency:** $f_c = 25.0\text{ kHz}$
- **Filter Topology:** Butterworth maximally flat response ($Q = \frac{1}{\sqrt{2}} \approx 0.7071$)
- **Component Design:**
  - $R_{14} = 1.65\text{ k}\Omega$, $R_{15} = 16.5\text{ k}\Omega$ (sets inverting gain $G = -10$)
  - $R_{16} = 237\ \Omega$
  - Feedback capacitors: $C_{27} = 22\text{ nF}$ (C0G ceramic) and $C_{28} = 470\text{ pF}$ (C0G ceramic)
  - All signal capacitors are specified in **C0G / NP0 dielectric** to completely prevent piezoelectric microphonics and voltage-coefficient harmonic distortion.
- **Attenuation Profile:** Provides $>20\text{ dB}$ attenuation across the $75\text{--}125\text{ kHz}$ aliasing band, ensuring out-of-band high-frequency structural resonances or mechanical vibrations do not fold down into the DC–$25\text{ kHz}$ baseband.

---

### 4. Non-Volatile Hardware Offset Nulling Network

Wheatstone bridges in physical installations exhibit baseline zero-offset errors due to manufacturing gauge resistance tolerances ($0.1\%\text{ to } 1\%$) and pre-load clamping tension. If uncorrected, a $2\text{ mV}$ initial bridge imbalance would consume $80\%$ of the dynamic range after $1000\times$ amplification.

Rather than relying purely on software tare (which wastes ADC dynamic range), this design incorporates a dedicated hardware offset nulling circuit:
- A 12-bit I²C DAC (**MCP4725A0**, address `0x60`) is controlled over the M5Stack I²C bus.
- The DAC output is attenuated through an asymmetrical summing resistor network ($R_{17} = 11.8\text{ k}\Omega$ to $V_\text{REF}$, $R_{18} = 19.6\text{ k}\Omega$ to GND, and $R_{19} = 10\text{ k}\Omega$ from DAC) connected to the non-inverting terminal of the MFB filter stage.
- **Adjustment Range:** Shifts the output by $\pm 7.7\text{ mV}$ referred to the input ($\approx \pm 300\%$ of full scale), allowing precise analog zeroing of substantial bridge imbalances while preserving all 16 bits of ADC conversion resolution.
- **Non-Volatile Storage:** The MCP4725 includes integrated EEPROM, enabling the host to store the factory null calibration code directly on the daughterboard. The zero point persists across power cycles without host intervention.

---

### 5. Split-Rail Power Architecture & Sub-Harmonic Isolation

Single-supply in-amp circuits fail when attempting to measure bridge voltages near ground, as the internal output transistors saturate $50\text{--}150\text{ mV}$ above the negative rail.

To guarantee uncompromising linearity across the entire span down to true $0.000\text{ V}$:
1. **Switching Pre-Regulators:**
   - TI **LMR62014** high-frequency boost converter steps up $+5\text{ V}$ from the M5Stack M-Bus to $+9.0\text{ V}$ switching at $1.6\text{ MHz}$.
   - TI **TPS60403** switched-capacitor charge pump inverts the $+5\text{ V}$ rail to $-5.0\text{ V}$.
   - Physical placement of both switchers is isolated in the bottom-right corner of the PCB, over $35\text{ mm}$ away from the sensitive analog input nodes.
2. **Dual Tracking Ultra-Low-Noise LDO:**
   - A Texas Instruments **TPS7A3901** dual tracking positive/negative LDO post-regulates the switcher rails down to $+7.5\text{ V}$ and $-3.3\text{ V}$.
   - The TPS7A39 provides $>50\text{ dB}$ Power Supply Rejection Ratio (PSRR) at $1\text{ MHz}$, eliminating switching harmonics from the analog rails.
   - The $-3.3\text{ V}$ negative headroom allows the AD8421 and OPA2197 output stages to swing linearly to and below $0\text{ V}$.
3. **Dedicated ADC Logic LDO:**
   - TI **LP5907-3.3** ultra-low-noise LDO ($I_\text{Q} = 250\text{ mA}$, noise $< 6.5\ \mu\text{V}_\text{rms}$) provides an isolated $+3.3\text{ V}$ digital/analog rail (`AVDD`) for the ADS8860 SAR ADC.

---

### 6. System Noise Budget & Effective Resolution (ENOB)

| Noise Source | Spectral Density | Effective Bandwidth ($f_\text{ENBW} = 1.11 \times 25\text{ kHz}$) | RMS Voltage RTI |
| :--- | :--- | :--- | :--- |
| **$350\ \Omega$ Bridge Thermal Noise** | $e_{n,\text{bridge}} = \sqrt{4 k_B T R} \approx 3.8\text{ nV}/\sqrt{\text{Hz}}$ | $27.75\text{ kHz}$ | $0.63\ \mu\text{V}_\text{rms}$ |
| **AD8421 In-Amp Voltage Noise** | $e_{n,\text{inamp}} = 3.2\text{ nV}/\sqrt{\text{Hz}}$ | $27.75\text{ kHz}$ | $0.53\ \mu\text{V}_\text{rms}$ |
| **Gain Resistor $R_G$ ($100\ \Omega$)** | $e_{n,RG} = 1.28\text{ nV}/\sqrt{\text{Hz}}$ | $27.75\text{ kHz}$ | $0.21\ \mu\text{V}_\text{rms}$ |
| **Total Input-Referred Noise Floor** | $e_{n,\text{total}} = \sqrt{3.8^2 + 3.2^2 + 1.28^2} \approx \mathbf{5.13\text{ nV}/\sqrt{\text{Hz}}}$ | $27.75\text{ kHz}$ | $\mathbf{0.85\ \mu\text{V}_\text{rms}}$ |

- **Instantaneous Single-Sample Noise:** $0.85\ \mu\text{V}_\text{rms}$ referred to the input corresponds to approximately **$11.1\text{ LSB}_\text{rms}$** or **$0.34\ \mu\varepsilon_\text{rms}$**.
- **Oversampling & Burst Averaging:** Operating at $100\text{ kSps}$, capturing a $4096\text{-sample}$ burst ($41\text{ ms}$) and averaging improves SNR by $\sqrt{N} = \sqrt{4096} = 64\times$:
  $$e_\text{averaged} = \frac{0.85\ \mu\text{V}_\text{rms}}{64} \approx 13.3\text{ nV}_\text{rms} \approx \mathbf{0.0053\ \mu\varepsilon_\text{rms}}$$
  Yielding laboratory-grade nanostrain resolution for static calibration and baseline tare.

---

## Layout, EMC & Signal Integrity Strategy

```
Layer Stackup (JLC04161H-7628 Controlled 1.6 mm 4-Layer):
┌─────────────────────────────────────────────────────────────┐ F.Cu   (0.035 mm) - Signal & Analog routing, 0.15 mm min
├─────────────────────────────────────────────────────────────┤ Dielectric: 0.21 mm 7628 Prepreg
███████████████████████████████████████████████████████████████ In1.Cu (0.035 mm) - Solid Continuous Ground Plane (Faraday cage)
├─────────────────────────────────────────────────────────────┤ Core: 1.065 mm FR-4 Core
▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ In2.Cu (0.035 mm) - Split Power Plane (+7.5 V / Clean Analog)
├─────────────────────────────────────────────────────────────┤ Dielectric: 0.21 mm 7628 Prepreg
└─────────────────────────────────────────────────────────────┘ B.Cu   (0.035 mm) - Underside Ground Shield & Wiring silkscreen
```

1. **Unbroken Solid Return Plane (Layer 2):** Layer 2 (`In1.Cu`) is dedicated as an uninterrupted, solid ground plane underneath the entire analog front-end. No signal or power traces pierce the ground shield in the analog region.
2. **Perimeter Ground Stitching Fence:** 273 stitched ground vias (spaced $\le 2.5\text{ mm}$ apart) bond top copper ground pours to Layer 2 and Layer 4, creating a low-impedance Faraday cage around the sensitive instrumentation amplifier and anti-aliasing filter stages.
3. **Strict Left-to-Right Topology:** Signal flow progresses strictly unidirectionally across the board:
   - *Left Edge:* Phoenix terminal block `J2` $\rightarrow$ ESD array $\rightarrow$ RFI filter.
   - *Center-Left:* AD8421 in-amp and gain setting resistor.
   - *Center:* ADR4550 reference and OPA2197 buffers.
   - *Center-Right:* 2-pole MFB filter and ADS8860 SAR ADC.
   - *Right Edge:* M-Bus 2×15 digital connector `J1`.
   - *Bottom-Right:* Isolated DC-DC switching power converters.
   This layout topology prevents noisy digital SPI return currents from circulating through the analog ground plane.
4. **Kelvin Sense & Symmetric Differential Pair:** `SIG+` and `SIG-` traces are routed symmetrically as a coupled differential pair shielded on both sides by grounded guard rings with guard vias tied directly to Layer 2.
5. **Connector-Only Cable Shield Grounding:** Terminal pin 5 (`SHIELD`) connects to ground at a single chassis point through $0\ \Omega$ resistor `R21`, preventing external ground loops from inducing circulating currents through the measurement cable shield.

---

## 7-Step Calibration & Bring-Up Procedure

1. **Thermal Stabilization:** Power the system for 10 minutes to allow the Wheatstone bridge, ADR4550 reference, and AD8421 in-amp to achieve thermal equilibrium.
2. **Hardware Offset Nulling:** With the sensor under zero load, execute a 12-step binary search over I²C (`0x60`) adjusting the MCP4725 DAC output until test point `TP8` (`ADC_IN`) sits at exactly $2.500\text{ V} \pm 10\text{ mV}$ (digital code $32768 \pm 200$). Write the optimal code to MCP4725 EEPROM.
3. **Zero-Point Tare:** Record a baseline calibration burst (average of 8 bursts of 4096 samples at $100\text{ kSps}$) and store the result as $\text{Code}_\text{tare}$.
4. **Span Calibration:** Apply a certified calibration reference load $P_\text{ref}$ (e.g., dead-weight tester or hydraulic reference transducer) and record the averaged code:
   $$\text{Span} = \frac{P_\text{ref}}{\text{Code}_\text{tare} - \text{Code}}$$
5. **Shunt Calibration Verification:** Connect a precision resistor across the active gauge arm to verify calculated electrical sensitivity:
   $$\varepsilon_\text{sim} = \frac{R_\text{gauge}}{\text{GF} \cdot (R_\text{gauge} + R_\text{shunt})}$$
   *(e.g., $349.65\text{ k}\Omega$ across $350\ \Omega$ simulates $-500\ \mu\varepsilon$, exactly 16384 ADC codes at $\text{GF} = 2.0$).*
6. **Gauge Factor Configuration:** Configure the gauge factor from the manufacturer calibration certificate (`g 2.05`) for direct microstrain readout.
7. **High-Speed Dynamic Streaming:** Execute high-speed SPI burst transfers at $100\text{ kSps}$ triggered by the ESP32 hardware LEDC timer clock (`CONVST` on `G19`), capturing 4096-sample bursts into DMA buffers for real-time FFT frequency-domain spectrum analysis.

---

## Manufacturing & Verification Summary

The design has been validated against JLCPCB 4-layer standard manufacturing design rules and exported to production formats:

| Deliverable Asset | File Location in Repository | Verification Status |
| :--- | :--- | :--- |
| **KiCad 10 Schematic** | `strain_gauge_afe.kicad_sch` | 0 ERC errors, clean pin electrical types |
| **KiCad 10 Layout** | `strain_gauge_afe.kicad_pcb` | 0 DRC violations, 0 unconnected nets |
| **RS-274X Gerbers & Drill** | `production/strain_gauge_afe_gerbers.zip` | 4-layer Protel extensions, Excellon PTH/NPTH drills |
| **Standardized BOM** | `production/strain_gauge_afe_bom.csv` | 108 total parts, LCSC Part numbers & MPNs mapped |
| **Component Placement List** | `production/strain_gauge_afe_cpl.csv` | Standard SMD centroid coordinates & orientations |
| **3D STEP Mechanical Model** | `docs/` | 100% matched footprint 3D heights ($\le 8.5\text{ mm}$) |
| **Bring-Up Firmware Sketch** | `firmware/src/main.cpp` | PlatformIO / Arduino Core2 bring-up driver |

---

## Design Tools

- **EDA Software:** KiCad 10.0.7
- **Design Review & Linting:** [kicad-happy](https://github.com/aklofas/kicad-happy) automated analyzer suite
- **Layout Synthesis:** Custom Python geometry generators (`scripts/design.py`, `scripts/gen_pcb.py`, `scripts/gen_sch.py`)
- **Embedded Firmware:** PlatformIO & Arduino-ESP32
