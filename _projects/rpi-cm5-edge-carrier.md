---
title: "CM5 Edge-Audio Carrier: Raspberry Pi Compute Module 5 Carrier Board"
date: 2026-10-21 10:00:00 -0700
tags: [hardware, kicad, high-speed, pcie, usb3, gigabit-ethernet, usb-pd, raspberry-pi, carrier-board]
thumbnail: /assets/images/cm5-carrier-thumb.png
excerpt: "A 100 x 85 mm, 4-layer impedance-controlled carrier for the Raspberry Pi CM5: USB-C PD power path, NVMe over PCIe, USB 3.0, Gigabit Ethernet and an I2S mic-array link. Length-matched pairs, DRC-clean, JLCPCB-ready."
---

A carrier board for the **Raspberry Pi Compute Module 5**, designed in **KiCad 10** for an always-on
edge-AI audio appliance. It includes:
- a **USB-C PD power path**
- **NVMe storage over PCIe**
- a **5 Gb/s USB 3.0** port
- **Gigabit Ethernet**
- an **I2S/I2C link** to a 6-microphone XVF3800 voice array

The 4-layer stack is impedance-controlled (JLC04161H-3313). Every high-speed pair is routed
coupled and length-matched, and the design closes with **0 DRC errors, 0 unconnected items and
0 schematic-parity issues**.

**Status:** Design complete. The full JLCPCB fabrication and assembly package is generated
(Gerbers, drills, BOM, CPL, STEP). Fabrication and bring-up are pending.

[View project repository](https://github.com/xmyoot/rpi-cm5-edge-carrier) · [Schematic PDF](https://github.com/xmyoot/rpi-cm5-edge-carrier/blob/main/docs/CM5_Edge_Carrier_schematic.pdf) · [Fabrication package](https://github.com/xmyoot/rpi-cm5-edge-carrier/tree/main/fab)

---

## Board & 3D Assembly

<p align="center">
  <img src="/assets/images/cm5-carrier-3d.png" alt="CM5 carrier isometric 3D render" width="100%" />
</p>

*KiCad 10 raytraced assembly with the Raspberry Pi CM5 seated on its two Hirose DF40 receptacles.
The front edge has the stacked USB-A, the USB-C service port and the M.2 socket. Power input and
the 5 V buck are at the top-left.*

<p align="center">
  <img src="/assets/images/cm5-carrier-3d-rear.png" alt="CM5 carrier rear 3D render" width="100%" />
</p>

*Rear edge: RJ45 MagJack, mic-array header, Qwiic, barcode-scanner and debug UARTs, and the fan connector.*

---

## Specifications

| Subsystem | Implementation | Key specifications |
| :--- | :--- | :--- |
| **Compute** | Raspberry Pi CM5 (BCM2712) | 4x Cortex-A76, 2x Hirose DF40C-100DS-0.4V receptacles |
| **PCB** | 100 x 85 mm, 4 layers | JLC04161H-3313, 1.6 mm, ENIG; L1 sig / L2 GND / L3 PWR / L4 sig |
| **Power input** | USB-C PD sink, WCH CH224K | Requests 12 V / 3 A; SMAJ15A TVS; 9 V UVLO interlock |
| **5 V rail** | TI TPS565208 sync buck | 5.02 V / 5 A; budget 4.7 A (CM5 + USB + fan) |
| **NVMe rail** | Diodes AP3441 sync buck | 3.33 V / 2 A, enabled by the CM5's `PCIE_PWR_EN` |
| **Storage** | M.2 Key-M, 2230/2242 | PCIe x1 Gen2 (Gen3 capable), 85 ohm |
| **USB** | Stacked Type-A + AP22653 switch | 1x USB 3.0 (5 Gb/s) + 1x USB 2.0, 1.5 A current limit |
| **Ethernet** | TRJG0926HENL MagJack | 1000BASE-T, 100 ohm, TPD4EUSB30 ESD on all 4 pairs |
| **Service port** | USB-C (USB 2.0 device) | rpiboot eMMC flashing, USB gadget console, USBLC6-2 ESD |
| **Audio link** | 2x6 shrouded header | I2S (33 ohm source-terminated) + I2C + reset/IRQ |
| **Size** | 90 footprints | 231 nets, 474 vias, 3.4 m of routed copper |

---

## Architecture & Schematic

<p align="center">
  <img src="/assets/images/cm5-carrier-block.png" alt="System block diagram" width="100%" />
</p>

*Root sheet: system block diagram and design notes. The schematic is hierarchical, with Power,
Compute Module, High-Speed IO and Peripheral IO sheets.*

Every peripheral maps to a native CM5 interface, so the carrier needs no bridge ICs. The
interesting engineering is in the power path and in routing the high-speed interfaces off
0.4 mm-pitch connectors.

<p align="center">
  <img src="/assets/images/cm5-carrier-sch-power.png" alt="Power sheet" width="100%" />
</p>

*Power sheet: PD sink, 5 V / 5 A synchronous buck with a UVLO interlock, and the 3.3 V NVMe buck
with feed-forward compensation.*

- **PD with an interlock.** The CH224K requests 12 V. The buck's enable divider puts turn-on near
  9 V, so a 5 V-only charger leaves the rail off instead of browning out the SoC.
- **SoC-sequenced NVMe power.** The M.2 rail stays off until the CM5 bootloader asserts `PCIE_PWR_EN`.
- **ESD sized to the threat.** TVS on PD VBUS, low-capacitance arrays on every GbE pair and on the
  service port. The 5 Gb/s pair stays stub- and via-free, following the Raspberry Pi CM5 IO reference.

<p align="center">
  <img src="/assets/images/cm5-carrier-sch-highspeed.png" alt="High-speed IO sheet" width="100%" />
</p>

---

## Layout & Signal Integrity

| Top layer (L1) | Bottom layer (L4) |
| :---: | :---: |
| ![Top copper](/assets/images/cm5-carrier-layout-top.png) | ![Bottom copper](/assets/images/cm5-carrier-layout-bottom.png) |

**Floorplan.** The CM5 is rotated so its PCIe/USB connector faces the front edge. The USB 3.0
jack, the service USB-C and the M.2 socket sit directly at those pins. The Ethernet pins face
the rear edge, where the MagJack sits.

**Impedance control.** The trace geometries were solved for L1 over 0.0994 mm 3313 prepreg (εr 4.1):

| Interface | Target | Width / gap | Calculated |
| :--- | :--- | :--- | :--- |
| PCIe TX/RX/REFCLK | 85 ohm diff | 0.20 / 0.15 mm | 85.6 ohm |
| USB 3.0 SS & USB 2.0 | 90 ohm diff | 0.18 / 0.15 mm | 90.8 ohm |
| 1000BASE-T MDI | 100 ohm diff | 0.16 / 0.18 mm | 100.0 ohm |

**Measured from the routed copper:**

| Pair | Length P / N | Intra-pair skew | Vias |
| :--- | :--- | :--- | :--- |
| PCIe TX | 52.33 / 52.33 mm | 0.01 mm | 0 |
| PCIe RX | 99.69 / 99.69 mm | 0.00 mm | 0 |
| USB 3.0 SSTX / SSRX | 11.37 / 11.33 and 9.20 / 9.20 mm | 0.04 / 0.00 mm | 0 |
| PCIe REFCLK | 50.02 / 49.76 mm | 0.26 mm | 4 (forced crossing) |

The PCIe and SuperSpeed data pairs never leave L1 and sit over an unbroken L2 ground. Only
REFCLK changes layer, because its pin order forces one crossing of the TX/RX pairs.

**Power and manufacturability.**
- Both bucks are laid out by hand on top-layer copper pours, with tight input/switch-node loops.
- A custom staggered fanout (0.4/0.2 mm vias, 3 levels) escapes the 0.4 mm DF40 pads, which keeps
  the whole board on JLCPCB's standard multilayer process.
- About 210 stitching vias tie the outer GND pours to the L2 plane.

<p align="center">
  <img src="/assets/images/cm5-carrier-top.png" alt="Top view render" width="100%" />
</p>

---

## Engineering Trade-offs

1. **CM5 instead of Jetson Orin NX.** It meets the functional brief at a fraction of the power and
   cost. NVMe runs on PCIe x1 instead of x4.
2. **The lower USB-A port is USB 2.0.** The far contact row of a stacked USB3 jack can only be
   reached by threading pairs between the near-row pins. One clean 5 Gb/s port covers the
   5G-modem use case.
3. **There is no on-board battery charger.** The PD-to-buck path keeps the board focused, and a
   UPS stage would be a daughterboard.

## Design Flow

The schematic, board and fab outputs are generated from a single Python design database. A
netlist checker proves that the KiCad-exported connectivity matches the design intent, and
routing is scripted with
[KiCadRoutingTools](https://github.com/drandyhaas/KiCadRoutingTools): impedance-controlled,
length-matched diff pairs first, then general routing. Running `build_all.sh` regenerates the
whole DRC-clean board in about four minutes.

**Tools:** KiCad 10.0.7 · Python / pcbnew · KiCadRoutingTools · kicad-happy · JLCPCB DFM
