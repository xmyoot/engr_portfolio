---
title: "Automotive CAN AM/FM Radio Module: STM32G4 + Si4730 + TDA7388, 12/24 V"
date: 2026-10-22 13:00:00 -0700
published: true
tags: [hardware, kicad, automotive, can, stm32, stm32g4, si4730, tda7388, audio, rf, power, buck, 4-layer, pcb, embedded]
thumbnail: /assets/images/automotive-can-radio-module-thumb.png
excerpt: "A client-driven 100 × 75 mm 4-layer 'hidden' car radio: STM32G431 commanded over CAN, Si4730 AM/FM tuner, TDA7388 driving two 4 Ω speakers. Fused, reverse-battery and load-dump protected 9–32 V input, with a 14 V pre-regulator so the amplifier survives 24 V trucks. Wired single-sheet schematic; ERC 0, DRC 0."
---

A client-driven **hidden AM/FM receiver** for vehicles. It has no user interface: it takes commands over **CAN**, tunes a digital radio IC and drives **two 7" 4 Ω speakers** from an onboard amplifier. The client handles firmware. The brief asked for the hardware to:
- survive dirty **12 V and 24 V** vehicle power: TVS, load dump, inline fusing and audio ground-loop isolation;
- connect everything through **2 mm solder holes**, with no connectors.

I designed it natively in **KiCad 10** as one fully wired A2 schematic and a **100 × 75 mm, 4-layer** board, ready for JLCPCB fabrication and assembly.

**Status: Rev A, fabrication-ready engineering prototype.**
- ERC 0 · DRC 0 · schematic↔PCB parity 0 · every net routed.
- Reviewed with the [kicad-happy](https://github.com/aklofas/kicad-happy) analyzers, including a CISPR 25 automotive EMC pre-check.
- Placement legalized and signals routed with [KiCadRoutingTools](https://github.com/drandyhaas/KiCadRoutingTools); power runs through the planes.

[View Project Repository](https://github.com/xmyoot/automotive-can-radio-module) · [Design Notes](https://github.com/xmyoot/automotive-can-radio-module/blob/master/docs/design-notes.md) · [Verification Report](https://github.com/xmyoot/automotive-can-radio-module/blob/master/docs/verification.md) · [Schematic PDF](https://github.com/xmyoot/automotive-can-radio-module/blob/master/docs/can-radio-schematic.pdf)

---

## 3D Hardware Overview

<p align="center">
  <img src="{{ '/assets/images/automotive-can-radio-module/board_isometric.png' | relative_url }}" alt="CAN radio module isometric 3D view" width="85%" />
</p>

<p align="center">
  <img src="{{ '/assets/images/automotive-can-radio-module/board_top.png' | relative_url }}" alt="CAN radio module top view" width="49%" />
  <img src="{{ '/assets/images/automotive-can-radio-module/board_bottom.png' | relative_url }}" alt="CAN radio module bottom view" width="49%" />
</p>

*Figure 1: KiCad 3D renders of Rev A.*

The floor plan follows the signal flow:
- **Power entry (top-left):** solder holes → mini-blade fuse → TVS → reverse-blocking P-FET.
- **14 V amplifier buck (top-centre):** its hot loop is kept tight.
- **TDA7388 (right edge):** its heat-slug face is flush with the board edge, so a heatsink or the enclosure wall bolts straight to it. Speaker holes are at the top-right corner.
- **CAN (bottom-left).**
- **MCU (centre):** rotated so its FDCAN, I2C and SWD pins face their peripherals.
- **Tuner and antenna landing (bottom edge):** as far as possible from both switchers.

---

## Schematic

<p align="center">
  <img src="{{ '/assets/images/automotive-can-radio-module/schematic.png' | relative_url }}" alt="CAN radio module schematic" width="100%" />
</p>

*Figure 2: One A2 sheet, laid out by signal flow.*
- **Top row:** input protection → 14 V amplifier rail → 5 V buck → 3.3 V LDO, drawn as one continuous wired rail.
- **Middle row:** CAN → MCU → tuner → preamp → amplifier → speakers.
- **Wiring:** every block is wired point to point. Labels are used only for the MCU's I/O (CAN, I2C, SWD, amplifier control).

The sheet is generated from a Python description in which every part carries its intended pin→net map. The build exports **KiCad's own netlist** and diffs it pin for pin against that map (70 nets), so a drawing mistake fails the build instead of reaching the board.

---

## Technical Specifications

| Subsystem | Implementation | Key details |
| :--- | :--- | :--- |
| **PCB** | 100 × 75 mm, 4-layer, 1.6 mm | L2 solid GND · L3 split VBAT / +14 V / +3.3 V planes · 0.15 mm min track/space · no via-in-pad on signal pads · 100+ GND stitching vias · 3 fiducials · 4 × M3 unplated |
| **Input protection** | Keystone 3568 + 10 A mini-blade, SMCJ36CA, SQD50P06-15L | 9–32 V. The bidirectional TVS clamps transients and blows the fuse on a sustained fault. The P-FET blocks reverse battery; a 15 V Zener limits its V<sub>GS</sub> on 24 V. 60 V+ rating on everything that sees VBAT |
| **Amplifier rail** | TI TPS54560B-Q1, 2.1 MHz | Regulates to **14 V** on 24 V vehicles and passes through near 100 % duty on 12 V. Coilcraft XAL7030 3.3 µH, 1000 µF bulk at the amplifier |
| **Logic rails** | TI LMR16006Y-Q1 (5 V, 2.1 MHz) → TLV755 (3.3 V) | Both switchers above the AM band. 5 V feeds the CAN PHY, preamp bias and amplifier control |
| **MCU** | ST STM32G431CBT6 | FDCAN1 (PA11/12), I2C1 to the tuner, USART2 debug pads, VBAT sense on PA0, 8 MHz HSE for CAN timing, SWD on Tag-Connect TC2030-NL |
| **CAN** | NXP TJA1051T/3 | VIO = 3.3 V (no level shifter), SZNUP2105L ESD, 120 Ω termination on a solder jumper |
| **Tuner** | Skyworks Si4730-D60 | 32.768 kHz crystal. FM through 1 nF and AM through 470 nF into a 330 µH loading coil, from one antenna landing with < 1 pF ESD |
| **Preamp** | TI TLV9062, gain −3.3 | Brings the tuner's ~80–270 mV<sub>rms</sub> line out up to the TDA7388's full-output drive |
| **Amplifier** | ST TDA7388 (Flexiwatt25) | Channels 1 and 2 bridged into two 4 Ω speakers. NPN-held standby and mute for pop-free start-up |

---

## Engineering Design Decisions

### 1. Keep the amplifier inside its rating on 24 V trucks
The client asked for a TDA7388 **and** 12–24 V operation, but the TDA7388 is only rated for **18 V operating** (28 V absolute maximum). Rather than limit the module to 12 V, I added a 5 A automotive buck (TPS54560B-Q1) that regulates the amplifier rail to 14 V. On a 12 V car the buck simply sits in dropout and passes the battery voltage through, so there's no loss of output swing in the common case.

### 2. Keep switching noise out of the AM band
A typical 400–500 kHz buck puts its 2nd–4th harmonics right inside 530–1710 kHz AM. **Both converters run at 2.1 MHz**, which puts the fundamental and every harmonic above the AM band. The tuner and antenna sit on the opposite edge of the board from both switchers.

### 3. Swap the tuner for one with a public reference design
The client preferred the TEF6686 but allowed the Si4730. The TEF6686's full datasheet and reference layout are NDA-gated, so a production-safe pinout couldn't be confirmed. I chose the Si4730-D60 instead: its application circuit is public, it has a verified KiCad symbol and footprint, and it supports a crystal reference. Its lower line level is made up by a small inverting preamp, which also keeps the amplifier's signal ground isolated (next point).

### 4. Ground-loop isolation
1. Both speakers are **bridged (BTL)**, so neither lead is ground.
2. The TDA7388's signal ground (S-GND, AC-GND, SVR and input returns) is its own **AGND** net. It meets system ground at **one net tie** beside the preamp, the audio source.
3. The mounting holes are unplated. The silkscreen notes that the GND-connected heat slug needs an insulating pad on a chassis-bonded heatsink.

### 5. Power through planes, signals on the outer layers
Every SMD pad on GND, VBAT, +14 V or +3.3 V gets a short **fan-out via** into its plane. I wrote this step as a small pcbnew script. On 0.5 mm pitch pins the stub leaves straight off the pin tip and then turns ("dog-leg"), so it never crosses a neighbour. KiCadRoutingTools then routes only the signals and local power on F.Cu/B.Cu. The result is short, direct copper with no power spaghetti:

<p align="center">
  <img src="{{ '/assets/images/automotive-can-radio-module/layout_routing.png' | relative_url }}" alt="Routed copper, top and bottom" width="85%" />
</p>

*Figure 3: Outer-layer copper. Red is F.Cu and blue is B.Cu; pours are hidden.*
- Wide VBAT paths at top-left.
- 1.2 mm speaker outputs from the amplifier to the corner holes.
- A regular 5 mm GND stitching grid.

### 6. Verify with independent tools, then triage the findings
- **KiCad:** ERC, DRC with zone refill, and schematic↔PCB parity all report **0**.
- **kicad-happy, real findings fixed:** no fiducials (added 3) and no thermal vias under the reverse-battery FET (added tab pours and 16 vias).
- **kicad-happy, false positives documented:**
  - "5 V/3.3 V CAN crossing": the TJA1051T/3's VIO pin is at 3.3 V.
  - "missing termination": the solder jumper provides it.
  - "amplifier at the board edge": intentional, for the heatsink.
- **Open bring-up items** are listed in the verification report: buck compensation measurement, AM antenna tuning, and the TVS choice if the vehicle has no central load-dump suppression.

<p align="center">
  <img src="{{ '/assets/images/automotive-can-radio-module/layout_top_copper.png' | relative_url }}" alt="Top copper" width="49%" />
  <img src="{{ '/assets/images/automotive-can-radio-module/layout_bottom_copper.png' | relative_url }}" alt="Bottom copper" width="49%" />
</p>

*Figure 4: Top and bottom copper layers.*

---

## Deliverables
- Native KiCad 10 project with a project-local symbol and footprint library (TDA7388 Flexiwatt25 with an aligned 3D model, 2 mm direct-wire landings).
- Gerbers and drill files, a JLCPCB-format CPL, a BOM with manufacturer part numbers for every line, the board STEP model, and schematic and assembly PDFs.
- Design notes with every calculation (feedback dividers, UVLO, switching-frequency resistor, compensation, TVS and FET ratings) and a bring-up sequence.
- Fully scripted regeneration (`build_all.sh`, `export.sh`), so the design rebuilds from source.
