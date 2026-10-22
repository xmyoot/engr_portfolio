---
title: "6 kW Commercial Induction Heater"
date: 2026-10-22 06:30:00 -0700
tags: [hardware, kicad, power-electronics, resonant-inverter, igbt, gate-driver, stm32]
thumbnail: /assets/images/commercial-induction-heater-thumb.png
excerpt: "Native KiCad power and control PCB for a 415 VAC three-phase, 6 kW induction-heating design: four-layer routing, isolated gate drives, STM32 control, and a fully populated 3D assembly visualization."
---

A **290 × 210 mm, four-layer power and control PCB** for a 415 VAC three-phase induction-heating system. This project combines high-voltage power conversion, isolated gate drive, analog protection, and embedded control in a native KiCad 10 design.

The latest update completes the assembly presentation: **all 52 component instances now have portable, repository-local 3D models**, with fresh isometric, top, and bottom renders. The model library and generation scripts are included with the hardware source.

[View hardware source](https://github.com/xmyoot/commercial-induction-heater-6kw) · [Schematic PDF](https://github.com/xmyoot/commercial-induction-heater-6kw/blob/master/fabrication/Commercial_Induction_Heater_6kW_Schematic.pdf) · [Fabrication exports](https://github.com/xmyoot/commercial-induction-heater-6kw/tree/master/fabrication)

## Populated assembly

*Native KiCad raytraced assembly view. The rectifier, DC-link capacitors, magnetics, IGBT modules, terminals, and control electronics now have visible component bodies. The routed slot separates the power and control regions.*

The models are **project-owned illustrative geometry derived from the custom footprint envelopes and pad positions**. Package heights, terminal hardware, and winding details are approximations. They communicate the assembly arrangement; verified supplier CAD is still needed for mechanical signoff. Fiducials and mounting holes intentionally have no component bodies.

## Power conversion and control

The intended power path starts with a three-phase input and EMI/transient filtering, followed by bridge rectification and a DC link. An IGBT half bridge drives the resonant load, while an STM32 controller provides switching commands and reads isolated current and temperature feedback.

| Subsystem | Design selection / target |
| --- | --- |
| Supply | 415 VAC, three phase |
| Output power | 6 kW target |
| Rectifier | VUO70-16NO7 module footprint |
| Power switches | FF75R12RT4 module footprints |
| Resonant operation | Nominal 31 kHz; 68 µH / 0.38 µF tank values |
| Gate drive | Isolated drivers with bipolar supply rails |
| Controller | STM32F407VET6 |
| Feedback and protection | Hall current sensing, temperature sensing, analog trip circuits |
| PCB | 290 × 210 mm, four copper layers |

These are design selections and operating targets. Efficiency, switching waveforms, ZVS operating range, protection latency, and full-power thermal behavior have not been demonstrated by measurements in this repository.

## Layout and isolation

![Top view of the fully populated PCB assembly](/assets/images/commercial-induction-heater-6kw/top_view.png)

*Top assembly view showing the relative size and placement of the power components, driver ICs, controller, and external interfaces.*

The layout partitions the power stage from the low-voltage control region and includes a physical isolation slot. Dedicated routing corridors carry driver, protection, and sensor signals around the power components. The four copper layers provide routing and plane regions for the power and control domains.

![Bottom view of the PCB routing and through-hole connections](/assets/images/commercial-induction-heater-6kw/bottom_view.png)

*Bottom view showing routing and through-hole connections. The model update leaves the electrical layout unchanged.*

## Verification and deliverables

The native PCB was checked again on **October 22, 2026** after the model update:

| Check | Result |
| --- | --- |
| PCB DRC | 0 violations, 0 unconnected items, 0 footprint errors under the committed settings |
| Schematic ERC | 0 errors, 42 existing library-symbol mismatch warnings |
| 3D coverage | 52 component instances; 24 local model files including a spare library footprint |
| Electrical layout preservation | Model declarations only; placement, copper, pads, nets, and outlines unchanged |

The [DRC report](https://github.com/xmyoot/commercial-induction-heater-6kw/blob/master/Commercial_Induction_Heater_6kW-drc.rpt) lists checks ignored by the project configuration. The [ERC report](https://github.com/xmyoot/commercial-induction-heater-6kw/blob/master/Commercial_Induction_Heater_6kW-erc.rpt) records the outstanding warnings.

The repository includes the native schematic and PCB, custom symbols and footprints, model-generation and rendering scripts, BOM, schematic PDF, Gerbers, and Excellon drill exports. The current work is a documented design and assembly visualization. Hardware bring-up, supplier footprint verification, mechanical integration, thermal testing, and compliance validation remain open before a production release.
