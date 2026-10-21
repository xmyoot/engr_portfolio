---
title: ESP32-C6 E-Paper Calendar Carrier
date: 2026-10-21
tags: [hardware, esp32, e-paper, kicad, embedded, pcb]
thumbnail: /assets/images/esp32c6-epaper-carrier.png
excerpt: "A 60 × 50 mm two-layer carrier for a socketed XIAO ESP32C6 and a bare 7.5-inch e-paper panel, with custom bias circuitry, firmware, and documented design verification. Prototype hardware testing pending."
---
A USB-powered carrier connecting a removable **Seeed XIAO ESP32C6** to a bare **Good Display GDEY075T7**, a 7.5-inch, 800 × 480 monochrome e-paper panel. The project covers requirements, architecture, schematic, PCB layout, assembly outputs, calendar firmware, and a bench bring-up plan.

**Status: designed, routed, and digitally checked. No physical prototype has been assembled or tested.** The image above is a KiCad rendering of the carrier; the removable MCU module and display are not shown.

[Source repository and deliverables](https://github.com/xmyoot/esp32c6-epaper-calendar) · [Schematic PDF](https://github.com/xmyoot/esp32c6-epaper-calendar/blob/main/docs/EPD_C6_RevA-schematic.pdf) · [Publication review](https://github.com/xmyoot/esp32c6-epaper-calendar/blob/main/docs/PUBLICATION_REVIEW.md)

## Architecture and scope

The XIAO supplies USB programming, the MCU, wireless connectivity, and 3.3 V regulation. The carrier adds the panel's external boost/charge pump, a 24-pin bottom-contact FPC connector, local bypass capacitors, SPI source damping, reset/chip-select defaults, and measurement access. The display's integrated UC8179 controls the switching stage through GDR and RESE.

| Item | Implementation |
|---|---|
| PCB | 60 × 50 mm, two layers; 1.6 mm FR-4 / 1 oz copper specified |
| Panel | GDEY075T7, 7.5-inch, 800 × 480 monochrome |
| MCU | Socketed XIAO ESP32C6; USB power and programming |
| Display interface | 3.3 V SPI plus DC, reset, and BUSY; 1 MHz initial SPI clock |
| Assembly | 25 SMT components in 13 BOM groups; two hand-fitted socket strips |
| Firmware | Paged calendar rendering, labeled offline demo, optional Wi-Fi NTP, full refresh and panel hibernate |

## Engineering decisions

I kept the architecture small by using a documented module instead of duplicating its USB, regulator, and RF circuitry. The bare-panel interface still required a switching and charge-pump circuit, physical connector verification, and a power budget.

The original requested panel lacked its referenced datasheet, so I selected a comparable panel with documented circuitry and a matching firmware driver. I resolved conflicting rail names in the reference drawing by physical FPC pin number, keeping unused contacts isolated and the internal regulator output separate from external 3.3 V.

The switching and source/shunt current paths were explicitly placed and locked before automated routing. The board includes a two-layer antenna exclusion area, a removable display-supply link, six probe pads, and four M2.5 mounting holes. Firmware disables Wi-Fi before a refresh to reduce coincident supply demand, and includes bounded BUSY fault recovery.

## Verification and remaining work

Fresh native KiCad checks reported **zero ERC violations, zero DRC violations, and zero unconnected pads** under the documented project rules. An independent netlist/PCB audit passed all **94 declared pin checks**. BOM/CPL reference sets agree, and all **16 release hashes** match the checked source and exported files. The calendar firmware has a recorded successful ESP32-C6 compile.

The review retained risks requiring real hardware: shared 3.3 V supply transients, effective MLCC capacitance at bias, gate-drive ringing and sense integrity, exact flex mating, connector-edge assembly access, and antenna behavior near the display backplate. Automated EMC findings were individually triaged; they do not establish compliance. No thermal or EMC qualification is claimed.

The next milestone is a small prototype build with recorded rail, current, gate/sense, temperature, display-refresh, and fault-recovery measurements. The repository includes the [bring-up procedure](https://github.com/xmyoot/esp32c6-epaper-calendar/blob/main/docs/BRING_UP.md) and [manufacturing notes](https://github.com/xmyoot/esp32c6-epaper-calendar/blob/main/manufacturing/README.md).

## Tools and attribution

Developed with AI assistance through Codex. KiCad provides the native design and rule checks; [kicad-happy](https://github.com/aklofas/kicad-happy) supports review, and [KiCadRoutingTools](https://github.com/drandyhaas/KiCadRoutingTools) completes routing around the protected critical copper. Tool versions, source references, limitations, and licenses are documented in the repository.
