# ConvoyLink — Design Decisions

## 1. Internet-Independent Communication

ConvoyLink is designed to operate without cellular internet connectivity.

The primary objective is to enable communication between vehicles in areas where cellular networks may be unavailable or unreliable.

## 2. Embedded Hardware Platform

ESP32 is selected as the primary microcontroller platform because it provides sufficient processing capability, wireless interfaces, GPIO, and a low-cost development platform suitable for prototyping.

## 3. Long-Range Wireless Communication

A long-range wireless technology will be used for the primary communication layer.

LoRa is being considered for Version 1 because the initial system focuses on low-bandwidth communication such as text messages, GPS information, status data, and emergency alerts.

The exact LoRa module and operating frequency will be selected after evaluating suitable hardware and regulatory requirements.

## 4. Three-Node Prototype

The initial prototype will use three ESP32-based nodes:

- Node 1 — Lead Vehicle
- Node 2 — Convoy Member
- Node 3 — Convoy Member

Using three nodes allows the prototype to represent a small convoy rather than only a point-to-point communication link.

## 5. OLED Display

A 0.96-inch OLED display will be used to provide a simple local interface for displaying messages, vehicle IDs, system status, and other information.

## 6. Development Strategy

ConvoyLink will be developed incrementally.

### Version 1
Basic wireless text communication.

### Version 2
GPS, convoy tracking, emergency alerts, and additional communication features.

### Version 3
Investigation and development of voice communication and higher-bandwidth networking.

## 7. Research and Testing

The system will be evaluated using measurable parameters such as:

- Communication range
- Packet delivery rate
- Latency
- RSSI
- Number of communicating nodes
- Power consumption
- Performance under obstacles and movement
# ConvoyLink — Design Decision Records

This document logs the key engineering decisions made during ConvoyLink's development, including context, technical justification, and known trade-offs for each decision. Superseded decisions are retained (marked accordingly) rather than deleted, to preserve a transparent record of the design's evolution.

---

## DD-001: Internet-Independent Communication Architecture

- **Status:** Decided
- **Context:** Convoy travel through remote, mountainous, or rural regions frequently experiences degraded or absent cellular coverage, leaving vehicles unable to coordinate.
- **Decision:** ConvoyLink will operate as a fully infrastructure-independent communication system, with no dependency on cellular networks, internet connectivity, or centralized servers.
- **Justification:** This constraint directly addresses the project's core research problem and ensures the system remains functional in the target deployment environments.

---

## DD-002: Selection of 433 MHz Operating Frequency

- **Status:** Decided
- **Date:** September 2026
- **Context:** ConvoyLink V1 requires a sub-GHz physical layer to evaluate point-to-point vehicle links under varying terrain and obstacle conditions.
- **Decision:** Selected the 433 MHz band using Semtech SX1278 transceivers (e.g., Ra-02 modules).
- **Technical Justification:**
  1. **Obstacle Penetration:** Longer wavelength (~69.2 cm) provides lower path attenuation around terrain obstructions and foliage compared to 868 MHz and 2.4 GHz.
  2. **Hardware Availability:** Readily available breakout boards with robust driver support (`RadioLib` / `LoRa.h`).
- **Constraints & Trade-offs:**
  1. **Radiated Power Limits:** Operating within the 433.05–434.79 MHz window in India requires software-enforced output limits (≤ 10 mW / 10 dBm e.r.p.), per WPC delicensing rules.
  2. **Antenna Scale:** Quarter-wave elements require ~17.3 cm clearance, which must be accounted for during field testing.
  3. **Bandwidth & Duty Cycle Compliance:** The delicensed 433–434.79 MHz allocation additionally limits channel bandwidth to 10 kHz with a 10% duty cycle — narrower than LoRa's typical 125 kHz default configuration. V1 will document the actual bandwidth/duty-cycle settings used in firmware and note this gap as a compliance consideration for any future field or public deployment.

Actual measurements will be added to the repository as development progresses.

---

## DD-003: Embedded Hardware Platform Selection

- **Status:** Decided
- **Decision:** ESP32 (dual-core Xtensa LX6) selected as the primary microcontroller platform for all nodes.
- **Justification:** Provides sufficient processing headroom for concurrent radio and UI handling, native SPI/I2C peripheral support, and a low-cost, well-documented development ecosystem suitable for iterative prototyping.

---

## DD-004: V1 Prototype Topology — Two-Node Configuration

- **Status:** Decided (supersedes earlier three-node consideration)
- **Context:** Initial project scoping considered a three-node prototype to represent a small convoy directly.
- **Decision:** V1 is scoped to a two-node, single-hop point-to-point configuration. Multi-node topology (3+ nodes) is deferred to V2.
- **Justification:** Isolating the two-node case first allows link-level characteristics (PDR, RSSI, latency, obstruction effects) to be established as a controlled baseline before introducing the additional variables of multi-node collision handling and store-and-forward behavior. This staged approach produces cleaner, more attributable experimental data at each phase.

---

## DD-005: Local Display Interface

- **Status:** Decided
- **Decision:** A 0.96-inch SSD1306 OLED display (I2C interface) is used at each node for local message and status display.
- **Justification:** Provides a minimal, low-power local interface without requiring an external host device, supporting fully standalone node operation.

---

## DD-006: Research and Evaluation Methodology

- **Status:** Decided
- **Decision:** System performance will be evaluated using the following measurable parameters:
  - Communication range
  - Packet Delivery Ratio (PDR)
  - Latency (round-trip time)
  - Received Signal Strength Indicator (RSSI) and Signal-to-Noise Ratio (SNR)
  - Power consumption
  - Performance under obstruction and vehicular motion
- **Justification:** Defining measurable evaluation criteria in advance ensures that project claims are supported by empirical data rather than qualitative demonstration, consistent with the experimental integrity standards outlined in `System_Architecture.md`.

Detailed version-by-version testing scope is documented in [`Development_Roadmap.md`](Development_Roadmap.md).

Actual measurements will be added to the repository as development progresses.
