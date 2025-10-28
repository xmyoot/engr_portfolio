---
title: Senior Design Project - Autonomous Trash Picking Robot (ATPR)
date: 2025-04-02
tags: [robotics, autonomous, lidar, computer-vision, prototype]
thumbnail: /assets/images/atpr-thumb.png
excerpt: "Autonomous mobile robot that navigates with LiDAR, detects bottles and oranges with computer vision, and collects/sorts waste into onboard bins."
---

The Autonomous Trash Picking Robot (ATPR) is a small, modular mobile robot built to reduce human labor for waste collection and encourage better environmental practices. ATPR autonomously navigates an environment using LiDAR-based mapping and obstacle avoidance, detects target waste items (bottles and oranges) using computer vision, and collects them using a robotic arm into onboard compost and recycle bins. Teleoperation via a PS4 controller provides a safety fallback for manual control.

# Key features
- Navigation & obstacle avoidance: LiDAR-based perception for safe autonomous traversal and telemetry for adaptable behavior.
- Computer vision: OpenCV with a webcam for object detection (oranges and bottles) used to trigger picking actions.
- Waste collection: small robotic arm and gripper that picks up detected objects and sorts them into onboard bins.
- Teleoperation & safety: PS4 controller for remote control, hardware power cutoff switches, and software fail-safes.
- Modularity: designed to be expanded for additional sensors, detection classes, or alternate end effectors.

# Design objectives
- Autonomous navigation using LiDAR while avoiding collisions and maintaining a safe stopping distance.
- Reliable object detection and collection for a defined set of waste items.
- Full system built under a budget of $700 and completed within six months.
- Onboard power source and the ability to receive human commands as a safety precaution.

# Validation & testing
- Obstacle avoidance: measured stopping/avoidance distance; 0.25 m determined as a safe stopping threshold after iterative testing and adjustments.
- Object detection: computer vision tests showed ~80% accuracy for oranges and ~70% for bottles; bottle detection was improved with additional tuning.
- Object collection: initial pickup success was ~40% due to LIDAR placement and thresholding; after permanently fixing and adjusting the LIDAR, collection accuracy increased (improvements documented in test logs).

# Project metrics
- Build time: completed within 6 months.
- Cost: total component cost $614 (under the $700 target).

# Conclusion

The ATPR met its project requirements: it navigates autonomously on onboard power, avoids obstacles, detects bottles and oranges at acceptable accuracy levels, and collects and sorts found items. Safety measures including power cutoffs and teleoperation were implemented. The modular design supports future expansions and deployment scenarios.

## Diagrams & Schematics

Below are reference images for the system block diagram and selected schematic excerpts.

![ATPR block diagram](/assets/images/atpr-block-diagram1.png){:alt="ATPR block diagram" style="max-width:100%;height:auto;"}

![ATPR software architecture diagram](/assets/images/atpr-block-diagram2.png){:alt="ATPR software diagram" style="max-width:100%;height:auto;"}

![ATPR schematic excerpt](/assets/images/atpr-schematic.png){:alt="ATPR schematic" style="max-width:100%;height:auto;"}

## Senior design report

Read the full senior design write-up: [Full ATPR senior design report](/assets/ATPR-report.docx){:target="_blank" rel="noopener"}
