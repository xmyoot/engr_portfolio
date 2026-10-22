---
title: "ESP32 LTE Kids Communicator: ESP32-C3 + SIMCom A7672 LTE Cat-1 Voice Handset"
date: 2026-10-22 12:00:00 -0700
published: true
tags: [hardware, kicad, esp32c3, lte, cat1, simcom, a7672, voice, audio, battery, usb-c, 4-layer, pcb, embedded]
thumbnail: /assets/images/esp32-lte-kids-communicator-thumb.png
excerpt: "A client-driven 84 × 56 mm 4-layer handset board: ESP32-C3 controller, pre-certified SIMCom A7672G LTE Cat-1 voice module, PAM8302A speaker amp, 1.8 V UART translation, USB-C Li-ion charging. Wired single-sheet schematic, 0 ERC / 0 DRC, JLCPCB-ready."
---

A client-driven **V1 cellular voice device for kids**: press **CALL**, talk over LTE, press **END**. The brief asked for an ESP32, a SIM/LTE module, mic + speaker, battery charging and antenna-placement guidance, built on **pre-certified (FCC-friendly) modules** while the mechanical design was still in progress.

I designed it natively in **KiCad 10** as a single, fully wired A3 schematic and an **84 × 56 mm, 4-layer** board. It's ready for JLCPCB fabrication and assembly.

**Status: Rev A, fabrication-ready engineering prototype.**
- ERC 0 · DRC 0 · schematic↔PCB parity 0 · 43/43 signal nets routed.
- Reviewed with the [kicad-happy](https://github.com/aklofas/kicad-happy) analyzers.
- Signals routed with [KiCadRoutingTools](https://github.com/drandyhaas/KiCadRoutingTools); power runs through the planes.

[View Project Repository](https://github.com/xmyoot/esp32-lte-kids-communicator) · [Hardware Design Document](https://github.com/xmyoot/esp32-lte-kids-communicator/blob/master/docs/hardware_design.md) · [Schematic PDF](https://github.com/xmyoot/esp32-lte-kids-communicator/blob/master/hardware/fabrication/kids-communicator-schematic.pdf)

---

## 3D Hardware Overview

<p align="center">
  <img src="{{ '/assets/images/esp32-lte-kids-communicator/board_isometric.png' | relative_url }}" alt="Kids Communicator isometric 3D view" width="85%" />
</p>

<p align="center">
  <img src="{{ '/assets/images/esp32-lte-kids-communicator/board_top.png' | relative_url }}" alt="Kids Communicator top view" width="49%" />
  <img src="{{ '/assets/images/esp32-lte-kids-communicator/board_bottom.png' | relative_url }}" alt="Kids Communicator bottom view" width="49%" />
</p>

*Figure 1: KiCad 3D renders of Rev A (matte black solder mask, ENIG finish).*

- **Controller (top-left):** the ESP32-C3-WROOM-02, with its PCB antenna on the board edge and an all-layer keep-out.
- **Cellular (centre-right):** the A7672G LGA module. Its UART pads face the level shifter, VBAT pads face the 330 µF bulk capacitor, the RF pad faces the U.FL, audio faces the right-edge mic/speaker connectors, and SIM faces the hinged nano-SIM socket.
- **User / power edge (bottom):** USB-C, the battery header, the charge LED and the CALL / END / AUX buttons, all on one edge for the enclosure.

---

## Schematic

<p align="center">
  <img src="{{ '/assets/images/esp32-lte-kids-communicator/schematic.png' | relative_url }}" alt="Kids Communicator schematic" width="100%" />
</p>

*Figure 2: The schematic is one A3 sheet drawn by signal flow: USB-C → charger → battery/LDO across the top, then ESP32 → level shifter → modem → SIM / antenna / audio. The UART bus runs straight across with no crossings. Labels are used only for genuinely distant nets (USB, AMP_EN, PWRKEY_CTL, VBAT_SENSE).*

The sheet is generated from a Python description and **refuses to write unless the drawing matches a 47-net design-intent table**. The script's netlist is then diffed against KiCad's own export, so wiring mistakes are caught before layout. That check caught two real ones: a TVS drawn to GND on the wrong pin, and a decoupling cap landing on a UART row.

---

## Technical Specifications

| Subsystem | Implementation | Key details |
| :--- | :--- | :--- |
| **PCB** | 84 × 56 mm, 4-layer JLC04161H-7628, 1.6 mm, ENIG | L2 solid GND · L3 split +3V3 / VBAT power · 0.15 mm track/space · 0.45/0.25 mm vias · no via-in-pad · 170+ stitching vias · 4 × M2 · 3 fiducials |
| **Controller** | Espressif ESP32-C3-WROOM-02-N4 | RISC-V 160 MHz, Wi-Fi/BLE, native USB Serial/JTAG for flashing and console (no USB-UART bridge) |
| **Cellular** | SIMCom A7672G LTE Cat-1 (LCC+LGA-124) | VoLTE voice, pre-certified; custom symbol and footprint built from SIMCom HD V1.03 |
| **Level translation** | TI TXS0104E | DTR / RI / TXD / RXD between 3.3 V and the modem's 1.8 V; OE tied to VDD_1V8, so it goes Hi-Z when the modem is off |
| **Modem power key** | BSS138 open-drain | 100 k gate pull-down prevents false toggles during ESP32 reset |
| **Audio** | Module codec + Diodes PAM8302A | Electret mic direct (bias inside the module); 32 Ω earpiece output → class-D amp → 8 Ω speaker, MCU shutdown control |
| **Charger** | Microchip MCP73831-2 | 4.20 V, 500 mA (R<sub>PROG</sub> 2 kΩ), charge LED |
| **3.3 V rail** | TI TLV75533 | 500 mA LDO from VBAT |
| **Modem supply** | Direct from the 1S cell | 330 µF polymer tantalum + 1 µ / 100 n / 33 p / 10 p at the pads, TVS, 0.8 mm bus and L3 plane |
| **RF** | U.FL + π-match (0 Ω, 2 × DNP) | 50 Ω CPWG over the L2 ground, ground-via fence, < 10 mm feed |
| **USB-C** | GCT USB4105 | 5.1 kΩ CC pull-downs (sink), USBLC6-2SC6 ESD on D± and VBUS |
| **Service** | Tag-Connect TC2030-NL | Modem USB, BOOT and log UART for SIMCom firmware updates |

---

## Engineering Design Decisions

### 1. Verify the module pinout from the datasheet
The earlier prototype's modem map listed "speaker" pins that don't exist. I rebuilt the A7672X symbol and land pattern from the SIMCom hardware-design manual. Unit A holds every pin the design uses or must terminate; unit B holds the unused LCD/CAM/GNSS/keypad pins with explicit no-connects. The manual also shows the audio output is a **32 Ω class-AB earpiece driver** (37 mW). That drove the decision to add a PAM8302A so an 8 Ω speaker reaches kid-audible volume.

### 2. Spend the four translator channels where they matter
Instead of mirroring every modem status line, the TXS0104E carries:

- **TXD and RXD**, the UART.
- **RI**, on a wake-capable GPIO, so an incoming call wakes the ESP32.
- **DTR**, which lets the modem enter UART-controlled sleep. This is the largest battery-life lever: about 2 mA idle instead of about 20 mA.

### 3. GPIO map chosen for the copper, not just the schematic
The first routing pass crossed the UART bus because the logical pin map didn't match the physical pad order. I remapped the ESP32 GPIOs and reordered the modem and translator symbol pins so the **ESP32 IO0–3 → TXS0104E → modem** bus is straight in both the schematic and the PCB. The strapping-pin rules still hold: GPIO2 and GPIO8 are pulled up, and GPIO9 is the BOOT/AUX button.

### 4. Power through planes, signals through the router
A deterministic fan-out gives every GND, VBAT and 3V3 pad its own adjacent via into L2/L3, with no via-in-pad. The autorouter only sees the 43 signal nets, with router keep-outs under both modules. The result is short, mostly 45° copper on L1, and an L4 that is almost entirely solid ground.

<p align="center">
  <img src="{{ '/assets/images/esp32-lte-kids-communicator/layout_top_copper.png' | relative_url }}" alt="Top copper" width="49%" />
  <img src="{{ '/assets/images/esp32-lte-kids-communicator/layout_bottom_copper.png' | relative_url }}" alt="Bottom copper" width="49%" />
</p>

*Figure 3: Top copper (signals + GND pour) and bottom copper (near-solid GND).*

### 5. Antenna guidance for the mechanical team
The two radios sit in opposite top corners. The LTE antenna is an external flex antenna on the U.FL; it should sit against the enclosure wall, away from the battery and speaker magnet, with the π-match pads left for tuning in the final housing. The ESP32 PCB antenna needs about 15 mm clear of metal, the battery and screws.

---

## Power Budget (1000 mAh 1S Li-ion, typical)

| State | Typical current | Battery life |
| :--- | :---: | :---: |
| **Idle, registered** (modem DTR sleep, ESP32 light sleep) | ≈ 2.3 mA | ~18 days standby |
| **Voice call** (modem ~200 mA average, 2 A peaks, amp on) | ≈ 270 mA | ~3.5 h talk time |

---

## Verification Gates

| Domain | Tool | Result |
| :--- | :--- | :---: |
| **Schematic vs. intent** | Generator check (47 nets) + KiCad netlist diff | **Identical** |
| **Schematic ERC** | KiCad 10.0.7 | **PASSED (0 errors, 0 warnings)** |
| **Layout DRC** | KiCad 10.0.7 | **PASSED (0 violations, 0 unconnected)** |
| **Schematic ↔ PCB parity** | KiCad 10.0.7 | **PASSED** |
| **Design review** | kicad-happy | Actionable findings closed (fiducials, MPN coverage) |
| **Fab outputs** | Gerbers, Excellon, BOM (MPNs), CPL, STEP, PDFs | **Generated** |
