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
