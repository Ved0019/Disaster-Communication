


# 🚨 ResQ-Mesh: Off-Grid Disaster Communication Network

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Platform: ESP32](https://img.shields.io/badge/Hardware-ESP32-green)
![Protocol: LoRa & BLE](https://img.shields.io/badge/Protocol-LoRa%20%7C%20BLE-orange)

**ResQ-Mesh** is an off-grid, delay-tolerant mesh network designed for disaster scenarios (floods, earthquakes, cyclones) when traditional cellular infrastructure completely fails. 

By utilizing **Bluetooth Low Energy (BLE)** for phone-to-phone local mesh hopping and **LoRa (Long Range)** for long-distance telemetry, ResQ-Mesh bridges the gap between trapped victims and emergency command centers without requiring internet, cellular service, or complex user hardware.

> **Built for the Smart India Hackathon (SIH)** 🇮🇳

---

## 📖 The Core Concept

1. **The Scream (BLE):** A victim hits SOS on the ResQ-Mesh app. The phone silently broadcasts a highly compressed 13-byte SOS payload via BLE Advertising (no pairing required).
2. **The Bucket Brigade (Mesh):** Other phones in the vicinity act as relays, catching the BLE packet and automatically bouncing it forward to extend the range (Store-and-Forward Mesh).
3. **The Megaphone (Bridge Node):** Low-cost ESP32+LoRa nodes (mounted on poles, rooftops, or search-and-rescue drones) sweep the area, pick up the BLE SOS, and transmit it over long distances (up to 15km) via LoRa.
4. **The Dashboard (Gateway):** The command center receives the LoRa packet and plots the victim's exact coordinates, emergency type, and severity on an offline local map.

---

## 🏗️ System Architecture

Our network consists of a 3-Tier architecture:

*   **Tier 1 (Victims):** Mobile App (Android/iOS) generating BLE GATT Advertising packets.
*   **Tier 2 (Relays):** Custom ESP32 hardware bridging BLE to LoRa.
*   **Tier 3 (Rescuers):** Central LoRa Gateway connected to a local Python/Web offline dashboard.

---

## ⚡ Technical Highlights

### Ultra-Optimized 13-Byte Payload
Bandwidth is our biggest constraint. Sending plain text over LoRa is slow and error-prone. We compressed the entire SOS dataset into a **13-byte binary struct** using enums and bitmasking, enabling the highest LoRa Spreading Factor (SF12) for maximum range and penetration through rubble.

```cpp
struct __attribute__((packed)) SOSPacket {
    uint8_t messageID;    // 1 Byte: Unique ID to prevent broadcast storms
    uint8_t hopCount;     // 1 Byte: TTL (Time-To-Live) for mesh routing
    uint8_t type;         // 1 Byte: Enum (1=Flood, 2=Quake, 3=Fire, etc.)
    uint8_t level;        // 1 Byte: Enum (1=Critical, 5=Info)
    float latitude;       // 4 Bytes: GPS Latitude
    float longitude;      // 4 Bytes: GPS Longitude
    uint8_t flags;        // 1 Byte: Bitmask (Bit 0=Child, Bit 1=Senior, Bit 2=Medical)
};

```

### Zero-Configuration Usage

Victims do not need to "pair" Bluetooth devices. The app injects the 13-byte payload directly into the public BLE Manufacturer Specific Data field.

---

## 🛠️ Hardware Requirements (BOM)

To build one **Tier 2 Bridge Node**, you need:

| Component | Description | Est. Cost (INR) |
| --- | --- | --- |
| **ESP32 DevKit** | Microcontroller with built-in Wi-Fi & BLE | ₹300 |
| **SX1278 LoRa Module** | 433MHz / 868MHz Long Range Radio | ₹450 |
| **TP4056 Module** | Lithium Battery Charger | ₹50 |
| **18650 Li-ion Battery** | 2500mAh Power Source for off-grid life | ₹150 |
| **Total** | **Ultra-low cost infrastructure** | **~₹950** |

*(Note: We recommend the TTGO LoRa32 board, which combines the ESP32, LoRa, and an OLED screen on one PCB for easy prototyping).*

---

## 💻 Software Stack

* **Firmware:** C++ (Arduino IDE / PlatformIO), RadioLib (LoRa), NimBLE (Bluetooth)
* **Mobile App:** Flutter / React Native (BLE Advertising & Scanning)
* **Command Dashboard:** Python, Flask, Leaflet.js (Offline OpenStreetMap)

---

## 🚀 Getting Started

### 1. Flash the ESP32 Bridge Node

1. Clone this repository.
2. Open the `/firmware/bridge_node/` folder in PlatformIO or Arduino IDE.
3. Install the required libraries: `RadioLib` and `NimBLE-Arduino`.
4. Select your ESP32 board and click **Upload**.

### 2. Run the Offline Rescue Dashboard

1. Navigate to `/dashboard/`.
2. Install Python requirements: `pip install -r requirements.txt`
3. Connect your LoRa Receiver Gateway to your PC via USB.
4. Run the server: `python app.py`
5. Open `http://localhost:5000` to view the live offline map.

---

## 🗺️ Project Roadmap (Hackathon Milestones)

* [x] **Level 1:** Establish basic LoRa point-to-point transmission.
* [x] **Level 2:** Implement 13-byte struct encoding/decoding.
* [ ] **Level 3:** Bridge BLE scanner on ESP32 to LoRa transmission.
* [ ] **Level 4:** Build the mobile app for BLE advertising.
* [ ] **Level 5:** Finalize Python offline map dashboard.

---

## 🤝 Contributing

Built with ❤️ for the Smart India Hackathon. Pull requests and hardware iterations are welcome!

## 📜 License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

```

```
