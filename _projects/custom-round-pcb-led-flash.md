---
title: "Round LED Flash — Annular Camera Ring Strobe"
date: 2026-10-21 08:00:00 -0700
tags: [hardware, power, boost, supercapacitor, led-driver, esp32, kicad]
thumbnail: /assets/images/round-led-flash-thumb.png
excerpt: "An 84 mm circular annular LED camera strobe with 4.8 V / 2.5 F balanced supercapacitor storage, 21 V synchronous boost, dual 200 mA current sinks, hardware timing interlocks, and ESP32-C3 BLE."
---

An advanced, hardware-interlocked **annular ring LED camera flash and video light** designed in **KiCad 10.0.7** to fit concentric around camera lens barrels. Engineered within an **84.0 mm outer diameter by 55.0 mm central opening** circular form factor, the board combines a **supercapacitor-buffered boost powertrain**, dual independently regulated linear current sinks driving a tunable white COB (2700 K warm / 6500 K cool), hardware-enforced timing and safety interlocks, and Bluetooth Low Energy (BLE 5.0) control via an **ESP32-C3**.

**Status:** Rev A Engineering Prototype Candidate. Schematic, component selection (100% MPN coverage across 94 unique parts), component placement, locked high-current power routes, and mathematical boundary verification models are complete. Physical fabrication and testing are pending first-article bench bring-up.

[View project repository](https://github.com/xmyoot/custom-round-pcb-led-flash) · [Schematic PDF](https://github.com/xmyoot/custom-round-pcb-led-flash/blob/main/output/pdf/Round_LED_Flash_Schematic.pdf) · [Engineering design review](https://github.com/xmyoot/custom-round-pcb-led-flash/blob/main/docs/DESIGN_REVIEW.md) · [Bring-up and qualification plan](https://github.com/xmyoot/custom-round-pcb-led-flash/blob/main/docs/BRING_UP_AND_QUALIFICATION.md)

---

## Board & 3D Assembly Overview

<p align="center">
  <img src="/assets/images/round-led-flash-3d.png" alt="Round LED Flash 3D Isometric Raytraced Render" width="100%" />
</p>

*KiCad 10 3D raytraced assembly render showing Eaton 5.0 F supercapacitor cans, dual TO-263 CAT4101 linear sinks, MP3435 boost converter, USB-C input, and outer-edge ESP32-C3 RF keepout.*

<p align="center">
  <img src="/assets/images/round-led-flash-layout.png" alt="Round LED Flash Annular Ring Layout" width="100%" />
</p>

*Annular ring 4-layer layout (84.0 mm OD × 55.0 mm ID, 14.5 mm radial width) showing high-current switching loops, supercapacitor placement, converter routing, and RF antenna keepout.*

The mechanical brief specified an 84 mm circular board with a 55 mm central lens aperture, leaving an annular circuit corridor of only **(84 − 55) / 2 = 14.5 mm** radial width. An initial client concept suggested an 18 × 70 mm battery cutout in the PCB edge; mechanical chord analysis demonstrated that an 18 mm wide rectangle cannot fit within a 14.5 mm band without cutting through the lens barrel or severing the annular ground ring. The architecture deliberately departs from that cutout by mounting the 18650 cell in an insulated cradle behind the lens plane, preserving complete ring rigidity and uninterrupted copper return paths.

---

## Technical Specifications & Architecture

| Subsystem | Implementation | Key Specifications |
| :--- | :--- | :--- |
| **PCB Form Factor** | 84.0 mm OD × 55.0 mm ID, 4 layers | 1.6 mm FR-4, 1 oz copper, ENIG finish, matte black soldermask |
| **Microcontroller Core** | Espressif ESP32-C3-WROOM-02-N4 | 32-bit RISC-V @ 160 MHz, BLE 5.0, 4 MB Flash, dedicated RF keepout |
| **Primary Energy Buffer** | 2× Eaton 5.0 F / 2.7 V (HV1020) | Series configuration: 2.5 F effective, 4.8 V nominal, 28.8 J stored energy |
| **Supercapacitor Charger**| Analog Devices LTC3225 | Charge pump with active cell balancing; 60 mA constant-current charge rate |
| **LED Boost Converter** | MPS MP3435GL-Z synchronous boost | 4.8 V input to 21.07 V output, 16 A internal switch, 4 A fuse F2 protection |
| **Current Regulation** | 2× onsemi CAT4101 linear sinks | 200 mA nominal per channel ($R_{\text{SET}} = 2.49\text{ k}\Omega$) for warm and cool channels |
| **Optical Emitter** | Bridgelux Vesta Tunable COB | Off-board 2700 K / 6500 K dual-channel COB on central heatsink (`J4`/`J5`) |
| **Hardware Flash Window**| Analog Devices LTC6993-1 | Non-retriggerable hardware one-shot: 1.403 ms nominal (1.449 ms max) cutoff |
| **Hardware Cooldown** | Analog Devices LTC6993-1 | Monostable timer enforcing 1.258 s minimum cooldown between flashes |
| **Physical Power Cutoff** | Alpha & Omega AO4407A P-MOSFET | High-side 30 V switch isolating 21 V boost rail outside active flash window |
| **System Logic Supply** | TI TPS63070RNMR buck-boost | Regulated 3.3 V @ 500 mA continuous across full 2.7 V–4.35 V battery range |
| **Battery Protection** | DW01A + 2× AO3400A N-MOSFETs | 2.79 V UVLO, 4.3 V overcharge cutoff, 3 A overcurrent trip, 4 A fuse F1 |
| **USB-C Charging** | GCT USB4085 + TI TUSB320LAI | Type-C receptacle with active CC negotiation (1.5 A / 3.0 A qualified) |
| **Linear Cell Charger** | TopPower TP4056 | 300 mA programmed cell charge current with dedicated pack NTC window |
| **Service Discharge** | Active dump FET (`Q34`) + passive bleed | Dual discharge path: safe <0.5 V storage level in <50 minutes |

---

## Powertrain & Hardware Safety Engineering

<p align="center">
  <img src="/assets/images/round-led-flash-sch.png" alt="Round LED Flash Schematic Excerpt" width="100%" />
</p>

*Master hierarchical schematic showing power isolation, boost converter, capacitor bank, and hardware timer interlocks.*

### 1. Supercapacitor Buffer & Boost Powertrain
To deliver high-power optical flashes without subjecting the 18650 Li-ion cell to aggressive pulse discharge, stored energy is accumulated slowly into two series-connected **Eaton 5.0 F / 2.7 V supercapacitors** (2.5 F effective capacitance, 4.8 V maximum charge, 28.8 J total stored energy). 
- An **ADI LTC3225** charge pump with active cell balancing charges the bank at a controlled 60 mA rate, preventing cell sag during charging.
- A high-efficiency **MPS MP3435GL-Z** boost converter steps the capacitor bank voltage up to 21.07 V, sized to drive the 18 V forward-voltage strings of the Bridgelux Vesta dual-channel COB LED.
- Current is regulated by two independent **onsemi CAT4101** linear constant-current sinks programmed to 200 mA nominal per channel.

### 2. Triple Hardware Interlocks & Independent Timers
Unlike hobbyist strobe designs that rely exclusively on microcontroller firmware delays, this architecture enforces optical flash limits strictly in hardware:
- **Maximum Duration Gate:** An **LTC6993-1** non-retriggerable timer enforces a maximum flash window of **1.403 ms nominal (1.449 ms maximum)**. Even if microcontroller firmware locks up with trigger lines asserted, the hardware timer automatically de-asserts power.
- **Physical High-Side Disconnect:** An **AO4407A P-channel MOSFET** directly cuts high-voltage power to the LED anodes outside of a validated hardware flash window.
- **Enforced Thermal Cooldown:** A second **LTC6993-1** timer locks out subsequent triggers for **1.258 s nominal**, guaranteeing that average thermal dissipation on the LED emitter and current sinks remains bounded.
- **Hardware Operating Inhibit:** Detection of an active USB-C VBUS input hardware-disables normal flash operation and capacitor charging, isolating the system during battery recharge.

### 3. Fail-Safe Service Bleed & Leakage Control
A charged 2.5 F supercapacitor bank at 4.8 V stores significant energy (28.8 J). Safe service handling requires reliable discharge:
- When system logic power is removed, active discharge transistor `Q34` turns on, rapidly dumping stored energy through power resistors.
- Because active FET discharge slows near gate threshold voltage, a permanent 330 Ω passive bleeder resistor (`R74`) guarantees continuous discharge down to safe bench voltages (<0.5 V) in under 50 minutes.

---

## Design Envelope & Mathematical Verification

The parametric calculations in `scripts/check_design_envelope.py` evaluate worst-case component tolerances, temperature corners (0°C to 40°C indoor operating envelope), and aged capacitor ESR:

```
PASS permanent_bleed_to_half_volt_model_within_60min  (2967 s ~ 49.5 min to <0.5 V)
PASS ADC_off_leakage_below_unpowered_pin_limit        (0.111 V max unpowered leakage vs 0.3 V max)
PASS cap_ready_reachable_at_charger_min               (4.38 V to 4.63 V vs charger output)
PASS boost_ready_reachable_at_boost_min               (19.07 V to 20.23 V vs 21.07 V boost target)
PASS input_above_converter_min_3V                     (3.18 V min loaded input voltage)
PASS current_below_min_average_limit                  (3.88 A peak input current vs 4.46 A limit)
PASS cap_terminal_above_uvlo                          (3.37 V loaded cap terminal vs 3.29 V UVLO)
PASS nominal_requested_1ms_plus_50us_inside_window    (1.05 ms pulse inside 1.357 ms min window)
PASS hardware_cutoff_model_below_2ms                  (1.403 ms nominal cutoff window)
PASS minimum_cooldown_exceeds_1s                      (1.217 s min cooldown enforced in hardware)
PASS open_ntc_halts                                   (Comparator halts charging on sensor fault)
PASS zeroC_always_cold_inhibited                      (Low-temperature charge lockout verified)
PASS fortyfiveC_always_hot_inhibited                  (High-temperature charge lockout verified)
```

Native KiCad electrical rule checks report **zero ERC errors and zero warnings**. Netlist parity between schematic symbols and placed footprints passes with **zero mismatches across all 237 physical components**.

---

## Component Density & Rev B Production Roadmap

At **237 SMD/THT components**, the Rev A design intentionally emphasizes **fail-safe hardware safety** over integration density. Over 45% of the total component count (108 parts) resides in the hardware interlock tree—employing 11 discrete 74LVC single-gate AND ICs, 4 precision comparators, 2 hardware supervisors, and 2 monostable one-shots to ensure zero software dependency for flash timing and over-temperature shutdown.

| Functional Subsystem | Rev A Discrete Count | Primary Parts | Rev B Production Target |
| :--- | :---: | :--- | :--- |
| **Hardware Interlocks & Timing** | 108 parts | 11× 74LVC1G08, 4× TLV3011, 2× LTC6993, 8× BJTs | **1× Renesas GreenPAK SLG46826** (~25 parts total) |
| **Current Regulation Sinks** | 12 parts | 2× onsemi CAT4101 (TO-263 packages) | **Dual switch-mode LED driver** (e.g. TI TPS92515) |
| **Power Management & Charging** | 46 parts | TP4056, DW01A, TPS63070, TLV76033, TUSB320 | **Integrated USB-C PMIC** (e.g. TI BQ25895) |
| **Boost Powertrain & Energy Storage** | 44 parts | MP3435, 2× Eaton HV1020 5.0 F caps, MLCC bank | Retained (high-density MLCC array) |
| **MCU & BLE Core** | 24 parts | ESP32-C3-WROOM-02-N4 + strapping/buttons | Retained (ESP32-C3 SoC / module) |

**Rev B Engineering Outcome:** Consolidating the discrete logic into a single 2 × 3 mm configurable mixed-signal array (GreenPAK) and transitioning from linear current sinks to switch-mode LED drivers reduces total component count from **237 to under 110 components** (>50% BOM reduction), eliminates massive TO-263 thermal dissipation on the annular ring, and dramatically lowers pick-and-place assembly cost for volume manufacturing.

---

## Deliverables & Prototype Bring-Up Plan

The project repository provides editable KiCad 10 sources, project-specific symbol and footprint libraries, complete BOM with verified manufacturer part numbers and datasheet links, design envelope verification scripts, and a staged prototype bring-up procedure:

1. **Initial Power-Off Checks:** Cold impedance sweeps from all rails to ground; inspection of QFN thermal pads and capacitor polarities.
2. **Logic & Supervisor Qualification:** Bench supply sweep (2.5 V to 4.2 V) verifying TPS63070 3.3 V regulation, supervisor threshold hysteresis, and zero ADC leakage during shutdown.
3. **Supercapacitor Charging & Balance:** Monitored first charge of Eaton HV1020 cells via LTC3225; balance voltage verification and active/passive bleed timing.
4. **Boost Converter Load Characterization:** Electronic load step tests verifying 21.07 V boost regulation, switch node ringing, and current-limit response.
5. **Optical Pulse Validation:** Photodiode and oscilloscope captures of warm, cool, and combined 1.0 ms pulses, confirming dual-channel current regulation and hardware timer cutoff.

---

## Tools & Attribution

Designed and verified using **KiCad 10.0.7**, [kicad-happy](https://github.com/aklofas/kicad-happy) for automated design review and cross-domain verification, Python for parametric boundary analysis, and Matplotlib for layout CAD visualization. Architecture, schematic synthesis, and verification scripts were developed with AI assistance; engineering decisions and qualification procedures are documented in the project repository.
