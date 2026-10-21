---
title: Guarded pH Acquisition Module
date: 2026-10-21
tags: [hardware, analog, instrumentation, adc, esp32, kicad]
thumbnail: /assets/images/soil-ph-afe-3d.png
excerpt: "A 50 × 35 mm guarded pH analog front end with a 16-bit ADC, precision reference and ESP32-S3 bring-up firmware. CAD reviewed; physical validation pending."
---

A compact pH acquisition daughterboard designed around the **LMP91200**, **ADS112C04** and **REF3220**, with an external ESP32-S3 handling data logging and calibration. The project explores the practical requirements of measuring a high-impedance electrode: driven guards, clean surfaces, controlled reference startup and a traceable validation plan.

**Status:** Rev A engineering prototype. Schematic, routed PCB, manufacturing outputs and firmware source are complete. The board has **not been fabricated, assembled or bench tested**. Images are CAD renders, not hardware photographs; the BNC connector visualization follows its manufacturer drawing for major dimensions, with illustrative thread/socket details.

[View project repository](https://github.com/xmyoot/guarded-ph-acquisition-module) · [Schematic PDF](https://github.com/xmyoot/guarded-ph-acquisition-module/blob/main/output/review/schematic.pdf) · [Engineering review](https://github.com/xmyoot/guarded-ph-acquisition-module/blob/main/docs/implementation/design_review.md)

## Design scope

The starting brief described an analog-chain replacement in a larger soil instrument. With no original Altium files or mechanical design available, I scoped an independent **50 × 35 mm, two-layer bench module** and documented the assumptions. Motor control, battery management, wireless hardware and galvanic isolation remain external. This is a pH front end, not a completed soil-testing product or a reconstruction of the original board.

## Signal and power architecture

- **Electrode interface:** LMP91200 unity-gain buffer with driven guards on both PCB faces. The measuring input follows a 4.143 mm top-layer trace with no vias or test-point stubs.
- **Acquisition:** ADS112C04 differential conversion at gain 2, nominal 20 SPS, with a 2.048 V REF3220 reference. The calculated 31.25 µV/code is resolution, not demonstrated measurement accuracy.
- **Conditioning:** Matched 1 kΩ resistors and a 10 nF C0G differential capacitor; calculated filter pole of 7.96 kHz. Interference rejection and settling remain measurement tasks.
- **Startup:** TPS7A2033 provides the analog 3.3 V rail; TPS3839K33 delays reference enable and ADC reset release. Firmware then checks configuration and broad reference/supply diagnostics.
- **Host interface:** Keyed JST PH connector carries regulated 5 V, ground and 3.3 V I²C. An external ESP32-S3 logs raw values and stores two-point calibration with separately supplied sample temperature.

## Layout and review

![Top copper and drill rendering from exported Gerbers](/assets/images/soil-ph-afe-copper.png)

*Independent rendering of the exported top copper, outline and drills. This illustrates the guard region and deliberate ground exclusion around the electrode input.*

KiCad 10.0.7 reports **zero enabled ERC errors/warnings, zero DRC violations, zero unconnected pads and zero schematic-parity errors**. An additional audit checks every native pad net, preserves the guarded input geometry, excludes foreign filled copper beneath the input and confirms that no vias overlap component paste apertures.

Review caught two via/paste overlaps, which were corrected before the final exports. A subsequent visual review removed staircase micro-jogs: routed segments fell from **352 to 257**, shortening copper by **8.203 mm** while preserving the locked guarded input and reference routing. The BNC preview was rebuilt with visible threaded barrel, bayonet lugs and socket geometry. The tooling review also exposed stale analyzer-cache results; the final review uses fresh analysis files and native KiCad checks. Generic EMC warnings, intentional guard-mask openings and connector overhang are documented rather than treated as unexplained failures or silently dismissed.

## Deliverables and next tests

The repository contains editable KiCad sources with local libraries, schematic and assembly drawings, Gerbers and separate plated/nonplated drills, BOM and placement files, Arduino bring-up firmware, calculations, design decisions and a staged test procedure. Firmware compiled for ESP32-S3; it has not run against this hardware.

The next milestone is first-article assembly followed by startup, leakage, noise, linearity, settling, temperature/humidity and probe-calibration tests. The ≤0.01 pH electronics residual is an **unproven engineering objective**. Complete probe accuracy must be established separately.

The BNC shell is driven at approximately 1.024 V and must remain insulated from chassis/ground. This nonisolated prototype is intended for a floating sample on a controlled bench. Field integration needs separately engineered isolation and ESD/EMC protection with a measured leakage budget.

## Tools

KiCad, [kicad-happy](https://github.com/aklofas/kicad-happy) for design review, [KiCadRoutingTools](https://github.com/drandyhaas/KiCadRoutingTools) for selected routing, Gerbonara for fabrication-file inspection, and Arduino-ESP32 for host firmware. Architecture and implementation were developed with AI assistance; engineering judgments and remaining validation limits are recorded in the repository.
