---
title: Cellular ePaper Device — ESP32-S3 and SIM7600G-H
date: 2026-10-21
published: true
tags: [hardware, esp32-s3, cellular, epaper, kicad, pcb]
thumbnail: /assets/images/cellular-epaper-revb.png
excerpt: "A four-layer engineering prototype combining LTE connectivity, a 2.9-inch ePaper display, and temperature/humidity sensing."
---

An engineering prototype for a battery-powered remote information display, combining an ESP32-S3-WROOM-1-N8, a SIM7600G-H LTE/GNSS modem, a Good Display GDEY029T94 296 × 128 panel, and an SHT40 sensor on a 100 × 80 mm four-layer PCB.

**Status: engineering prototype — CAD design complete; physical validation pending.** No assembled prototype, measured battery life, RF qualification, or successful display operation is claimed.

[Project repository](https://github.com/xmyoot/cellular-epaper-esp32s3-sim7600) · [Design report](https://github.com/xmyoot/cellular-epaper-esp32s3-sim7600/blob/main/DESIGN_REPORT.md) · [Validation record](https://github.com/xmyoot/cellular-epaper-esp32s3-sim7600/blob/main/design/VALIDATION.md)

![Revision B PCB CAD view; engineering prototype](/assets/images/cellular-epaper-revb.png)

*CAD rendering only. The modem and SIM socket body models are omitted; their lands remain visible. Physical fit is unverified.*

The redesign addresses errors found during a source and circuit audit: the modem symbol and 135-pad land pattern were rebuilt against the selected module, SIM protection was separated into individual lines, the ePaper boost circuit was reconstructed from the panel reference, and modem control signals were assigned explicit voltage-domain translation.

Power comes from a protected, externally charged single-cell battery. The modem and display have switched supplies; USB-C provides data/programming only. Removing the onboard charger simplifies the power architecture but makes offboard charging a system requirement. Battery life remains a measurement task, including regulator quiescent current, network registration, transmit bursts, and display-refresh energy.

Native KiCad review reports record zero ERC errors (one retained library-comparison warning), zero PCB DRC violations, zero open connections and no schematic/PCB mismatches. Four pad-overlapping vias require filled, capped and plated processing; fabricator review remains open.

The repository includes hierarchical schematics, an explicit pin/net map, a BOM with procurement status, native KiCad review reports, critical-connection regression checks, and a staged bring-up procedure. Previous fabrication files and renders are archived as historical material and are not manufacturing instructions for this revision.

The next milestones are independent footprint/contact checks, a selected fabricator stackup with impedance review, complete part qualification, and current-limited bench bring-up. Fabrication release remains contingent on documented validation results.
