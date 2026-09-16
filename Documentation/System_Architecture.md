# ConvoyLink: System Architecture Specification

## 1. Abstract & Research Motivation
Reliable Vehicle-to-Vehicle (V2V) communication typically assumes terrestrial cellular backhaul or established Dedicated Short-Range Communications (DSRC) infrastructure. In degraded operational environments—such as remote mountainous transit, disaster relief corridors, and subterranean or rural corridors—cellular dependence introduces critical single points of failure. 

ConvoyLink is an experimental embedded research platform designed to investigate internet-independent, infrastructure-free communication between coordinated mobile nodes. Rather than evaluating pre-packaged networking appliances, this platform systematically characterizes the physical-layer and link-layer boundaries of low-cost, resource-constrained embedded microcontrollers paired with sub-GHz long-range wireless transceivers under mobile line-of-sight (LOS) and non-line-of-sight (NLOS) conditions.

---

## 2. Research Problem & Core Questions
The core research objective of this platform is to quantify how low-cost embedded hardware maintains link integrity between vehicular nodes without access to centralized time synchronization or network infrastructure.

### V1 Link Characterization Questions:
* **RQ1 (Distance vs. PDR):** How does the Packet Delivery Ratio ($PDR = \frac{P_{received}}{P_{transmitted}} \times 100\%$) attenuate over distance increments (10 m to 500 m) in a stationary two-node configuration?
* **RQ2 (Latency Profile):** What is the baseline round-trip time (RTT) for fixed-payload short text packets (≤ 32 bytes) at default spreading parameters?
* **RQ3 (Antenna Alignment):** What degradation in Received Signal Strength Indicator (RSSI) and Signal-to-Noise Ratio (SNR) occurs between co-polarized vertical omnidirectional dipoles versus cross-polarized configurations?
* **RQ4 (Environmental Occlusion):** What is the observed link margin attenuation introduced by static obstacles (e.g., foliage, building corners, vehicular chassis)?

---
## 3. Layered Architectural Model

To facilitate long-term extensibility without necessitating driver-level refactoring, ConvoyLink implements a decoupled, layered architectural model:

| Layer | V1 Scope | Future Scope |
|---|---|---|
| **Application Layer** | Diagnostic text console | V2: Telemetry / SOS alerts |
| **Network Layer** | Static peer addressing (Node_ID: 0x01 ↔ 0x02) | V2: Forwarding tables, linear relay engine (planned) |
| **Communication Layer** | Framing (preamble, sync word, length, CRC-16 checksum), serialization/deserialization protocols | — |
| **Physical Layer** | Microcontroller (ESP32) ↔ SPI bus ↔ Transceiver (SX1278) ↔ antenna interface; power regulation (3.3V LDO rails) | — |

Each layer exposes a fixed interface to the layer above it, so that hardware or protocol substitutions (e.g., a different transceiver in a later version) can occur without modifying application-level logic.

---

## 4. V1 Implementation Baseline (Two-Node Prototype)

### 4.1 Topology
V1 operates strictly as a two-node, single-hop point-to-point architecture:

[ Node 1 / Vehicle 1 ] <==== RF Wireless Link ====> [ Node 2 / Vehicle 2 ]

### 4.2 Hardware Allocation per Node
* **Compute Engine:** Espressif ESP32 dual-core Xtensa LX6 (Core 0 allocated to protocol/radio handling; Core 1 allocated to UI/peripherals).
* **Display Interface:** 0.96-inch SSD1306 OLED (I2C interface: SCL, SDA, 3.3V, GND).
* **Input Interface:** Tactile push-button triggering interrupt-driven status/test packet dispatch.
* **RF Transceiver:** Sub-GHz Long-Range Transceiver Module (LoRa sx1278).
* **Power Conditioning:** 5V/3.3V DC-DC step-down regulation for automotive bus compatibility.

### 4.3 V1 Data Flow Model

The end-to-end data path for a single message, from user trigger to remote display, proceeds as a linear pipeline:

1. **User Input Trigger** — push-button interrupt initiates message dispatch
2. **Message Assembly Engine** — payload constructed in firmware
3. **CRC-16 Framing & Serialization** — preamble, sync word, length, and checksum applied
4. **SPI Bus Transfer** — framed packet passed from ESP32 to RF transmitter module
5. **RF Transmitter Module** — packet modulated onto the sub-GHz carrier
6. **↓ Air Interface (Sub-GHz RF Packet) ↓**
7. **RF Receiver Module** — remote node demodulates incoming packet
8. **SPI Bus Transfer** — received packet passed to remote ESP32
9. **Packet Demux & CRC Verification** — frame validated and unpacked
10. **Display Buffer Update** — decoded message written to frame buffer
11. **SSD1306 OLED Frame** — message rendered on remote node's display
---

## 5. Software Modularity & Decoupling
To ensure that peripheral drivers do not introduce non-deterministic jitter into the radio pipeline, the firmware architecture enforces strict physical separation:

* `Firmware/Node/communication/`: Transceiver drivers, SPI hardware abstractions, packet serialization, and link state machines. Completely decoupled from display logic.
* `Firmware/Node/display/`: Framebuffer formatting and I2C write routines for the SSD1306 controller.
* `Firmware/Node/input/`: Hardware debounce routines and GPIO interrupt service routines (ISRs).
* `Firmware/Node/config/`: Hardware pin mappings, radio parameter configurations (carrier frequency, bandwidth, spreading factor, code rate), and node identifier constants.

---

## 6. Progressive System Roadmap

### Phase 1: V1 Two-Node Baseline (Current Stage)
* Point-to-point text messaging.
* Static node addressing.
* Quantitative measurement of PDR, RSSI, SNR, and baseline latency.
* Characterization of antenna polarization losses.

### Phase 2: V2 Three-Node Convoy (Planned)
* Introduction of Node 3 (Vehicle C).
* Linear vehicular topology: `[Node A] <---> [Node B] <---> [Node C]`.
* Evaluation of simple multi-node packet collision and store-and-forward latency across an intermediate relay.
* Integration of GPS NMEA sentence parsing for relative convoy position tracking.

### Phase 3: V3 High-Throughput & Hybrid Architectures (Future Research)
* Investigation of dual-band or multi-radio architectures:
  * Sub-GHz narrowband link reserved for mission-critical telemetry, collision warnings, and emergency SOS packets.
  * Secondary 2.4 GHz / high-throughput link evaluated for burst voice transmission.

### Phase 4: V4 Automotive & Distributed Systems (Long-Term Research Direction)
* RTOS-driven deterministic scheduling (FreeRTOS task priority assignment and queue management).
* Controller Area Network (CAN / CAN-FD) interface evaluation for direct vehicular telemetry ingestion.
* High-level automotive middleware concepts (SOME/IP service discovery models, embedded Linux/QNX evaluation).

---

## 7. Experimental Integrity & Limitations
* **Empirical Claims:** No performance metrics (e.g., maximum line-of-sight range, minimum latency, or sustained PDR) will be asserted in technical papers or documentation without accompanying empirical test logs.
* **Operating Environment:** Initial tests assume ground-level terrestrial conditions. Vehicle metal enclosures present significant Faraday cage attenuation; early experiments will isolate antenna placement using magnetic mountings or direct line-of-sight external setups before evaluating internal dashboard attenuation.
