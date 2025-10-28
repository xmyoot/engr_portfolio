---
title: LM5116 Evaluation DC‑DC Board
date: 2025-10-27
tags: [power, hardware, lm5116, dcdc, evalboard]
thumbnail: /assets/images/lm5116-thumb.png
excerpt: "Compact evaluation board based on the LM5116 synchronous buck controller — wide input range, precision current sensing, and test points for power-system development."
---
A compact evaluation board built around the TI LM5116 synchronous buck controller. The design targets power-system development and validation for embedded and industrial applications where a robust, configurable DC‑DC stage is required.

Key features
- Controller: TI LM5116 synchronous buck controller (wide VIN range, high efficiency).
- Input range: designed for wide input (e.g., 6 V – 65 V) with input filtering and transient protection.
- Adjustable output: single-turn potentiometer for quick VOUT adjustment; pads for remote sense / Kelvin connections.
- Power stage: dedicated high-current MOSFET footprints, low‑ESR output capacitors, and a compact steel‑cored power inductor (visible on the board).
- Current sensing: precision shunt footprint and test points for differential current measurement; adds support for closed-loop current-limit testing.
- Testability: abundant labeled test points (TP#) for VIN, VOUT, MOSFET nodes, and gate-drive signals to simplify bench characterization and scope probing.
- Protection and reliability: input fuse footprint, transient suppression (TVS) area, and thermal reliefs for better heat dissipation.
- Connectivity: rugged screw terminals / banana-style posts for VIN and VOUT; additional pads for I2C/PMBus if telemetry is added.
- Prototyping area: silkscreened component positions for optional RC snubbers, EMI filter components, and measurement headers.

Electrical / mechanical notes
- Intended logic / control: primary power stage is independent of MCU logic; any telemetry or control interface should be level‑shifted appropriately for the target system.
- Thermal management: power MOSFETs and the inductor require copper pours and thermal vias for sustained high-power operation.
- Typical board size: compact evaluation form factor with mounting posts for bench fixtures; ensure adequate airflow for continuous high-load testing.

Suggested test and measurement procedures
- Start with low input voltage and no load; verify VOUT adjusts correctly with potentiometer before applying load.
- Characterize efficiency across input voltages and loads; probe switching node (HS/ LX) with a high-frequency differential probe.
- Validate current-limit performance using a programmable electronic load and monitor temperature rise of MOSFETs and inductor.

Use cases
- Power-stage validation for embedded controllers and motor drivers
- Rapid prototyping of power supply integration for sensors and actuators
- Educational bench tool for switching regulator behavior and measurements

Repository / more
- Design files, BOM, and test procedures: https://github.com/your/repo (replace with actual repo)
