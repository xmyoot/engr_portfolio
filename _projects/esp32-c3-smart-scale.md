---
title: "ESP32-C3 Smart Scale: Low-Power Wi-Fi/BLE Load-Cell Board with USB-C Li-ion Charging"
date: 2026-10-22 11:00:00 -0700
published: true
tags: [hardware, kicad, esp32c3, load-cell, hx711, low-power, battery, usb-c, pcb, embedded]
thumbnail: /assets/images/esp32-c3-smart-scale-thumb.png
excerpt: "A 52 × 38 mm 2-layer smart-scale main board: ESP32-C3-MINI-1U with external antenna, HX711 24-bit load-cell ADC with switched excitation, MCP73831 USB-C Li-ion charging and a 25 µA-Iq 3.0 V rail. About 35 µA in standby, JLCPCB-ready."
---

A client-driven **low-power smart-scale main board** designed natively in **KiCad 10**. The brief asked for the *cheapest sensible architecture* that is still a real product rather than a dev-board clone. It needed Wi-Fi + BLE with an external antenna, USB-C charging of a 1S 3000 mAh cell, battery-level sensing, load-cell weighing, very low sleep current and factory programming pads.

The result is a **52 × 38 mm, 2-layer, single-sided SMT** board built from 24 BOM lines, all of them JLCPCB/LCSC-sourced. Standby draw is about **35 µA**.

**Status: Rev A, fabrication-ready.** ERC 0 · DRC 0 · schematic↔PCB parity 0 · 100 % routed. It was also reviewed with the [kicad-happy](https://github.com/aklofas/kicad-happy) schematic, PCB and Gerber analyzers.

[View Project Repository](https://github.com/xmyoot/esp32-c3-smart-scale) · [Architecture & Design Decisions](https://github.com/xmyoot/esp32-c3-smart-scale/blob/main/docs/architecture.md) · [Design Review](https://github.com/xmyoot/esp32-c3-smart-scale/blob/main/docs/design_review.md) · [Bring-Up Notes](https://github.com/xmyoot/esp32-c3-smart-scale/blob/main/docs/bring_up_notes.md) · [Schematic PDF](https://github.com/xmyoot/esp32-c3-smart-scale/blob/main/fab/schematic.pdf)

---

## 3D Hardware Overview

<p align="center">
  <img src="{{ '/assets/images/esp32-c3-smart-scale/board_isometric.png' | relative_url }}" alt="ESP32-C3 Smart Scale isometric 3D view" width="85%" />
</p>

<p align="center">
  <img src="{{ '/assets/images/esp32-c3-smart-scale/board_top.png' | relative_url }}" alt="ESP32-C3 Smart Scale top view" width="49%" />
  <img src="{{ '/assets/images/esp32-c3-smart-scale/board_bottom.png' | relative_url }}" alt="ESP32-C3 Smart Scale bottom view" width="49%" />
</p>

*Figure 1: KiCad 3D renders of Rev A.*

- **RF zone (top-right):** the ESP32-C3-MINI-1U, with its U.FL antenna connector at the board edge.
- **Power zone (right and bottom-centre):** USB-C, ESD array, charger, charge LED and the JST-PH battery connector.
- **Analog zone (left):** the HX711, its PNP excitation regulator and the JST-PH load-cell connector.
- **Underside:** a 7-pad pogo interface for factory flashing and test.

---

## Schematic

<p align="center">
  <img src="{{ '/assets/images/esp32-c3-smart-scale/schematic.png' | relative_url }}" alt="ESP32-C3 Smart Scale schematic" width="100%" />
</p>

*Figure 2: The schematic is a single A3 sheet, drawn by signal flow (USB-C → ESD/charger → battery/LDO → MCU → load-cell ADC). Every signal is a real wire, net labels only name nets, and the design notes sit on the sheet.*

---

## Technical Specifications

| Subsystem | Implementation | Key details |
| :--- | :--- | :--- |
| **PCB** | 52 × 38 mm, 2-layer FR-4 1.6 mm | JLCPCB standard rules (0.2 mm track/space, 0.6/0.3 mm vias), GND pour on both layers with ~110 stitching vias, 4 × M2 holes, 3 fiducials |
| **MCU / radio** | Espressif ESP32-C3-MINI-1U-H4 | RISC-V 160 MHz, Wi-Fi 4 + BLE 5, 4 MB flash, U.FL external antenna, native USB Serial/JTAG (no USB-UART bridge) |
| **Load-cell ADC** | Avia HX711 (SOIC-16) | 24-bit, PGA ×128, 10 SPS, internal oscillator, 2-wire interface to IO5/IO6 |
| **Bridge excitation** | MMBT5401 PNP + 12 k/10 k divider | Uses the HX711's own regulator loop: EXC+ = 2.75 V, and excitation switches **off** in power-down |
| **Charger** | Microchip MCP73831-2 | 4.20 V CC/CV, R<sub>PROG</sub> 2.2 kΩ → 455 mA (0.15 C), status LED |
| **System rail** | TI TLV75530 LDO | 3.0 V / 500 mA, 25 µA I<sub>Q</sub>, 238 mV dropout |
| **Battery gauge** | 1 MΩ / 330 kΩ divider + 100 nF | 4.2 V → 1.04 V into the IO4 ADC, 3.2 µA standing current |
| **USB-C** | HRO TYPE-C-31-M-12 | 5.1 kΩ CC pull-downs (sink), USBLC6-2SC6 ESD on D± and VBUS |
| **User input** | 5.1 mm tactile switch on IO3 | Active-high with 100 k pull-down; RTC GPIO deep-sleep wake |
| **Factory interface** | 7 pogo pads (underside) | VBAT, 3V0, EN, IO9/BOOT, TXD0, RXD0, GND |

---

## Engineering Design Decisions

### 1. Switching the load-cell excitation off in sleep, with no extra GPIO
A 1 kΩ strain bridge draws about 3 mA continuously, roughly 100× the rest of the sleep budget. The HX711 has an on-chip regulator controller (BASE/VFB) that is only active while the chip is powered up. I used it to drive a PNP pass element, which gives two things:

- **Regulated excitation:** EXC+ = 1.25 V × (1 + R12/R13) = 2.75 V, which improves ratiometric accuracy.
- **Free switching:** excitation turns off automatically when firmware holds PD_SCK high.

There's no extra load switch and no extra GPIO.

### 2. A 3.0 V rail instead of 3.3 V
The ESP32-C3 is specified from 3.0 V. A 3.0 V LDO keeps the module in spec down to a cell voltage of about 3.25 V, even during a 350 mA Wi-Fi burst. That recovers most of the Li-ion discharge curve without a buck-boost converter, and keeps the BOM and EMI risk low for a device that is awake only a few seconds per weighing.

### 3. Battery-backed power path
The product always contains a cell, so the system runs from VBAT and the MCP73831 charges it directly. Charge current is set to 455 mA, which leaves headroom inside a USB 2.0 500 mA port while the radio is active. This removes the need for a dedicated power-path IC.

### 4. A schematic that reads like a circuit
I built custom ESP32-C3-MINI-1U and HX711 symbols with pin placement chosen around the signal flow. The strapping pull-ups step down into their pins, the factory pads run straight across into EN/IO9/UART, the USB pair enters the module from below, and the load-cell bridge, excitation regulator and divider form one wired analog block. Power symbols are used only for rails.

### 5. Layout: hand-routed trunks plus an assisted router
The power trunks (VBAT, 3V0), the HX711 bus and the USB-C fan-out were placed by hand as orthogonal routes:

- **VBAT** drops under the HX711 bus on the bottom layer to the LDO.
- **The two USB-C D+ pads** are joined at their outer ends and routed clear of the shell tab.
- **The two D− pads** are joined through a single bottom-layer hop.

The remaining short local nets were completed with [KiCadRoutingTools](https://github.com/drandyhaas/KiCadRoutingTools) at the JLC standard fab tier. Every iteration was checked with KiCad DRC plus schematic parity.

<p align="center">
  <img src="{{ '/assets/images/esp32-c3-smart-scale/layout_top_copper.png' | relative_url }}" alt="Top copper" width="49%" />
  <img src="{{ '/assets/images/esp32-c3-smart-scale/layout_bottom_copper.png' | relative_url }}" alt="Bottom copper" width="49%" />
</p>

*Figure 3: Top and bottom copper.*

---

## Power Budget (3000 mAh 1S Li-ion)

| State | Active | Typical current | Battery life |
| :--- | :--- | :---: | :---: |
| **Deep sleep** | RTC timer + IO3 wake, LDO, divider; HX711 and excitation off | **≈ 35 µA** | ~9 years (self-discharge dominated) |
| **Weighing** | CPU + HX711 + 2.75 mA bridge, radio off | ≈ 25 mA | — |
| **Wi-Fi report** | TX bursts | ≈ 120 mA avg, 350 mA peak | — |
| **Typical use** | 10 weighings/day (3 s each) + one 5 s Wi-Fi sync, sleep otherwise | ≈ 1.2 mAh/day (sleep ≈ 0.84 mAh of that) | **> 5 years per charge** (self-discharge limited) |

---

## Verification Gates

| Domain | Tool | Result |
| :--- | :--- | :---: |
| **Schematic ERC** | KiCad 10.0.7, all severities | **PASSED (0)** |
| **Layout DRC** | KiCad 10.0.7 | **PASSED (0 errors, 0 unconnected)** |
| **Schematic ↔ PCB parity** | KiCad 10.0.7 | **PASSED (0 issues)** |
| **Design review** | kicad-happy schematic / PCB / Gerber analyzers | **0 errors**; warnings triaged in the review doc |
| **Sourcing** | LCSC lookup (kicad-happy) | **All 24 BOM lines carry MPN + LCSC codes** |
| **Fab outputs** | Gerbers, Excellon, JLC BOM/CPL, STEP, PDF | **Generated** |
