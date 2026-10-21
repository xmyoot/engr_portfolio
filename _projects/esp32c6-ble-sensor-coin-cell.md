---
title: ESP32-C6 BLE Coin-Cell Sensor Node
date: 2026-10-20
tags: [hardware, esp32, bluetooth, sensor, kicad, low-power]
thumbnail: /assets/images/esp32c6-sensor-thumb.png
excerpt: "Ultra-compact 31.5 mm circular 4-layer IoT sensor pod with ESP32-C6, buck-boost PMIC, precision I2C multi-sensor suite, and CR2450 coin-cell power."
---
An ultra-compact, industrial-grade **31.5 mm circular sensor node** built around the Espressif **ESP32-C6-MINI-1** RISC-V SoC. Engineered specifically for harsh industrial potting, hermetic enclosure pucks, and multi-year battery lifespans using a single CR2450 coin cell.

- **GitHub Repository:** [xmyoot/esp32c6-ble-sensor-coin-cell](https://github.com/xmyoot/esp32c6-ble-sensor-coin-cell)
- **Form Factor:** $\varnothing 31.50\text{ mm}$ circular PCB with dual keying notches and $\le 2.40\text{ mm}$ top-side height envelope.
- **Layer Stackup:** 4-layer ENIG (Signal / Ground / Power / Signal) with strict JLCPCB DFM compliance.

# Board Overview

<p align="center">
  <img src="/assets/images/esp32c6-sensor-iso.png" alt="ESP32-C6 Sensor Node 3D View" width="100%" />
</p>

The board features a symmetrical component distribution around the central ESP32-C6 module, with the RF antenna extending over the board edge. Gold test pads are arrayed radially for automated bed-of-nails pogo-pin testing.

![ESP32-C6 Sensor Top Layout](/assets/images/esp32c6-sensor-top.png)

![ESP32-C6 Sensor Bottom Coin Cell](/assets/images/esp32c6-sensor-bottom.png)

# Key Engineering Features

- **Microcontroller & Wireless:**
  - Espressif **ESP32-C6-MINI-1-N4** (32-bit RISC-V core up to 160 MHz).
  - 2.4 GHz Wi-Fi 6 (802.11ax), Bluetooth 5.3 LE, and IEEE 802.15.4 (Zigbee / Thread).
  - Onboard PCB antenna extending cleanly past the ground plane keepout boundary.
- **Power Architecture & Power Management:**
  - **Primary Supply:** Single CR2450 3.0 V Lithium coin cell ($\approx 620\text{ mAh}$) mounted in a rugged through-hole bottom retainer clip.
  - **Buck-Boost Converter:** Texas Instruments **TPS631000DRLR** ($1.5\text{ MHz}$ fixed-frequency buck-boost, $I_\text{Q} = 8\ \mu\text{A}$), providing a rock-solid 3.3 V rail down to end-of-life battery discharge.
  - **Voltage Supervisor:** TI **TPS3839G33DQNR** ($I_\text{Q} = 150\text{ nA}$, $3.08\text{ V}$ threshold, $200\text{ ms}$ reset delay) preventing brownout MCU locks.
- **Precision Multi-Sensor Array (Fast-Mode 400 kHz I2C):**
  - **Sensirion SHT40-AD1B:** High-precision relative humidity ($\pm 1.8\%$) and ambient temperature ($\pm 0.2^\circ\text{C}$).
  - **STMicroelectronics LIS2DW12TR:** Ultra-low-power 3-axis accelerometer with autonomous wake-on-motion interrupt on `GPIO4`.
  - **TI TMP117AIDRVR:** Medical/industrial grade high-accuracy temperature sensor ($\pm 0.1^\circ\text{C}$ across $-20^\circ\text{C}$ to $+50^\circ\text{C}$).
- **Programming & Bring-Up:**
  - South edge mid-mount Micro-USB connector for initial flashing and hardware calibration.
  - Tactile boot and reset switches with debounced RC filters.

# Electrical & Layout Architecture

- **Strict 45° Routing:** 99.5% angle compliance across 568 track segments; 101 vias with 0 unrouted nets.
- **Ground Return & RF Isolation:** Continuous internal L2 ground plane under high-speed traces with stitching vias around the periphery. The module antenna region has complete copper, trace, and plane keepouts across all four layers.
- **Potting & Height Constraints:** All surface-mount passives are low-profile 0402/0603, ensuring the entire assembly stays strictly below the 2.40 mm mechanical ceiling for low-pressure resin overmolding.

# Use Cases

- Industrial machine condition monitoring and predictive vibration telemetry.
- Environmental cold-chain shipping loggers (temperature, humidity, drop detection).
- Low-profile wearable health and motion trackers.
