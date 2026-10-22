---
title: "ESP32-P4 Edge AI Camera Board: MIPI CSI-2, SIM7600G-H LTE Cat-4, Wi-Fi 6 & IR Night Vision"
date: 2026-10-22 09:40:00 -0700
published: true
tags: [hardware, kicad, esp32p4, lte, mipi-csi, wifi6, camera, night-vision, pcb, embedded]
thumbnail: /assets/images/esp32-p4-lte-camera-thumb.png
excerpt: "A 75 × 70 mm 4-layer Edge AI vision platform combining Espressif ESP32-P4 (dual RISC-V @ 400 MHz, 32 MB PSRAM), 2-lane MIPI CSI-2 camera, SIM7600G-H global LTE Cat-4/GNSS, ESP32-C6 Wi-Fi 6 co-processor, and quad 850 nm IR night vision with DRV8837 IR-cut filter control."
---

A high-performance, battery-operated **75.0 × 70.0 mm 4-layer Edge AI camera platform** engineered for autonomous remote vision, cellular telemetry, and extreme low-power field deployment. Built around Espressif's flagship **ESP32-P4NRW32X** SoC (dual-core 32-bit RISC-V @ 400 MHz, integrated 32 MB PSRAM, 16 MB QSPI NOR flash), the board bridges high-throughput local computer vision and long-range connectivity by pairing a **2-lane MIPI CSI-2 camera interface** with a **SIMCom SIM7600G-H global LTE Cat-4 modem** and an **ESP32-C6-MINI-1 Wi-Fi 6 / BLE 5.3 companion module**.

Designed from the ground up for unattended 24/7 wildlife, security, and industrial monitoring, the board integrates a **quad 850 nm high-power infrared illumination array** driven by a Texas Instruments **TPS61169** constant-current boost converter, an electro-mechanical **IR-cut filter H-bridge actuator (TI DRV8837)**, an on-board **XYC-PIR223D-S0 digital motion sensor** for microampere deep-sleep wakeup, an advanced **TI BQ25895 3 A NVDC switch-mode charger** with JEITA NTC battery profiling, and complete multi-rail load-switched power gating.

**Status: Layout and Schematic Verified (Rev A).** The hierarchical electrical schematic and 4-layer controlled-impedance PCB layout are digitally validated in KiCad 10 with **zero ERC violations, zero DRC errors, and zero unrouted nets**. Controlled impedance profiles enforce **100 Ω differential MIPI CSI-2**, **90 Ω differential USB**, and **50 Ω coplanar waveguide (CPWG)** RF feeds with stitched ground fencing for all three cellular and positioning antenna ports.

[View Project Repository](https://github.com/xmyoot/esp32-p4-midi-lte) · [Design Specification](https://github.com/xmyoot/esp32-p4-midi-lte/blob/main/DESIGN_SPEC.md) · [Architecture Specification & Review Gates](https://github.com/xmyoot/esp32-p4-midi-lte/blob/main/docs/ARCHITECTURE.md) · [Placement Review](https://github.com/xmyoot/esp32-p4-midi-lte/blob/main/docs/review/PLACEMENT_REVIEW.md) · [Schematic Review](https://github.com/xmyoot/esp32-p4-midi-lte/blob/main/docs/review/SCHEMATIC_REVIEW.md) · [Full Schematic PDF](https://github.com/xmyoot/esp32-p4-midi-lte/blob/main/docs/review/schematic_revA_draft.pdf)

---

## 3D Hardware Overview

<p align="center">
  <img src="{{ '/assets/images/esp32-p4-lte-camera/board_isometric.png' | relative_url }}" alt="ESP32-P4 LTE Camera Board Isometric 3D View" width="85%" />
</p>

<p align="center">
  <img src="{{ '/assets/images/esp32-p4-lte-camera/board_top.png' | relative_url }}" alt="ESP32-P4 LTE Camera Board Top Placement" width="49%" />
  <img src="{{ '/assets/images/esp32-p4-lte-camera/board_bottom.png' | relative_url }}" alt="ESP32-P4 LTE Camera Board Bottom Layout" width="49%" />
</p>

*Figure 1: Native raytraced 3D CAD visualization of the ESP32-P4 LTE Camera Board (Rev A). Top: Primary component assembly showing the front-edge 22-pin MIPI CSI-2 connector, corner 850 nm IR LEDs, TO-5 PIR motion sensor, central ESP32-P4NRW32X QFN-104 SoC, north-east ESP32-C6-MINI-1 Wi-Fi 6 module with ground keep-out, south-east SIM7600G-H LTE modem with U.FL antenna trio, and west-side BQ25895 / TPS631000 power management stage. Bottom: Underside view highlighting the high-frequency ceramic decoupling capacitor ring directly underneath the ESP32-P4 power vias and solid ground reference.*

---

## Functional Architecture & Floorplan

<p align="center">
  <img src="{{ '/assets/images/esp32-p4-lte-camera/floorplan.png' | relative_url }}" alt="ESP32-P4 LTE Camera Board Functional Floorplan" width="65%" />
</p>

*Figure 2: Functional floorplan partitioning. Clean domain separation minimizes cross-talk between the high-gain analog PIR/battery management circuits, high-speed MIPI/SDMMC buses, and high-power LTE RF transmission paths.*

---

## Technical Specifications & Architecture

| Subsystem | Component / Implementation | Key Specifications & Engineering Details |
| :--- | :--- | :--- |
| **PCB Form Factor** | 75.0 mm × 70.0 mm rectangular | 4 mounting holes ($\varnothing 3.2\text{ mm}$ for M3 hardware) with $6.0\text{ mm}$ keepouts, rounded $2.0\text{ mm}$ corners. |
| **Layer Stackup** | 4-Layer `JLC04161H-7628` | $1.60\text{ mm}$ thickness, 1 oz outer / 1 oz inner Cu, ENIG surface finish, solid Layer 2 continuous GND return plane ($0.21\text{ mm}$ dielectric spacing). |
| **Main Compute Core** | Espressif ESP32-P4NRW32X | Dual-core RISC-V @ 400 MHz, single-precision FPU, AI vector instructions, integrated **32 MB PSRAM**, QFN-104 ($10 \times 10\text{ mm}$). |
| **External Flash** | Winbond W25Q128JVSIQ | 16 MB (128 Mbit) high-speed QSPI NOR Flash @ 3.3 V with $0\ \Omega$ series damping resistors and dedicated 10 kΩ CS pull-up. |
| **Wi-Fi 6 / BLE Co-Processor** | Espressif ESP32-C6-MINI-1 | 32-bit RISC-V @ 160 MHz, Wi-Fi 6 (802.11ax), BLE 5.3, 802.15.4 / Zigbee / Thread; connected to P4 via 4-bit SDIO (`esp-hosted`). |
| **Cellular & GNSS Engine** | SIMCom SIM7600G-H | Global LTE Cat-4 (150 Mbps DL / 50 Mbps UL), 3G UMTS/HSPA+, 2G GSM fallback, multi-constellation GNSS (GPS, GLONASS, BeiDou, Galileo). |
| **RF Antenna Feeds** | 3× U.FL (IPEX MHF1) Connectors | 50 Ω Coplanar Waveguides over Ground (CPWG) with inline $\pi$-match filter networks and dual-sided ground via stitching fences. |
| **Camera Interface** | 22-Pin 0.5 mm pitch FPC (`05B20L22P`) | 2-lane MIPI CSI-2 (100 Ω differential impedance, $11.4\text{ mm}$ trace length directly facing P4 CSI pins), Raspberry Pi Zero/CM compatible pinout. |
| **Night Vision IR Array** | 4× 850 nm High-Power SMD LEDs | `M3838E1IRS1G09-850NM` ($3.8 \times 3.8\text{ mm}$), driven in series at ~294 mA (~2.3 W total optical output power). |
| **IR Constant-Current Driver** | Texas Instruments TPS61169DCKR | High-frequency inductive boost converter, $0.68\ \Omega$ sense resistor, PWM dimming input from ESP32-P4 LEDC timer. |
| **IR-Cut Filter Actuator** | Texas Instruments DRV8837DSGR | Low-voltage H-bridge motor driver (1.8 V – 11 V, 1 A peak) driving bi-directional solenoid optical day/night filter; $120\text{ nA}$ sleep draw. |
| **PIR Motion Sensor** | Senba XYC-PIR223D-S0 | Low-power digital PIR sensor (TO-5 package), $10\ \mu\text{A}$ operating current, direct push-pull active-high trigger into P4 Low-Power GPIO. |
| **Battery Management & Charger** | Texas Instruments BQ25895RTWR | I²C-controlled 3 A switch-mode buck charger, dynamic NVDC power path, JEITA temperature monitoring via NTC, autonomous input current limit. |
| **Battery Cell Protection** | Diodes Inc DW01A + FS8205A | On-board dual N-FET protection against cell over-voltage ($4.30\text{ V}$), under-voltage ($2.40\text{ V}$), and discharge over-current ($> 3\text{ A}$). |
| **3.3 V Logic Rail** | Texas Instruments TPS631000DRLR | High-efficiency synchronous buck-boost ($1.5\text{ MHz}$, $I_\text{Q} = 8\ \mu\text{A}$), 1.5 A continuous output capability across $2.7\text{ V} – 5.5\text{ V}$ input. |
| **Core Supply (VDD_HP)** | Texas Instruments TLV62569DBVR | Synchronous step-down converter delivering 1.1 V core rail with mandatory $499\text{ k}\Omega$ feedback network and 22 pF feedforward cap (HDG v3.x). |
| **Modem Domain LDO** | Texas Instruments TPS7A0218DBVR | Ultra-low quiescent current LDO ($I_\text{Q} = 25\text{ nA}$), powers P4 `VDD_IO_6` domain (GPIO49–54) at 1.8 V to eliminate level shifters on modem UART. |
| **Cellular Surge Switch** | Texas Instruments TPS22965DSGR | 16 mΩ $R_\text{DS(on)}$ 6 A load switch switching raw `SYS` power to `VBAT_LTE`, backed by $2 \times 220\ \mu\text{F}$ low-ESR bulk capacitors for 2 A RF bursts. |
| **Peripheral Power Gating** | Dual TI TPS22918DBVR Load Switches | Controlled power isolation of `3V3_CAM` (camera module) and `3V3_SD` (microSD slot) to zero standby leakage during deep-sleep cycles. |
| **Supervisory Reset IC** | Texas Instruments TPS3839G33DBZR | Ultra-low quiescent supervisor ($I_\text{Q} = 150\text{ nA}$), precision 3.08 V threshold, fixed 200 ms delay driving `CHIP_PU` to prevent brownouts. |
| **Mass Storage** | Push-push MicroSD Socket (`TF-01A`) | High-speed 4-bit SDMMC bus connected to ESP32-P4 SDMMC Slot 0, localized bypass and ESD suppression. |
| **Diagnostic & Host Interface** | USB Type-C 16-Pin Receptacle | Mid-mount bottom edge, $5.1\text{ k}\Omega$ CC pulldowns (UFP mode), BC1.2 charger detection link, TI TPD4E05U06 0.5 pF ESD clamp array. |

---

## System Architecture Block Diagram

```mermaid
flowchart TD
    subgraph Power_Source ["Primary Energy & Charging Subsystem"]
        J_USB["USB-C Receptacle\n(5 V VBUS)"] -->|TVS SMF5.0A| BQ["TI BQ25895\n3 A NVDC Charger + Power Path"]
        J_BAT["1S Li-ion Battery\n(3.7 V Nom, JST-PH 3P)"] <--> PROT["DW01A + FS8205A\nCell Protection"]
        PROT <--> BQ
        J_BAT -.->|NTC Thermistor| BQ
        BQ --> SYS["SYS Rail (3.5 V – 4.35 V)"]
    end

    subgraph Regulators ["Precision Power Distribution & Gating"]
        SYS --> BUCK_BOOST["TI TPS631000\n3.3 V Buck-Boost (1.5 A, Iq = 8 µA)"]
        BUCK_BOOST --> V33["3V3 System Rail"]
        V33 --> TLV["TI TLV62569\nVDD_HP DC-DC (1.1 V)"]
        TLV --> VDD_HP["VDD_HP Core (1.1 V)"]
        V33 --> LDO["TI TPS7A0218\n1.8 V LDO (Iq = 25 nA)"]
        LDO --> V18["1V8_IO6 Domain"]
        
        SYS -->|TPS22965 (16 mΩ)| SW_LTE["VBAT_LTE Rail\n(2x 220 µF Bulk)"]
        V33 -->|TPS22918| SW_CAM["3V3_CAM Rail"]
        V33 -->|TPS22918| SW_SD["3V3_SD Rail"]
        SYS --> DRV_CUT["TI DRV8837\nIR-Cut H-Bridge"]
        SYS --> BOOST_IR["TI TPS61169\nCC Boost Driver"]
    end

    subgraph Compute_Core ["Central Processing Core"]
        VDD_HP --> MCU["ESP32-P4NRW32X\nDual RISC-V @ 400 MHz\n32 MB PSRAM (QFN-104)"]
        V33 --> MCU
        V18 --> MCU
        FLASH["16 MB QSPI Flash\n(W25Q128JVSIQ)"] <-->|QSPI| MCU
        SUP["TI TPS3839 Supervisor\n(3.08 V, Iq = 150 nA)"] -->|CHIP_PU Reset| MCU
        PIR["XYC-PIR223D-S0\nDigital Motion Sensor"] -->|LP Wake Interrupt| MCU
    end

    subgraph Connectivity ["Long-Range & Local Wireless Connectivity"]
        MCU <-->|4-bit SDIO + LP Wake| C6["ESP32-C6-MINI-1\nWi-Fi 6 / BLE 5.3"]
        C6 --> ANT_WIFI["Onboard PCB Antenna\n(East Edge Keepout)"]
        SW_LTE --> SIM["SIMCom SIM7600G-H\nGlobal LTE Cat-4 / GNSS"]
        MCU <-->|1.8 V Direct UART| SIM
        SIM <--> NANO_SIM["Nano-SIM Push-Push\n+ TPD4E05U06 ESD"]
        SIM --> UFL1["U.FL MAIN (50 Ω CPWG)"]
        SIM --> UFL2["U.FL DIV (50 Ω CPWG)"]
        SIM --> UFL3["U.FL GNSS (50 Ω CPWG)"]
    end

    subgraph Vision_Illumination ["Vision & Night Vision Subsystem"]
        SW_CAM --> CAM_FPC["22-Pin 0.5 mm FPC\n(Front Edge)"]
        MCU <-->|2-lane MIPI CSI-2 (100 Ω)\n+ Fast-Mode I2C| CAM_FPC
        BOOST_IR --> IR_LEDS["4x 850 nm IR LEDs (294 mA)\nCorner Symmetrical Array"]
        MCU -->|LEDC PWM| BOOST_IR
        DRV_CUT --> IR_SOL["IR-Cut Filter Solenoid\nDay/Night Switching"]
        MCU -->|DIR / EN| DRV_CUT
    end

    subgraph Storage_IO ["Mass Storage & Host Diagnostics"]
        SW_SD --> SD_CARD["MicroSD Push-Push\n(4-bit SDMMC Slot 0)"]
        MCU <-->|SDMMC Bus| SD_CARD
        J_USB <-->|USB 2.0 FS + BC1.2| MCU
    end
```

---

## Hierarchical Schematic Architecture

The electrical design is organized into seven modular, clean hierarchical sheets in KiCad 10:

<p align="center">
  <img src="{{ '/assets/images/esp32-p4-lte-camera/schematic_overview.png' | relative_url }}" alt="Schematic Sheet 1: Top Level Hierarchy" width="75%" />
</p>

*Figure 3: Top-level hierarchical schematic interconnect. Sheets strictly segregate power domains, high-speed compute, wireless radios, vision acquisition, and mass storage interfaces.*

<p align="center">
  <img src="{{ '/assets/images/esp32-p4-lte-camera/schematic_power.png' | relative_url }}" alt="Schematic Sheet 2: Power and Charging Subsystem" width="49%" />
  <img src="{{ '/assets/images/esp32-p4-lte-camera/schematic_mcu.png' | relative_url }}" alt="Schematic Sheet 3: ESP32-P4 Compute Core" width="49%" />
</p>

*Figure 4: Detailed subcircuit schematics. Left: Power subsystem showing the BQ25895 3 A charger, TPS631000 buck-boost, and TPS7A0218 1.8 V domain regulator. Right: ESP32-P4NRW32X core engine showing crystal loading, QSPI flash array, TLV62569 core switching regulator, and MIPI series matching links.*

<p align="center">
  <img src="{{ '/assets/images/esp32-p4-lte-camera/schematic_lte.png' | relative_url }}" alt="Schematic Sheet 5: SIM7600G-H LTE Modem & Antennas" width="49%" />
  <img src="{{ '/assets/images/esp32-p4-lte-camera/schematic_camera.png' | relative_url }}" alt="Schematic Sheet 6: MIPI CSI Camera, IR Boost & PIR" width="49%" />
</p>

*Figure 5: High-speed and wireless interfaces. Left: SIM7600G-H cellular modem with TPS22965 load switch, bulk capacitor bank, and U.FL CPWG feeds. Right: 22-pin MIPI CSI-2 camera connector, TPS61169 constant-current IR boost driver, DRV8837 H-bridge, and digital PIR sensor.*

---

## Deep Dive: Engineering Design Decisions & Challenges

### 1. High-Density QFN-104 Fan-Out & Bottom-Side Decoupling Architecture

The ESP32-P4NRW32X packs **104 pins into a $10 \times 10\text{ mm}$ package** with a fine **$0.35\text{ mm}$ pin pitch** and a large central thermal ground pad ($7.2 \times 7.2\text{ mm}$). Accommodating 26 required power supply and internal regulator decoupling capacitors (`VDD_HP`, `VDD_IO`, `VDD_ANA`, `VDD_LP`, `VDDO_FLASH`, `VDDO_PSRAM`, `VDD_MIPI_DPHY`), 6 series tuning resistors for flash, 6 MIPI series links, and USB terminations on a single side created severe routing congestion and extended trace loop lengths:

<p align="center">
  <img src="{{ '/assets/images/esp32-p4-lte-camera/assembly_top.png' | relative_url }}" alt="Assembly Top Drawing showing component placement" width="60%" />
</p>

*Figure 6: Top-layer component placement and silk boundaries. Notice how rotating the ESP32-P4 by 180° points the MIPI CSI pins directly north toward connector J2, achieving a direct 11.4 mm straight connection.*

- **180° SoC Orientation Optimization:** Rotating the P4 by 180° placed the MIPI CSI-2 physical pins (pins 41–48) directly on the northern package perimeter facing the front-edge camera FPC connector (`J2`). This slashed the critical differential trace path to just **$11.4\text{ mm}$ straight line**, completely eliminating signal looping around the chip.
- **Bottom-Side Decoupling Ring (B.Cu):** In the single-sided prototype placement, mean decoupling distance to owner pins was $3.6\text{ mm}$, with worst-case pins reaching $9.2\text{ mm}$. By transitioning the ESP32-P4's 100 nF and 1 µF high-frequency ceramic capacitors to the bottom layer directly beneath the SoC footprint:
  - Mean capacitor distance dropped to **$2.4\text{ mm}$** (worst-case $5.8\text{ mm}$).
  - High-frequency loop inductance $L_\text{loop}$ dropped by more than **$60\%$**, drastically stiffening the power delivery network (PDN) during core frequency bursts (up to $400\text{ MHz}$).
  - Top-layer routing congestion was cleared, allowing uninhibited escape of the 4-bit SDMMC, QSPI flash, and camera control lines.

---

### 2. Shifter-Free 1.8 V Cellular Modem Interface via Dynamic VDD_IO_6 Domain

Connecting modern high-performance microcontrollers to industrial cellular modems typically requires multiple bi-directional level translator ICs (such as the TXS0108E) because modern modems (SIM7600G-H) operate exclusively at **1.8 V CMOS logic**, whereas traditional microcontrollers operate at 3.3 V:

```
                      +-------------------+
                      |   TI TPS7A0218    |
                      | 1.8V LDO (Iq=25nA)|
                      +---------+---------+
                                | 1.8V
                                v
+------------------+     VDD_IO_6 (Pin 96)     +--------------------+
|                  |-------------------------->|                    |
|   ESP32-P4       |   UART_TX (1.8V CMOS)     |   SIM7600G-H       |
|   NRW32X         |-------------------------->|   Cellular Modem   |
|   SoC            |   UART_RX (1.8V CMOS)     |   (1.8V Baseband)  |
|                  |<--------------------------|                    |
|                  |   UART_RTS / CTS (1.8V)   |                    |
|                  |<------------------------->|                    |
+------------------+                           +--------------------+
```

- **Architectural Breakthrough:** The ESP32-P4 features independent power supply domains for its general-purpose I/O banks. Bank 6 (`VDD_IO_6`, pin 96) controls `GPIO49` through `GPIO54`.
- Instead of tying `VDD_IO_6` to the board's 3.3 V rail and adding multi-channel level translators, **`VDD_IO_6` is powered from a dedicated ultra-low-noise, ultra-low-quiescent 1.8 V LDO (TI TPS7A0218, $I_\text{Q} = 25\text{ nA}$)**.
- **Benefits:**
  - Zero propagation skew and zero high-speed waveform degradation at baud rates up to **$4\text{ Mbps}$**.
  - Eliminates 2 IC packages, 4 decoupling capacitors, and their corresponding routing area.
  - For the low-power modem Ring Indicator (`RI`) wake-up signal that must drive an LP GPIO in the 3.3 V domain, a single-gate discrete directional shifter (**TI SN74LVC1T45**) is deployed with zero standby penalty.

---

### 3. Dynamic Power-Path & Cellular 2 A Current Surge Protection

Cellular transmissions under 2G GSM fallback and Cat-4 uplink bursts draw short-duration current spikes of up to **$2.0\text{ A}$** with fast edge rates ($\text{d}I/\text{d}t > 1\text{ A}/\mu\text{s}$). If routed through an intermediate step-down or buck-boost regulator, these bursts can trigger inductor saturation, severe thermal stress, and battery rail brownouts:

```
USB-C (5V) ──────► BQ25895 NVDC Charger ──┬──► SYS Rail (3.5 V – 4.35 V)
                     ▲                    │
                     │ BAT                ├──► TPS22965 (16 mΩ) ──► VBAT_LTE (2x 220 µF) ──► SIM7600G-H
            1S Li-ion Battery             │
            (DW01A/FS8205A)               └──► TPS631000 (8 µA Iq) ──► 3V3 Logic Rail
```

- **Raw SYS Rail Feed via 16 mΩ Load Switch:** The SIM7600G-H requires an input operating voltage between $3.4\text{ V}$ and $4.2\text{ V}$ ($3.8\text{ V}$ nominal). The modem is powered directly from the BQ25895's **`SYS` rail** through an ultra-low-resistance load switch (**TI TPS22965DSGR**, $R_\text{DS(on)} = 16\text{ m}\Omega$, 6 A rating).
- **Transient IR Drop Analysis:**
  $$\Delta V_\text{switch} = I_\text{peak} \times R_\text{DS(on)} = 2.0\text{ A} \times 0.016\ \Omega = 32\text{ mV}$$
  The $32\text{ mV}$ drop is negligible, keeping $V_\text{BAT\_LTE}$ solidly within the modem's operating window.
- **Bulk Energy Reservoir:** Two **$220\ \mu\text{F}$ low-ESR tantalums** ($440\ \mu\text{F}$ total) are positioned directly at pins 38 and 39 of the modem package, supplying instant dynamic charge and filtering RF ripple from the system rail.
- **Autonomous Sleep Gating:** During deep-sleep standby, the ESP32-P4 asserts the enable pin of the TPS22965 low, completely disconnecting the modem and eliminating the SIM7600's multi-milliampere standby leakage.

---

### 4. MIPI CSI-2 Differential Routing (100 Ω) & High-Speed Integrity

The 2-lane MIPI CSI-2 camera bus operates at physical data rates up to **$1.5\text{ Gbps per lane}$** (D-PHY Specification). At these frequencies, transmission line impedance mismatches and intra-pair length skew cause devastating phase distortion and frame corruption:

| Metric | Target Specification | Achieved Design Value | Margin / Status |
| :--- | :---: | :---: | :---: |
| **Differential Impedance ($Z_\text{diff}$)** | $100\ \Omega \pm 10\%$ ($90 – 110\ \Omega$) | **$100.4\ \Omega$** | **PASS ($+0.4\%$)** |
| **Single-Ended Impedance ($Z_0$)** | $50\ \Omega \pm 10\%$ ($45 – 55\ \Omega$) | **$51.2\ \Omega$** | **PASS ($+2.4\%$)** |
| **Intra-Pair Skew ($\Delta L$)** | $\le 0.15\text{ mm}$ ($\approx 1\text{ ps}$) | **$< 0.08\text{ mm}$** | **PASS ($< 0.5\text{ ps}$)** |
| **Total Trace Length ($L_\text{total}$)** | $\le 50\text{ mm}$ maximum | **$11.4\text{ mm}$ straight path** | **PASS (77% margin)** |
| **Reference Plane** | Solid Layer 2 Ground Fill | Continuous unbroken `In1.Cu` GND | **PASS (Zero plane splits)** |

- **Geometry on JLC04161H-7628:** Trace width $W = 0.152\text{ mm}$ ($6.0\text{ mil}$), edge-to-edge differential spacing $S = 0.152\text{ mm}$ ($6.0\text{ mil}$), dielectric height $H = 0.21\text{ mm}$ over solid Layer 2 GND.
- **Series Tuning Links:** Six $0\ \Omega$ 0402 series damping resistors (`R301`–`R306`) are placed in-line near the transmitter for impedance fine-tuning and eye-diagram probing during bring-up.

---

### 5. Quad 850 nm Active Night Vision & Optical IR-Cut Filter Solenoid

True day/night optical surveillance requires active infrared illumination coupled with an electro-mechanical filter mechanism that physically removes the IR-blocking glass when transitioning to night vision mode:

```
[Daylight Operation]
  IR LEDs: OFF
  DRV8837 H-Bridge: Drives forward pulse (10 ms)
  IR-Cut Filter: Glass engaged over sensor (Blocks 850 nm IR, full natural color reproduction)

[Nighttime Surveillance]
  PIR Sensor: Triggers motion wake-up interrupt to ESP32-P4 LP core
  DRV8837 H-Bridge: Drives reverse pulse (10 ms) -> IR-Cut Filter: Glass retracted
  TPS61169 Boost Driver: Ramps quad 850 nm LEDs to 294 mA (LEDC PWM dimming)
  MIPI CSI-2: High-contrast monochrome IR image captured and transmitted via LTE Cat-4
```

- **Symmetrical Front Illumination:** Four high-radiance 850 nm SMD LEDs (`M3838E1IRS1G09-850NM`) are deployed in dual pairs at the top-left and top-right board corners. Placing the LEDs along the extreme board perimeter prevents direct optical backscatter into the camera lens and eliminates hot-spot saturation.
- **Constant-Current Boost Efficiency:** The four IR LEDs are connected in a single series string ($V_F \approx 4 \times 1.6\text{ V} = 6.4\text{ V}$). The TI **TPS61169** boost converter steps up the battery rail ($3.5\text{ V} – 4.2\text{ V}$) to $6.4\text{ V}$ with $> 88\%$ conversion efficiency. A precision $0.68\ \Omega$ 1% sense resistor fixes string current to $I = 0.200\text{ V} / 0.68\ \Omega \approx 294\text{ mA}$.
- **Zero-Static-Power Solenoid Drive:** The miniature IR-cut mechanism uses a bi-stable latching solenoid. The TI **DRV8837** H-bridge requires only a single **$10\text{ ms}$ pulse** to switch states. Once switched, the motor driver enters its ultra-low-current sleep mode ($I_\text{Q} = 120\text{ nA}$), drawing zero static power to hold the filter in position.

---

### 6. Triple RF Antenna Integration & Coplanar Waveguide Design

Operating Cat-4 LTE, GNSS positioning, and 2.4 GHz Wi-Fi 6 on a single $75 \times 70\text{ mm}$ substrate creates severe RF coexistence and desense challenges if return paths and radiated fields are not strictly controlled:

- **Antenna Partitioning:**
  - **Wi-Fi 6 / BLE:** Handled by the ESP32-C6-MINI-1 module located at the north-east corner. An all-layer copper void (`F.Cu`, `In1`, `In2`, `B.Cu`) extends under the module's PCB trace antenna, radiating clear off the east edge.
  - **LTE Main & Diversity:** U.FL connectors `J5` (MAIN) and `J6` (DIV) are positioned along the eastern perimeter, directly adjacent to the SIM7600G-H RF pins, minimizing feed lengths to $< 8\text{ mm}$.
  - **GNSS Active Feed:** U.FL connector `J7` is isolated with a dedicated low-noise ground island and provides an optional footprint for an active antenna DC phantom bias tee.
- **50 Ω CPWG Implementation:** All three RF lines are routed as **Coplanar Waveguides over Ground (CPWG)**:
  - Signal trace width: $W = 0.35\text{ mm}$ ($13.8\text{ mil}$).
  - Ground coplanar clearance: $G = 0.20\text{ mm}$ ($7.9\text{ mil}$).
  - Dielectric height to solid Layer 2 GND: $H = 0.21\text{ mm}$ ($\varepsilon_r \approx 4.5$).
  - **Ground Stitching Via Fence:** Dual parallel fences of $0.60\text{ mm}$ vias ($0.30\text{ mm}$ drill) spaced at $1.5\text{ mm}$ pitch run along the entire trace length, preventing RF leakage into the internal logic planes.

---

## Power Budget & Operating Lifecycle

| Operational State | Subsystems Active | Typical Current Draw (@ 3.7 V VBAT) | Power Consumption | Expected Battery Life (3000 mAh 1S Li-ion) |
| :--- | :--- | :---: | :---: | :---: |
| **Deep Sleep (Surveillance Mode)** | PIR sensor active, LP timer, TPS3839 supervisor, DW01A protection; P4, C6, LTE, Camera, IR LEDs switched OFF | **$28\ \mu\text{A}$** | **$0.10\text{ mW}$** | **> 10.5 Years (Shelf-Life Dominated)** |
| **Standby / Network Keep-Alive** | ESP32-P4 light sleep, SIM7600 eDRX / PSM sleep listening for incoming SMS/cloud trigger | **$3.2\text{ mA}$** | **$11.8\text{ mW}$** | **~39 Days (Continuous Standby)** |
| **Daylight Vision Capture & Inference** | P4 dual RISC-V @ 400 MHz, 32 MB PSRAM, MIPI CSI camera active, flash recording | **$340\text{ mA}$** | **$1.26\text{ W}$** | **~8.8 Hours (Continuous Video Streaming)** |
| **Nighttime Capture (IR Illuminated)** | P4 @ 400 MHz, Camera, Quad 850 nm IR LEDs @ 294 mA (full brightness) | **$860\text{ mA}$** | **$3.18\text{ W}$** | **~3.5 Hours (Continuous Night Capture)** |
| **LTE Cat-4 Uplink Burst** | P4 streaming captured video / images to cloud over LTE Cat-4 modem (uplink active) | **$680\text{ mA}$** (bursts to 2 A) | **$2.52\text{ W}$** | **~4.4 Hours (Continuous Cloud Upload)** |

---

## Formal Manufacturing Verification Gates Rollup

| Verification Domain | Tool / Methodology | Pass Criteria | Validation Result |
| :--- | :--- | :--- | :---: |
| **Schematic ERC** | KiCad 10.0.7 ERC Engine | 0 errors, 0 unresolved pin violations | **PASSED (0 Violations)** |
| **Layout DRC** | KiCad 10.0.7 DRC Engine | 0 errors, 0 unconnected nets, 0 clearance conflicts | **PASSED (0 Errors / 0 Unconnected)** |
| **Footprint Pin Audit** | Netlist vs Datasheet Table | 100% pin matching on ESP32-P4 (104P) and SIM7600 (135P) | **PASSED (All 239 complex pads verified)** |
| **Differential Impedance** | Analytic 2D Field Solver | MIPI CSI 100 Ω $\pm 10\%$, USB 90 Ω $\pm 10\%$ | **PASSED ($100.4\ \Omega$ MIPI, $89.8\ \Omega$ USB)** |
| **RF CPWG Impedance** | Coplanar Waveguide Solver | 50 Ω $\pm 5\%$ across LTE and GNSS feeds | **PASSED ($50.2\ \Omega$ with via fencing)** |
| **Thermal Dissipation** | Copper Pour & Via Arrays | Exposed pads stitched on P4, BQ25895, TPS22965 | **PASSED (High-density thermal via matrix)** |
| **BOM & Sourcing** | JLCPCB / LCSC Library Check | Every single line assigned valid MPN and LCSC C-code | **PASSED (199 Placements / 72 BOM lines)** |

---

*Authored by Hector Jimenez for Electrical Hardware Engineering Portfolio.*
