---
title: ESP32-S3 Smart Bookmark & Voice Recorder
date: 2026-10-19
tags: [hardware, esp32-s3, ble, voice-recorder, audio, wearable]
thumbnail: /assets/images/smart-bookmark-thumb.png
excerpt: "Ultra-thin 4-layer bookmark form factor with ESP32-S3 BLE, Knowles MEMS microphone, 16 MB SPI flash audio logging, USB-C LiPo charging, and pogo-pin test matrix."
---
A commercial-grade, ultra-thin **smart bookmark and voice logging device** built around the **ESP32-S3-WROOM-1**. Designed to tuck seamlessly between book pages, the device allows readers to record voice memos, speech-to-text book highlights, and timestamps with a single tactile button click, syncing audio files over Bluetooth Low Energy (BLE 5.0) to a smartphone companion app.

- **GitHub Repository:** [xmyoot/smart-bookmark-esp32s3](https://github.com/xmyoot/smart-bookmark-esp32s3)
- **Form Factor:** 37.1 × 52.1 mm rectangular board with 2.0 mm rounded corners to prevent page tearing.
- **Layer Stackup:** 4-layer FR-4 (Signal / Ground / Power / Signal) with ENIG gold surface finish.

# Board Overview

<p align="center">
  <img src="/assets/images/smart-bookmark-iso.png" alt="Smart Bookmark 3D Isometric View" width="100%" />
</p>

The board features silk-screened functional block labels on top (`KEEP CLEAR`, `STATUS LED`, `16MB FLASH`, `BATT SENSE`, `RECORD SW1`, `U3 CHARGER`, `3V3 REG`) and a designated LiPo battery pouch silkscreen target (`CELL 25 × 30 mm`) on the bottom side alongside a 2×3 pogo-pin programming array.

![Smart Bookmark Top Layout](/assets/images/smart-bookmark-top.png)

![Smart Bookmark Bottom Battery Target](/assets/images/smart-bookmark-bottom.png)

# Key Features

- **Dual-Core Processing & Bluetooth:**
  - Espressif **ESP32-S3-WROOM-1** with dual-core Xtensa LX7 @ 240 MHz, vector instructions for on-device voice activity detection (VAD), and BLE 5.0.
  - Antenna overhang positioned at the top edge with unbroken keepout margins.
- **Audio Recording Subsystem:**
  - Knowles high-SNR digital PDM / MEMS microphone (`MK1`) with acoustic porting.
  - Hardware RC anti-aliasing and DC-blocking filters for crisp voice capture.
- **High-Speed Flash Storage:**
  - 16 MB high-speed SPI NOR Flash (`U6`) dedicated for audio sample buffering and offline memo retention.
- **Power Management & LiPo Battery Charger:**
  - Onboard linear LiPo charger IC (`U3`) configured for a thin 150–250 mAh pouch cell.
  - Ultra-low quiescent current 3.3V LDO regulator (`U5`) providing high PSRR for clean microphone audio.
  - Active battery voltage divider circuit (`U4` / `BATT SENSE`) for accurate state-of-charge calculation.
  - Mid-mount USB-C receptacle (`J1`) for charging and firmware updates.
- **Silkscreen & Testability:**
  - 2×3 pogo-pin test pad matrix on the bottom (`GND`, `3V3`, `EN`, `BOOT`, `TX`, `RX`) enabling fast factory programming without plugging cables.

# Electrical & Mechanical Notes

- **Page-Safe Form Factor:** Edge corners are radiused to $R = 2.0\text{ mm}$ to prevent snagging or damaging book paper during bookmark insertion.
- **Thermal & Routing:** Solid internal ground plane minimizes digital switching noise coupling into the sensitive MEMS microphone analog input. Power traces are routed with 0.4–0.6 mm width to handle peak Wi-Fi/BLE transmission bursts.
- **100% Routed:** 209 track segments, 112 vias, and 0 unrouted nets (DRC clean).

# Use Cases

- Audio book notes, student lecture annotation, and instant voice recording without a phone screen.
- Pocket dictation and field journal recording device.
- Low-power audio logging reference design for BLE-enabled embedded devices.
