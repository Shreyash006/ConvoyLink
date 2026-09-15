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

## DD-002: Selection of 433 MHz Operating Frequency

* **Status:** Decided
* **Date:** September 2026
* **Context:** ConvoyLink V1 requires a sub-GHz physical layer to evaluate point-to-point vehicle links under varying terrain and obstacle conditions.
* **Decision:** Selected the 433 MHz band using Semtech SX1278 transceivers (e.g., Ra-02 modules).
* **Technical Justification:**
  1. **Obstacle Penetration:** Longer wavelength (~69.2 cm) provides lower path attenuation around terrain obstructions and foliage compared to 868 MHz and 2.4 GHz.
  2. **Hardware Availability:** Readily available breakout boards with robust driver support (`RadioLib` / `LoRa.h`).
* **Constraints & Trade-offs:**
  1. **Radiated Power Limits:** Operating within the 433.05–434.79 MHz window in India requires software-enforced output limits (≤ 10 mW / 10 dBm).
  2. **Antenna Scale:** Quarter-wave elements require ~17.3 cm clearance, which must be accounted for during field testing.

Actual measurements will be added to the repository as development progresses.
