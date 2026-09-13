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
