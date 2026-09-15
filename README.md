# ConvoyLink

**An Experimental Embedded Platform for Internet-Independent Vehicle-to-Vehicle Communication**

---

## Abstract

Reliable communication between vehicles traveling in a convoy is typically assumed to depend on cellular infrastructure. This assumption fails in degraded or infrastructure-free environments — remote mountain routes, disaster-relief corridors, off-road expeditions, and rural highways — where connectivity is intermittent or entirely absent.

ConvoyLink is a research-oriented embedded systems project investigating the feasibility, performance, and engineering trade-offs of low-cost, infrastructure-independent vehicle-to-vehicle (V2V) communication using ESP32 microcontrollers paired with sub-GHz long-range (LoRa) wireless transceivers. Rather than treating this as a demonstration of existing networking appliances, the project systematically characterizes the physical- and link-layer behavior of resource-constrained embedded hardware under real-world mobile conditions, and documents this behavior through quantitative field testing.

The project is developed incrementally across four versions (V1–V4), progressing from a validated two-node text-messaging prototype toward automotive-grade embedded networking concepts (RTOS, CAN, SOME/IP). Full technical rationale is documented in [`Documentation/System_Architecture.md`](Documentation/System_Architecture.md) and [`Documentation/Design_Decision.md`](Documentation/Design_Decision.md).

---

## Problem Statement

Convoy travel through low-connectivity regions (e.g., Himalayan and Ladakh routes, rural highways, off-road expeditions) leaves vehicles unable to coordinate, share status, or raise emergency alerts once cellular coverage is lost. Existing solutions either depend on cellular/satellite infrastructure (costly, unavailable in remote terrain) or on consumer radio devices lacking structured data exchange, location awareness, or extensibility for research instrumentation.

This project investigates whether a low-cost, ESP32 + LoRa-based embedded system can provide a **reliable, measurable, and extensible** communication substrate for this use case — and, in doing so, generate empirical data on the practical limits of sub-GHz embedded wireless links in vehicular, obstructed, and mobile conditions.

---

## Research Objectives

1. Characterize the relationship between physical distance, obstruction, and Packet Delivery Ratio (PDR) for a two-node LoRa link under vehicular conditions (see `System_Architecture.md`, RQ1–RQ4).
2. Establish a reproducible, instrumented testing methodology (range, RSSI, SNR, latency, duty-cycle compliance) that generates citable engineering data rather than qualitative demonstration.
3. Progressively extend the system's architectural complexity (2-node → multi-node convoy → automotive-grade networking concepts) while maintaining empirical validation at each stage.
4. Document all design decisions, trade-offs, and regulatory constraints (e.g., WPC 433 MHz band limitations) as a transparent engineering record.

---

## System Overview

Each node consists of an ESP32 microcontroller, a Semtech SX1278 LoRa transceiver (433 MHz), a 0.96" I2C OLED display, and a local user-input interface, powered independently per vehicle. Nodes communicate over a point-to-point LoRa link with no dependency on internet or cellular infrastructure.

Full architectural detail — including the layered software model, hardware allocation, and data flow — is documented in [`Documentation/System_Architecture.md`](Documentation/System_Architecture.md).

---

## Current Status

**Active development — Version 1 (2-Node Prototype)**

- [x] System architecture and research questions defined
- [x] Hardware platform and frequency band selected (see DD-001, DD-002 in `Design_Decision.md`)
- [x] Block diagram and wiring specification completed
- [ ] LoRa hardware procurement (in progress)
- [ ] Firmware implementation (message framing, SPI driver integration)
- [ ] Field testing and empirical data collection

---

## Repository Structure

- **Documentation/**
  - `System_Architecture.md` — Layered architecture, research questions
  - `Design_Decision.md` — DD-00x decision records with technical justification
  - `Development_Roadmap.md` — V1–V4 phased roadmap
  - `V1_Block_Diagram_(2 Node).jpeg`
- **Hardware/** — Wiring diagrams, pin mappings, BOM
- **Firmware/** — ESP32 source code (per-node)

---

## Technology Stack

| Component | Selection | Rationale |
|---|---|---|
| Microcontroller | ESP32 (dual-core) | Sufficient processing headroom, native SPI/I2C, low cost |
| Wireless Transceiver | Semtech SX1278 (LoRa, 433 MHz) | Legal unlicensed band in India; strong obstacle penetration |
| Display | 0.96" OLED (SSD1306, I2C) | Local status/message interface without external hardware |
| Firmware | Embedded C++ (Arduino/PlatformIO) | Mature LoRa driver ecosystem (`RadioLib`, `LoRa.h`) |

Full justification for each selection is recorded in `Design_Decision.md`.

---

## Development Roadmap

| Version | Focus | Status |
|---|---|---|
| V1 | 2-node point-to-point text messaging, empirical link characterization | In Progress |
| V2 | 3-node convoy topology, GPS integration, SOS alerts | Planned |
| V3 | Structured protocol, ACK/retry, error detection, reliability testing | Planned |
| V4 | RTOS, CAN concepts, automotive networking architectures | Future Direction |

Full roadmap detail: [`Documentation/Development_Roadmap.md`](Documentation/Development_Roadmap.md)

---

## Motivation

This project is developed as a long-term, research-oriented undergraduate initiative in embedded systems and wireless networking, intended to support applications to international research programs (e.g., Mitacs Globalink Research Internship, MEXT Scholarship) through demonstrated engineering rigor, empirical validation, and transparent documentation of the design process.

---

## License

This project is licensed under the MIT License. See [`LICENSE`](LICENSE) for details.
