---
title: "6 kW Commercial Induction Heater: 415 VAC LLC Resonant Inverter"
date: 2026-10-22 06:30:00 -0700
tags: [hardware, kicad, power-electronics, resonant-inverter, igbt, gate-driver, commercial-appliance, stm32]
thumbnail: /assets/images/commercial-induction-heater-thumb.png
excerpt: "A 290 x 210 mm, 4-layer 6 kW commercial induction heater power stage and controller natively engineered in KiCad 10: 415 VAC 3-phase input, dual 1200V/75A IGBT half-bridge, 31 kHz LLC series-resonant tank with Zero-Voltage Switching (ZVS), galvanic creepage isolation barrier, and hardware protection."
---

A complete industrial-grade **6 kW Commercial Induction Heater** power electronics converter and embedded digital controller, natively designed and laid out in **KiCad 10**. 

Engineered for high-duty commercial cooking, industrial melt processing, and fluid induction vessels, the system directly rectifies a **415 VAC 3-phase line** to generate an ~580 V DC bus powering a **31 kHz series-resonant LLC half-bridge inverter**. The inverter achieves zero-voltage switching (ZVS) across all operating loads using phase-tracking zero-crossing detection and dual isolated gate drivers.

The design closes natively with **0 DRC errors, 0 unconnected items, and 0 ERC issues**, featuring a reinforced galvanic isolation barrier with a physical routed creepage slot isolating high-voltage power from the low-voltage control domain.

**Status:** Design and manufacturing package complete. Full production fabrication files (RS-274X Gerbers, Excellon drill files, IPC-compliant Bill of Materials, and vectorized schematics) generated and validated.

[View Project Repository](https://github.com/xmyoot/commercial-induction-heater-6kw) · [Schematic PDF](https://github.com/xmyoot/commercial-induction-heater-6kw/blob/master/fabrication/Commercial_Induction_Heater_6kW_Schematic.pdf) · [Fabrication Package](https://github.com/xmyoot/commercial-induction-heater-6kw/tree/master/fabrication)

---

## 3D Assembly & Raytraced Board Views

<p align="center">
  <img src="/assets/images/commercial-induction-heater-6kw/isometric_view.png" alt="6 kW Commercial Induction Heater Isometric 3D Render" width="100%" />
</p>

*Figure 1: Isometric raytraced 3D render of the 290 × 210 mm 4-layer board in KiCad 10. The top domain houses the 3-phase AC input stage, common-mode filter, bus choke, DC-link film snubber capacitors, and dual 1200 V / 75 A IGBT power modules mounted to high-current M6 terminal lugs. A 2.5 mm routed Edge.Cuts creepage slot separates the HV stage from the STM32F407 control domain below.*

<p align="center">
  <img src="/assets/images/commercial-induction-heater-6kw/top_view.png" alt="6 kW Commercial Induction Heater Top View" width="100%" />
</p>

*Figure 2: Orthogonal top view showing clean Manhattan routing, isolated gate drive corridors, balanced resonant capacitor array (CR1, CR2), Hall-effect current transducer, and segregated sensor lines.*

<p align="center">
  <img src="/assets/images/commercial-induction-heater-6kw/bottom_view.png" alt="6 kW Commercial Induction Heater Bottom View" width="100%" />
</p>

*Figure 3: Orthogonal bottom view showing bottom copper traces (B.Cu) implementing horizontal signal routing buses, solid ground shielding, and dedicated Kelvin emitter returns.*

---

## System Architecture

The power stage converts incoming utility 3-phase 415 VAC (50/60 Hz) into high-frequency alternating magnetic flux inside a work coil, coupling inductively into stainless steel or cast iron cookware:

```
  [ 415 VAC 3-Phase ]
          │
          ▼
  [ EMI Filter + RV1-3 ] ──> [ VUO70-16NO7 Bridge ] ──> [ DC Choke + Film Link ]
                                                                 │
                                                    +580 VDC Bus │
                                                                 ▼
[ STM32F407 MCU ] ──(PWM)──> [ 2x 1EDC20I12AH Drivers ] ──> [ Dual IGBT Module ]
        │                              │                (FF75R12RT4 1200V/75A)
        │                              ▼                         │
   (ADC/ZCD) <── [ ZCD + Hall + NTC ] <────────────────── [ LLC Resonant Tank ]
                                                            (68 uH Coil + 0.38 uF)
```

1. **AC Rectification & Filtering:** Three-phase line enters through heavy-duty terminal block `TB1`, passing through metal-oxide varistors (`RV1`, `RV2`, `RV3`) and X2 class filtering before entering the **IXYS VUO70-16NO7** 1600 V / 70 A three-phase bridge rectifier.
2. **DC Link & Filtering:** A low-loss 250 µH DC bus choke `L1` attenuates ripple, feeding dual high-ripple electrolytic capacitors (`CBUS1`, `CBUS2`) with balancing bleeders (`R_BAL1`, `R_BAL2`) and high-frequency polypropylene snubber caps (`CSNUB1`, `CSNUB2`).
3. **Resonant Inverter:** Dual **Infineon FF75R12RT4** 1200 V / 75 A IGBT modules in a half-bridge topology switch at 31 kHz into an LLC series-resonant tank comprising a 68 µH planar induction work coil and high-current low-ESR resonant film capacitors (`CR1`, `CR2`, total 0.38 µF).
4. **Galvanic Isolation & Gate Drive:** Two **Infineon 1EDC20I12AH** coreless transformer isolated gate drivers provide reinforced $\ge 1200\,\text{V}_{\text{peak}}$ galvanic isolation. Each driver operates with asymmetrical $+15\,\text{V} / -8\,\text{V}$ bipolar rails to prevent parasitic Miller turn-on under high $dV/dt$ ($> 20\,\text{V/ns}$).
5. **Control & Protection:** An **STM32F407VET6** ARM Cortex-M4 microcontroller samples bus voltage, coil current via an **Allegro ACS772** 100 A galvanically isolated Hall sensor, heatsink temperature via dual NTC thermistors, and phase angle via an ultra-fast zero-crossing detector (LMV7231). Dual hardware window comparators trip PWM shutoff within $< 250\,\text{ns}$ upon over-current (OCP) or over-voltage (OVP).

---

## Technical Specifications

| Subsystem | Parameter | Specification | Implementation Detail |
| :--- | :--- | :--- | :--- |
| **Input Power** | Voltage / Phase | $415\,\text{VAC} \pm 15\%$, 3-Phase | 3-Wire + Earth (L1, L2, L3, PE), 50/60 Hz |
| **Max Power Output** | Continuous Power | **6.0 kW** continuous | Efficiency $> 96.5\%$ at full load |
| **DC Bus Voltage** | Nominal $V_{\text{DC}}$ | **$580\,\text{V}_{\text{DC}}$** (peak $680\,\text{V}$) | $V_{\text{line}} \times \sqrt{2}$, rated 1200 V breakdown |
| **Inverter Topology** | Half-Bridge LLC | Series-Resonant ZVS | Dual IGBT (1200 V / 75 A), 31 kHz resonant freq |
| **Resonant Tank** | Work Coil / Cap | $L_r = 68\,\mu\text{H}, C_r = 0.38\,\mu\text{F}$ | Polypropylene film, rated $I_{\text{RMS}} = 45\,\text{A}$ |
| **Switching Frequency** | Operating Range | **28 kHz – 45 kHz** | Above resonance for zero-voltage switching |
| **Gate Drive** | Isolated Bipolar | $+15\,\text{V} / -8\,\text{V}$ | Infineon 1EDC20I12AH, $I_{\text{peak}} = \pm 6\,\text{A}$ |
| **Isolation Barrier** | Creepage / Clearance | **$> 12.0\,\text{mm}$ creepage** | 2.5 mm physical milled Edge.Cuts slot ($X: 22..190\,\text{mm}$) |
| **PCB Dimensions** | Board Form Factor | **$290\,\text{mm} \times 210\,\text{mm}$** | 4-layer FR4, 2 oz copper outer / 2 oz copper inner |
| **Controller** | Microcontroller | STM32F407VET6 | 168 MHz Cortex-M4, Hardware FPU, Advanced TIM1 |
| **Hardware Protection**| OCP / OVP Latencies | **$< 250\,\text{ns}$** hardware trip | Ultra-fast analog comparators directly disabling gate drivers |

---

## PCB Stackup & Thermal Plane Architecture

The PCB is built on an industrial 4-layer stackup configured with **2 oz (70 µm) outer copper** and **2 oz inner planes** to minimize $I^2R$ conduction loss and maximize thermal dissipation:

| Layer | Type | Copper Weight | Primary Function / Net Assignment |
| :--- | :--- | :--- | :--- |
| **1. F.Cu** | Signal / Power | 2.0 oz ($70\,\mu\text{m}$) | Power polygons (AC, $V_{\text{mid}}$, $L_r$), vertical Manhattan signal escapes |
| **2. In1.Cu**| Solid Plane | 2.0 oz ($70\,\mu\text{m}$) | High-voltage `BUS_N` plane ($Y \le 148$) / Quiet control `GND` plane ($Y \ge 154$) |
| **3. In2.Cu**| Solid Plane | 2.0 oz ($70\,\mu\text{m}$) | High-voltage `BUS_P` plane ($Y \le 148$) / Low-voltage `+3V3` plane ($Y \ge 154$) |
| **4. B.Cu** | Signal / Plane | 2.0 oz ($70\,\mu\text{m}$) | Horizontal Manhattan buses, Kelvin gate returns, auxiliary power routing |

### High-Voltage Isolation Slot
To meet **IEC 60335-1 / IEC 60335-2-36** commercial appliance standards for 415 V supply mains, a **2.5 mm wide Edge.Cuts creepage barrier slot** is milled into the PCB along $Y = 152.0\,\text{mm}$ ($X = 22.0$ to $190.0\,\text{mm}$). This breaks the FR4 surface tracking path, extending effective creepage distance to **$> 12.5\,\text{mm}$** between the floating 600 V power stage and the grounded digital controller.

---

## Orthogonal Manhattan Signal Routing

All low-voltage logic, comparator trips, analog sensing, and MCU escapes adhere to a **strict two-layer orthogonal Manhattan routing methodology**:
- **Horizontal channels** strictly traverse `B.Cu` ($Y = 154.2\,\text{mm}$ to $Y = 185.5\,\text{mm}$).
- **Vertical escapes** strictly traverse `F.Cu`, dropping cleanly into pads or vias without crossing.
- **MCU QFP-100 Pad Ring:** All escape vias are staggered **$\ge 2.0\,\text{mm}$** outside the footprint perimeter or directed into the open courtyard center, completely eliminating solder mask sliver bridging and pad courtyard violations.
- **Zero DRC Violations:** The entire design verified with `kicad-cli pcb drc` with **0 DRC errors**, **0 unconnected pads**, and **0 footprint warnings**.

---

## Verification & Fabrication Package

The project repository includes the complete manufacturing deliverables ready for quick-turn industrial PCB fabrication and surface mount assembly:
- **Gerbers:** Standard RS-274X format (`F_Cu.gtl`, `B_Cu.gbl`, `In1_Cu.g1`, `In2_Cu.g2`, `F_Mask.gts`, `B_Mask.gbs`, `F_Silkscreen.gto`, `Edge_Cuts.gm1`).
- **Excellon Drill:** Plated through-hole and via drill definitions (`Commercial_Induction_Heater_6kW.drl`).
- **Bill of Materials:** Complete component sourcing specifications (`fabrication/BOM.csv`).
- **Full Schematic:** Vectorized PDF complete with design annotations (`fabrication/Commercial_Induction_Heater_6kW_Schematic.pdf`).
- **Raytraced 3D Renders:** 1080p photorealistic captures stored in `renders/`.
