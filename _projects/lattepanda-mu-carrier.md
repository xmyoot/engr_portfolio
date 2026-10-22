---
title: LattePanda MU Industrial Carrier
date: 2026-10-22
tags: [hardware, kicad, pcb, embedded, power, pcie]
thumbnail: /assets/images/mu-carrier-3d.png
excerpt: "A native KiCad 10, four-layer carrier combining LattePanda MU compute, ESP32-S3 supervision, PCIe Wi-Fi, protected power conversion and field interfaces. Designed and digitally checked; prototype testing pending."
---
A **150 × 100 mm, four-layer carrier** for a LattePanda MU N100 compute module, with an ESP32-S3 service controller, AX200/AX210 Wi-Fi, protected 24 V power, and low-voltage machine interfaces. The work covers architecture, connected schematics, PCB layout, assembly outputs and a prototype qualification plan.

**Status: designed, routed and digitally checked. Physical assembly, functional testing, thermal characterization and EMC qualification are pending.** The images are native KiCad renders, including the MU assembly. Several component models are simplified clearance envelopes; they are not enclosure tolerance signoff.

[Native design and release files](https://github.com/xmyoot/lattepanda-mu-carrier) · [Schematic PDF](https://github.com/xmyoot/lattepanda-mu-carrier/blob/main/reports/schematic.pdf) · [Design review](https://github.com/xmyoot/lattepanda-mu-carrier/blob/main/docs/design-review.md)

## Architecture

| Function | Implementation |
|---|---|
| Compute and service | Socketed MU N100; ESP32-S3 with UART programming and independent watchdog |
| Power | Fused, reverse-protected 24 V input; 12 V, 5 V and 3.3 V converter cascade |
| High-speed interfaces | PCIe / USB M.2 E-key 2230, USB 2.0 host, two-lane eDP adapter |
| Field connections | RS232, 3.3 V GPIO, 24 V / 2.2 A fan control, normally-open dry contact |
| PCB | Signal / GND / GND / signal; two SMT sides, seven bottom-edge terminals |

The [architecture and power budget](https://github.com/xmyoot/lattepanda-mu-carrier/blob/main/docs/architecture.md) define shared converter limits rather than adding every rail's maximum independently. The relay is a low-voltage dry contact for external equipment; this board is not a safety-rated plasma controller.

## Engineering decisions

I kept the compute and RF functions modular and concentrated the carrier on power, physical interfaces and serviceability. A reverse-polarity PMOS replaces the draft's series diode to reduce input dissipation. The fan receives a dedicated gate driver and flyback protection, while the relay and fan default off. The watchdog resets the service MCU, with firmware heartbeat behavior left as an explicit integration requirement.

The layout uses two internal ground references, an ESP32 antenna exclusion, module retention hardware and accessible rail test points. PCIe coupling capacitors sit beside the E-key socket. Differential pairs were routed and length-tuned using a model that includes via-barrel length; copper-only lengths are recorded separately. Factory impedance verification and real link testing remain necessary.

The display uses a documented custom adapter from the MU's separate 40-pin eDP connector. Its pin mapping, panel voltage and backlight contract are explicit, avoiding an assumption that all panel cables are interchangeable. The native schematic uses connected wiring across four readable sheets; the historical Atopile scaffold is retained separately.

![Top view of the carrier]({{ '/assets/images/mu-carrier-top.png' | relative_url }})

## Verification and remaining work

Final native KiCad checks reported **zero ERC violations, zero DRC violations, zero unconnected items and zero schematic/PCB parity differences** under the documented rules. An independent XML-netlist audit passed **752 declared physical-pin checks across 124 footprints**. The assembly package has **107 BOM parts**, with matching reference sets for the **91 SMT placements**.

The [review](https://github.com/xmyoot/lattepanda-mu-carrier/blob/main/docs/design-review.md) retains supplier and measurement tasks: via annular-ring acceptance, thermal-pad assembly, converter stability, effective capacitance, fan return currents, harness EMC, and exact display-cable compatibility. Automated analyzer warnings are triaged with their limitations; no thermal or EMC compliance claim is made. Parts lifecycle availability still needs confirmation at purchase.

The repository includes [Gerbers, BOM/CPL and a STEP assembly](https://github.com/xmyoot/lattepanda-mu-carrier/tree/main/manufacturing), plus a [current-limited bring-up procedure](https://github.com/xmyoot/lattepanda-mu-carrier/blob/main/docs/bring-up.md). The next milestone is a small prototype build with recorded rail, load-step, temperature, boot, link and fault-recovery measurements.

![Underside with service controller and module sockets]({{ '/assets/images/mu-carrier-bottom.png' | relative_url }})

## Tools and attribution

Developed with AI assistance through Codex. KiCad 10 provides the editable native design, ERC/DRC and renders. [kicad-happy](https://github.com/aklofas/kicad-happy) supports design review; [KiCadRoutingTools](https://github.com/drandyhaas/KiCadRoutingTools) and `pcbnew` support routing and layout automation. Tool revisions, source references, model limitations and licenses are documented with the design.
