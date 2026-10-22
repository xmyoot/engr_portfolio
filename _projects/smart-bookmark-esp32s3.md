---
title: "Smart Bookmark ESP32-S3: Ultra-Low-Power Voice Recorder & BLE Annotation Device"
date: 2026-10-22
published: true
tags: [hardware, kicad, esp32-s3, ble, audio, low-power, pcb, embedded]
thumbnail: /assets/images/smart-bookmark-esp32s3-thumb.png
excerpt: "An ultra-thin 37 × 52 mm, 4-layer smart bookmark device powered by ESP32-S3: digital PDM MEMS microphone, 16 MB SPI NOR flash, USB-C LiPo charging, and ~15 µA deep-sleep power architecture."
---

An ultra-compact, ultra-thin **37.0 × 52.0 mm, 4-layer smart bookmark hardware device** designed to nest unobtrusively inside a book or notebook. Designed around the **Espressif ESP32-S3-MINI-1-N8**, the device captures spoken memos, audio annotations, and reading timestamps via a single tactile push-button, streams and compresses audio onto dedicated 16 MB SPI NOR flash, and syncs data via Bluetooth Low Energy (BLE 5.0) to a mobile companion application.

**Status: Designed, routed, and digitally verified in KiCad 10.** Complete fabrication and assembly package generated (Gerbers, drills, JLCPCB SMT BOM, CPL, STEP 3D CAD model). The design validates with **zero ERC errors, zero DRC violations, and zero unconnected nets**. Physical prototype fabrication and bench testing are pending.

[View Project Repository](https://github.com/xmyoot/smart-bookmark-esp32s3) · [Schematic PDF](https://github.com/xmyoot/smart-bookmark-esp32s3/blob/main/fab/smart-bookmark-esp32s3_schematic.pdf) · [Design Specification](https://github.com/xmyoot/smart-bookmark-esp32s3/blob/main/docs/design_spec.md) · [Manufacturing Package](https://github.com/xmyoot/smart-bookmark-esp32s3/tree/main/fab)

---

## 3D Hardware Overview

<p align="center">
  <img src="{{ '/assets/images/smart-bookmark-esp32s3/render_iso.png' | relative_url }}" alt="Smart Bookmark ESP32-S3 3D Isometric View" width="100%" />
</p>

*Figure 1: High-resolution raytraced 3D CAD rendering of the Smart Bookmark ESP32-S3 (Rev A). All active and passive components reside on the top layer, leaving a flat underside for a 25 × 30 mm single-cell LiPo battery.*

<p align="center">
  <img src="{{ '/assets/images/smart-bookmark-esp32s3/render_top.png' | relative_url }}" alt="Top PCB View" width="49%" />
  <img src="{{ '/assets/images/smart-bookmark-esp32s3/render_bot.png' | relative_url }}" alt="Bottom PCB View with Battery Cradle" width="49%" />
</p>

*Figure 2: Top side with populated SMT component islands (left) and bottom side featuring the battery target footprint, through-hole acoustic mic port, and 2×3 pogo-pin test pad matrix (right).*

---

## Technical Specifications & Architecture

| Subsystem | Component / Implementation | Key Specifications |
| :--- | :--- | :--- |
| **PCB Form Factor** | 37.0 × 52.0 × 0.8 mm, 4 Layers | FR-4, 1 oz Cu, ENIG finish, R2.0 mm radiused corners (paper-safe) |
| **Layer Stackup** | F.Cu / In1.Cu / In2.Cu / B.Cu | Top Signals + GND pour / Solid GND / Solid +3V3 Plane / Bottom GND pour & trunks |
| **Compute Core** | Espressif ESP32-S3-MINI-1-N8 | Dual-core Xtensa LX7 @ 240 MHz, 512 KB SRAM, 8 MB Quad SPI Flash, Wi-Fi 4 + BLE 5.0 |
| **Audio Capture** | TDK InvenSense T3902 (MMICT390200012) | Bottom-port digital PDM MEMS microphone, 64 dBA SNR, 1.65–3.63 V; 0.5 mm acoustic port |
| **Audio Format** | Hardware I2S PDM → IMA ADPCM | 16 kHz 16-bit mono PDM stream compressed in real time to 4-bit IMA ADPCM (8 kB/s) |
| **Secondary Storage**| Winbond W25Q128JVPIQ (WSON-8) | 16 MB high-speed SPI NOR Flash dedicated for audio memo storage (~33+ min audio) |
| **Battery Charger** | Microchip MCP73832T-2ACI/OT | Single-cell Li-ion/LiPo linear charger, 4.20 V regulation, 100 mA charging current ($0.5\,\text{C}$) |
| **Battery Protection**| DW01A + FS8205A Dual NMOS | Overcharge (4.30 V), overdischarge (2.50 V), short-circuit cutoff; 3 µA operating draw |
| **System LDO** | Richtek RT9080-33GJ5 (TSOT-23-5) | 3.3 V output, 600 mA rating, 2 µA ultra-low quiescent current ($I_q$), 310 mV dropout |
| **Battery Monitoring**| Switched resistor divider (Q2/Q3) | AO3401A PMOS + 2N7002 NMOS high-side switch; $100\,\text{k}\Omega / 100\,\text{k}\Omega$ divider; $0\,\mu\text{A}$ sleep loss |
| **USB Interface** | 16-Pin USB Type-C Receptacle | USB 2.0 Full-Speed native USB-Serial-JTAG for debugging/flashing; USBLC6-2SC6 ESD clamp |
| **Standby Power** | Deep Sleep (RTC GPIO ext0 armed) | **~14–15 µA total quiescent standby draw** (>1 year shelf life on 150 mAh pouch cell) |

---

## System Schematic Overview

The design is organized hierarchically across 7 functional KiCad schematic sheets, separating compute, audio sensing, power conversion, flash memory, and user I/O into clean logical domains:

<p align="center">
  <img src="{{ '/assets/images/smart-bookmark-esp32s3/schematic_overview.png' | relative_url }}" alt="Smart Bookmark Hierarchical Schematic Overview" width="100%" />
</p>

*Figure 3: KiCad 10 top-level hierarchical block diagram linking MCU, Audio, Storage, Power, UI/Test, and USB sub-sheets.*

---

## Deep Dive: Engineering Design Decisions

### 1. Ultra-Low Standby Power Architecture (~15 µA Total)

A portable bookmark spends >99% of its operational life in sleep mode tucked between book pages. To guarantee months of battery endurance on a miniature 150–200 mAh LiPo pouch cell without requiring a physical toggle switch, the power system was engineered for microamp-level quiescent conservation:

<p align="center">
  <img src="{{ '/assets/images/smart-bookmark-esp32s3/schematic_power.png' | relative_url }}" alt="Smart Bookmark Power Management Schematic" width="100%" />
</p>

*Figure 4: Complete power subsystem: USB-C input with ESD protection, MCP73832 LiPo charger, DW01A/FS8205A battery protection, RT9080-33GJ5 ultra-low Iq LDO, and zero-leakage switched battery fuel divider.*

- **Elimination of Power-Path Diode Leakage:** Rather than placing a Schottky OR-ing diode in the power path—whose reverse leakage current (often 10–50 µA at elevated ambient temperatures) would exceed the sleep draw of the entire system—the system load is connected directly to the protected battery cell. When USB-C is plugged in and inactive, the firmware enters deep sleep so battery charge current drops cleanly below the MCP73832's 7.5 mA termination threshold.
- **Switched Battery Fuel Gauge:** Continuous resistor dividers across a lithium battery introduce an unavoidable 15–40 µA parasitic drain. This design utilizes a high-side P-channel MOSFET (`Q2`, AO3401A) gated by an N-channel MOSFET (`Q3`, 2N7002) driven by `BAT_SENSE_EN` (GPIO16). The divider is only powered during brief 50 ms ADC sampling intervals, dropping sleep leakage to **0.0 µA**.
- **Gated Microphone Supply:** Digital MEMS microphones typically consume 10–25 µA even in standby/power-down. In this design, the T3902 microphone's $V_{\text{DD}}$ pin is powered directly from an ESP32-S3 GPIO (`MIC_PWR`, GPIO41) through an RC filter ($100\,\Omega / 1\,\mu\text{F} / 100\,\text{nF}$). In sleep mode, GPIO41 is driven low, removing all power from the acoustic front end.

| Subsystem State in Deep Sleep | Typical Quiescent Current | Basis |
| :--- | :--- | :--- |
| ESP32-S3 RTC Core (ext0 wake armed) | 8.0 µA | Espressif ESP32-S3 Datasheet (Table 6-7) |
| RT9080-33GJ5 Low-Dropout Regulator | 2.0 µA | Richtek RT9080 Specification ($I_q$ typical) |
| DW01A Battery Protection IC | 3.0 µA | Fortune Semiconductor DW01A Datasheet |
| MCP73832 Reverse Leakage | 0.15 µA | Microchip MCP73832 Specification |
| W25Q128JV SPI NOR Flash (Deep Power-Down) | 1.0 µA | Winbond W25Q128JV Datasheet |
| Switched Battery Divider / T3902 Mic / LED | 0.0 µA | High-side PMOS isolated & unpowered |
| **Total Measured/Calculated Standby Current** | **~14.15 µA** | **>14 months standby on 150 mAh cell** |

With an average daily duty cycle of ten 20-second recordings (~2.2 mAh) plus a BLE data synchronization burst (~1.3 mAh), the total daily energy consumption is ~3.5 mAh. A single 150 mAh pouch cell provides **over 6 weeks of continuous daily usage** between charges.

---

### 2. High-Fidelity Audio Front-End & Dual-Stage Storage

- **TDK InvenSense T3902 MEMS Microphone:** The T3902 is an ultra-low-noise bottom-port digital PDM microphone with 64 dBA SNR and an acoustic overload point (AOP) of 120 dB SPL. Its bottom-port design allows sound to enter cleanly through a 0.5 mm plated hole on the reverse side of the PCB, shielding the acoustic inlet from dust and debris while remaining completely flush against book pages.
- **Hardware PDM-to-PCM Decoding:** The ESP32-S3's hardware I2S peripheral captures PDM data directly, applying integrated hardware sinc filtering to decimate and convert the 1-bit high-frequency PDM bitstream into 16-bit linear PCM audio at 16 kHz without taxing the dual Xtensa CPU cores.
- **Real-Time IMA ADPCM Compression:** The incoming 16-bit PCM stream (32 kB/s raw) is compressed in real time using an optimized 4-bit IMA ADPCM encoder (8 kB/s). This yields a 4:1 compression ratio while preserving speech intelligibility and voice clarity.
- **Dual-Tier Storage Architecture:** Audio memos are buffered onto an external **Winbond W25Q128JVPIQ 16 MB NOR Flash** (`U6`) mounted via dedicated Quad-SPI lines, holding up to 33 minutes of compressed voice memos. In addition, the ESP32-S3-MINI-1's internal 8 MB flash dedicates 4.4 MB to a LittleFS partition (holding an additional 8.5 minutes of overflow storage). The device seamlessly fails over between external and internal flash.

---

### 3. Mechanical & Single-Sided PCB Layout Constraints

<p align="center">
  <img src="{{ '/assets/images/smart-bookmark-esp32s3/placement_top.png' | relative_url }}" alt="Top Layer Placement Map" width="55%" />
  <img src="{{ '/assets/images/smart-bookmark-esp32s3/placement_back.png' | relative_url }}" alt="Bottom Layer Placement & Battery Zone" width="43%" />
</p>

*Figure 5: Placement maps showing high-density top-side assembly (left) and the reserved bottom-side battery cradle with 2×3 pogo-pin programming interface (right).*

- **Strict Single-Sided SMT Placement:** To ensure the bookmark lies flat against paper pages without rocking or puncturing book paper, 100% of the active and passive SMT components are populated on the top layer (`F.Cu`).
- **Recessed Battery Cradle Target:** The bottom layer (`B.Cu`) features a defined 25 × 30 mm silk boundary designated for double-sided adhesive attachment of an ultra-thin lithium polymer pouch cell. All vias within this footprint are tented to eliminate the risk of shorting against the cell's aluminum casing.
- **RF Antenna Keep-Out Zone:** The ESP32-S3-MINI-1 module is placed flush with the top edge of the board, positioning the meandered inverted-F antenna (MIFA) to project outward from the book head. A mandatory 5.0 mm keep-out zone across all 4 copper layers is strictly maintained around the antenna to ensure omnidirectional BLE radiation.
- **Direct Pogo-Pin Test & Flashing Matrix:** A 2×3 test pad array (`GND`, `3V3`, `EN`, `BOOT`, `TX`, `RX`) on the bottom side enables rapid automated factory programming and production testing via spring-loaded pogo-pin test fixtures.

<p align="center">
  <img src="{{ '/assets/images/smart-bookmark-esp32s3/route_top.png' | relative_url }}" alt="Top Copper Routing" width="55%" />
  <img src="{{ '/assets/images/smart-bookmark-esp32s3/route_bot.png' | relative_url }}" alt="Bottom Copper Routing" width="43%" />
</p>

*Figure 6: High-density 4-layer copper routing: clean orthogonal top-layer signal routing (left) and bottom-layer power distribution trunks with solid ground pour (right).*

---

## Design Verification & Manufacturing Readiness

The hardware design was verified against commercial fabrication and assembly rules:

- **KiCad Native Checks:** Passed with **0 Design Rule Check (DRC) violations, 0 Electrical Rules Check (ERC) warnings, and 0 unconnected nets**.
- **JLCPCB SMT Compatibility:** All selected components were cross-referenced against current JLCPCB parts stock. Resistors, capacitors, and switching transistors utilize JLCPCB Basic Parts to minimize reel-loading setup fees.
- **Manufacturing Outputs:** Fully generated manufacturing package including:
  - Gerber RS-274X copper, soldermask, silkscreen, and drill files (`fab/smart-bookmark-esp32s3_gerbers_JLCPCB.zip`).
  - JLCPCB-formatted Bill of Materials (`fab/BOM_JLCPCB.csv`).
  - Automated Pick-and-Place Component Placement List (`fab/CPL_JLCPCB.csv`).
  - Full 3D STEP mechanical model (`fab/smart-bookmark-esp32s3.step`).
  - Assembly drawings and schematic documentation (`fab/smart-bookmark-esp32s3_schematic.pdf`).

---

## Firmware Architecture & BLE Protocol

The accompanying ESP-IDF / PlatformIO firmware (`firmware/`) implements a complete event-driven state machine:

```
                    ┌───────────────────────────────┐
                    │          DEEP SLEEP           │
                    │   (RTC Ext0 Wake on BTN_N)   │
                    └──────────────┬────────────────┘
                                   │ Short Press (< 2 s)
                                   ▼
                    ┌───────────────────────────────┐
                    │       AUDIO RECORDING         │
                    │  - Enable MIC_PWR & MIC_CLK   │
                    │  - I2S PDM Hardware Sampling  │
                    │  - Real-Time IMA ADPCM Encode │
                    │  - Stream to W25Q128 Flash    │
                    │  - Pulsing Status LED         │
                    └──────────────┬────────────────┘
                                   │ Button Release / Timeout
                                   ▼
                    ┌───────────────────────────────┐
                    │     CLIP FINALIZATION         │
                    │  - Flush buffers & update TOC │
                    │  - Power down MIC & Flash     │
                    │  - Re-enter Deep Sleep        │
                    └───────────────────────────────┘
```

When a long button press (> 3 s) is detected, the device boots the BLE 5.0 subsystem and advertises a custom GATT data transfer service. The companion mobile application can query the recorded clip index, stream ADPCM voice memos over high-throughput BLE notifications, synchronize real-time clock timestamps, and monitor battery voltage.
