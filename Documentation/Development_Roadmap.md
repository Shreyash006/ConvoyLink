# ConvoyLink — Development Roadmap

## Project Evolution

ConvoyLink is planned as a progressive embedded-systems project. Each version increases the system's communication capability, networking complexity, and relevance to advanced embedded and automotive systems.

---

# V1 — 2-Node Communication Prototype

### Objective

Develop and validate the fundamental ConvoyLink communication system using two embedded nodes.

### Planned Features

- 2 ESP32-based nodes
- Long-range wireless communication
- OLED displays
- User input for text messages
- Two-way text communication
- Unique node/vehicle identification
- Internet-independent operation
- Basic send/receive functionality

### Testing

The V1 prototype will be evaluated using parameters such as:

- Communication range
- Packet delivery rate
- Communication latency
- RSSI
- Reliability of message transmission

### Milestone

Successfully exchange text messages between two nodes without relying on cellular networks or the internet.

---

# V2 — 3-Node Convoy System

### Objective

Expand the V1 prototype into a small multi-vehicle convoy communication system.

### Planned Features

- 3 ESP32-based nodes
- Multi-node communication
- Vehicle/node identification
- GPS integration
- Location sharing
- Emergency/SOS messages
- Lead vehicle and convoy-member modes
- Vehicle distance/status information

### Testing

The system will be evaluated for:

- Multi-node communication reliability
- Range between convoy vehicles
- Packet delivery rate
- Latency
- GPS/location transmission
- Performance during vehicle movement

### Milestone

Demonstrate communication and basic coordination between three vehicles/nodes operating as a convoy without internet connectivity.

---

# V3 — Reliable Communication Protocol

### Objective

Investigate higher-bandwidth and more sophisticated communication capabilities beyond basic text and telemetry.

### Planned Features

- Voice communication investigation
- Higher-bandwidth wireless communication
- More sophisticated networking architecture
- Possible mesh/networking approaches
- Improved user interface
- Field testing
- Improved system reliability

### Research Focus

This stage will investigate the limitations of the communication technologies used in earlier versions and explore architectures capable of supporting higher-bandwidth communication.

### Milestone

Develop and evaluate an advanced communication prototype capable of supporting communication features beyond basic text and telemetry.

---

# V4 — Automotive / Advanced Embedded Systems

### Objective

Extend ConvoyLink toward concepts used in professional embedded and automotive systems.

### Planned Areas

- RTOS
- CAN and automotive communication concepts
- Automotive Ethernet
- SOME/IP
- Embedded Linux concepts
- QNX concepts
- More sophisticated networking architecture
- Real-time communication
- Distributed embedded-system architecture
- ECU-oriented system design

### Research Focus

V4 will explore how the concepts developed during earlier versions can be extended toward real-time, distributed, and automotive embedded systems.

The focus will shift from a simple prototype toward understanding the architecture and engineering principles used in advanced embedded systems.

### Milestone

Develop an advanced embedded-system architecture demonstrating selected automotive communication, real-time operating system, networking, and embedded software concepts.

---

## Production Engineering Trajectory (V1 → V4)

To prevent premature optimization while maintaining clear architectural progression, ConvoyLink follows an iterative, data-driven hardening model. Higher-tier engineering practices are introduced only after the immediate lower-tier assumptions are physically validated.

### Version Matrix & Scope Boundaries

| Capability Domain | V1 (Baseline Link Validation) | V2 (Telemetry & Linear Relay) | V3 (Systems & Driver Hardening) | V4 (Automotive / Hardware Hardening) |
| :--- | :--- | :--- | :--- | :--- |
| **Node Count & Topology** | 2 Nodes (Point-to-Point) | 3 Nodes (Linear Relay) | Multi-Node Cluster / Mesh | Scaled Convoy Fleet |
| **Payload Scope** | Fixed Text / Status Pings | GPS Telemetry + Heading Vectors | Compressed Telemetry + ACK/Retries | Prioritized Stream (Telemetry + Voice/Alerts) |
| **Firmware Runtime** | Arduino-ESP32 + FreeRTOS Tasks | FreeRTOS Queues + Task Notifications | Pure ESP-IDF (C++17) / Zephyr | Safety-Critical RTOS Constraints |
| **RF Transceiver Stack** | RadioLib (SX1278 SPI) + Basic Frame (Preamble/Sync/Length/CRC-16) | RadioLib + Structured Telemetry Serialization (Binary-Packed Payloads) | Custom Bare-Metal SPI Register Driver | Dual-Band / Diversity Link |
| **MAC Layer** | Basic Broadcast | Measured CSMA/CA or Slotted ALOHA | Deterministic TDMA + ACK/Retries | Priority Emergency Preemption |
| **Power & Hardware** | Breadboard / Dev-kits (USB 5V) | Perfboard / Modular Enclosure | Custom 2-Layer KiCad PCB | 4-Layer PCB + Automotive Load-Dump Protection |
| **Display & UI** | 0.96" SSD1306 (U8g2 / Page Buffer) | SSD1306 Vector Heading / Radar | Color TFT (ST7789) + Lightweight Canvas | Automotive Dash-Mount Display / LVGL |

---

### Technical Justification for Phasing

#### 1. Why FreeRTOS under Arduino Core for V1?

Rather than taking on the setup overhead of raw ESP-IDF, V1 leverages the FreeRTOS scheduler baked into the ESP32 Arduino framework to ensure clean task boundaries early:

- Core 0: Dedicated RF RX/TX and radio interrupt handlers (`Task_RF`).
- Core 1: User-input handling (buttons/encoders) and OLED display refresh (`Task_UI`).
- Inter-Task Communication: Handled via fixed-size FreeRTOS queues with zero dynamic memory allocation.

#### 2. Why RadioLib before Bare-Metal SPI Drivers?

V1 is an empirical validation of the basic RF link budget, line-of-sight range, and base packet transmission. Using RadioLib establishes a known-good baseline for packet error rate (PER) and RSSI/SNR profiles. In V3, writing a custom register-level SX1278 SPI driver will be validated against this baseline data.

#### 3. Why Basic Broadcast for V1 (Deferring CSMA & ACK/Retries)?

V1 isolates the physical layer and baseline firmware scheduler. Adding retransmission logic (ACK/retries) masks RF link drops under the hood rather than exposing raw packet delivery. Once 3-node relay contention arrives in V2, measured collision rates will drive the implementation of channel-sensing (CSMA/CA), followed by formal deterministic TDMA and ACK protocols in V3.

---

# Overall Development Path

```text
V1
2-Node Communication
        ↓
V2
3-Node Convoy System
        ↓
V3
Reliable Communication Protocol
        ↓
V4
Automotive / Advanced Embedded Systems
