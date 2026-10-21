---
title: 16-Bit 100 kSps Precision Strain Gauge AFE Module
date: 2026-10-17
tags: [hardware, analog, instrumentation, adc, m5stack, kicad]
thumbnail: /assets/images/strain-gauge-afe-thumb.png
excerpt: "Precision 4-layer analog front-end stackable module for M5Stack Core2 with low-noise instrumentation amp, 16-bit SAR ADC, Kelvin bridge excitation, and heavy-duty screw terminal."
---
A laboratory-grade, **high-speed 16-bit 100 kSps precision Analog Front-End (AFE)** stackable instrumentation module designed for dynamic strain gauge, load cell, torque transducer, and bridge sensor data acquisition. Compatible with the **M5Stack Core2 / CoreS3** ecosystem via a stackable 2×15 2.54 mm M-Bus connector.

- **GitHub Repository:** [xmyoot/strain-gauge-afe-m5stack](https://github.com/xmyoot/strain-gauge-afe-m5stack)
- **Form Factor:** 50.1 × 50.1 mm square module matching standard M5Stack enclosure geometry with M3 corner mounting holes.
- **Layer Stackup:** 4-layer (Signal / Ground / Power / Signal) with dense via stitching grid and isolated analog ground islands.

# Board Overview

<p align="center">
  <img src="/assets/images/strain-gauge-afe-iso.png" alt="Strain Gauge AFE 3D View" width="100%" />
</p>

The board packs 108 components into a compact 50 × 50 mm outline. The analog signal chain occupies the left and center sections, while the digital M-Bus interface runs along the right edge. Complete screw-terminal wiring instructions are silkscreened directly onto the bottom copper layer.

![Strain Gauge AFE Top Layout](/assets/images/strain-gauge-afe-top.png)

![Strain Gauge AFE Bottom Silkscreen](/assets/images/strain-gauge-afe-bottom.png)

# Key Features

- **Precision Analog Signal Chain:**
  - **Instrumentation Amplifier Stage:** Ultra-low-noise, rail-to-rail instrumentation amplifier with precision gain setting and differential low-pass anti-aliasing filter.
  - **16-Bit SAR ADC (100 kSps):** High-speed Texas Instruments **ADS8861** true-differential input ADC communicating over high-speed SPI with a dedicated `DRDY` data-ready interrupt line.
  - **Low-Drift Voltage Reference:** High-stability reference providing calibrated $+5.0\text{ V}$ excitation voltage ($V_\text{EXC}$) for Wheatstone bridge sensors.
  - **Kelvin Bridge Sensing:** Dedicated 4-wire and 6-wire sense traces to compensate for lead-wire resistance in long industrial sensor cables.
- **Industrial Connectors & Terminal Block:**
  - Heavy-duty 5-position 3.81 mm pitch screw terminal block (`J2`):
    - `Pin 1: VEXC (+5V)` (Excitation Positive)
    - `Pin 2: SIG+` (Differential Signal In +)
    - `Pin 3: SIG-` (Differential Signal In -)
    - `Pin 4: GND (EXC-)` (Excitation Negative / Ground)
    - `Pin 5: SHIELD` (Earth / Cable Shield)
  - Full pinout table printed in high-contrast silkscreen on the bottom layer for field wiring without documentation.
- **M5Stack Core2 M-Bus Stackability:**
  - 2×15 2.54 mm pass-through header (`J1`) routing SPI, chip select, interrupt, and 5 V / 3.3 V power rails directly into the M5Stack bus.
- **Diagnostics & Calibration:**
  - 11 gold-plated test points (`TP1` – `TP11`) for probing excitation stability, differential inputs, ADC reference, and supply rails.
  - Solder jumper (`JP1`) for selecting internal vs external excitation modes.

# Electrical & Layout Architecture

- **Extreme Trace Discipline:** 902 track segments with 99.9% 45° angle consistency (0.1% odd angle deviation); 273 vias; 0 unrouted nets.
- **Faraday Cage Shielding:** A dense grid of 273 ground stitching vias envelopes the sensitive analog routing, tying the top and bottom ground copper to the inner ground plane.
- **Thermal & Layout Symmetry:** Differential signal traces (`SIG+` and `SIG-`) are length-matched and routed symmetrically over unbroken ground copper to maximize Common-Mode Rejection Ratio (CMRR).

# Use Cases

- High-speed dynamic impact, vibration, and drop shock testing.
- Structural health monitoring (bridges, aerospace airframes, crane load cells).
- Industrial automated test equipment (ATE) and load cell calibration benches.
