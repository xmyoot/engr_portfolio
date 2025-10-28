---
title: STM32 Dev Board
date: 2025-08-25
tags: [embedded, hardware, stm32, devboard]
thumbnail: /assets/images/stm32-thumb.png
excerpt: "Compact BluePill-compatible STM32 dev board optimized for sensor-focused, low-power projects — flexible power rails, and standard debug/serial interfaces."
---
A compact, BluePill-derived STM32 development board engineered for sensors and low-power embedded applications. The design prioritizes breadboard-friendly headers, robust power routing for external sensors, and common host interfaces for debugging and communication.

# Schematic
A high-resolution schematic showing power rails, USB power input, UART bridge connections, SWD header and I2C routing.

![STM32 Dev Board schematic](/assets/images/stm32-schematic.png)

# Key features
- MCU: STM32F103 — 32-bit ARM Cortex-M3, up to 72 MHz.
- USB power input with over-voltage and reverse-polarity protection for safe operation from host USB.
- Integrated USB-to-UART bridge (CH340/CP2102/FTDI selectable) exposed on a dedicated header for console debugging and bootloader access.
- Dedicated I2C interfaces: SDA/SCL exposed on both the main header and a secondary sensor header; includes 10 kΩ pull-ups to 3.3V with option to populate/remove for bus customization.
- Power rails for external sensors:
  - 3.3V regulated rail (on-board LDO capable of powering multiple low-power sensors).
  - 5V rail directly from USB (fused) for sensors that require higher voltage — with a jumper to isolate from 3.3V when needed.
  - GND plane and multiple GND pins for clean signal routing.
- Level-shifting options: footprint for bi-directional level shifters on I2C and UART lines for 5V peripheral support.
- Boot configuration jumper (BOOT0) and reset button for easy firmware flashing and recovery.
- SWD header (2x5) for hardware debugging and programming with ST-Link or compatible debuggers.
- User LED and status indicators for power and UART activity.
- Breadboard-compatible 0.1" pin headers with BluePill-aligned GPIO mapping for easy porting of existing designs.
- Prototyping area with footprint for an RTC coin cell holder and optional battery backup circuitry.

# Electrical / mechanical notes
- Logic level: 3.3V. External 5V sensors require level translation or the provided 5V rail and level shifters.
- On-board regulator supplies up to 500 mA to 3.3V rail (ensure thermal management for sustained loads).
- Typical board dimensions: ~53 x 21 mm (BluePill-compatible); mounting holes provided for M2 screws.

# Use cases
- Sensor acquisition nodes (I2C / UART sensors)
- Rapid firmware development and debugging with SWD
- Prototyping low-power embedded applications that require multiple power rails