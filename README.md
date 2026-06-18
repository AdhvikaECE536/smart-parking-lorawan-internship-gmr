# smart-parking-lorawan-internship-gmr

>**Internship Learning Documentation**

>**Organisation**: GMR Airports

>**Project**: Smart Parking System

>**Role**: Understanding & Documentation (B.Tech, Post-2nd Year)

---

### 📂 Repository Structure

- `1. Architecture.md`     – overall view
- `2. Hardware.md`         – PCB and sensor explanation
- `3. LoRaWAN.md`          – about wireless protocols
- `4. ChirpStack.md`       – dashboard and server explanation 
- `5. Firmware`            – code explanation

---

### Note

This is **not** source code. It is my structured learning notes from an internship at GMR Airports, where i observed and understood a production-grade Smart Parking system. I have documented the hardware architecture, the LoRaWAN communication flow, and the ChirpStack dashboard logic as a learner, not as the original designer.

---

### Abstract

When a car gets pulled into a parking bay,a small battery-powered board under the road surface senses the vehicle's metal mass via a 3-axis magnetometer (BMM350) on the PCB board (STATIONS-V1.1), using STM32WLE5CCU6 - a single-chip SoC (Cortex-M4 + integrated LoRa transceiver). 
A tiny ceramic Chip Antenna (M620720) chirps the informatiion through a LoRaWAN Gateway to a ChirpStack server. The dashboard turns that chirp into a red or green icon: 1 = Occupied, 0 = Vacant.

---

## System Architecture (Text Diagram)

```
┌─────────────────┐      ┌──────────────┐      ┌─────────────────┐      ┌─────────────┐
│   Vehicle       │      │   Sensor     │      │   LoRaWAN       │      │  ChirpStack │
│   (Metal Mass)  │─────►│   Node       │─────►│   Gateway       │─────►│  Dashboard  │
│                 │      │  (STM32+     │      │  (Roof-mounted  │      │  (Red/Green │
│                 │      │  BMM350)     │      │   antenna)      │      │   Icons)    │
└─────────────────┘      └──────────────┘      └─────────────────┘      └─────────────┘
        │                        │                      │                        │
        │                        │                      │                        │
   Distorts Earth's         Reads magnetic         Forwards raw           Decodes 1/0
   magnetic field           field change           LoRa packets           payload bits
```

**Why this?**  
Running Wi-Fi or Ethernet to every parking slot at an airport is avoided as it is too expensive to trench cables under asphalt. LoRaWAN travels kilometers on tiny battery power, so one gateway on a lamp-post can cover hundreds of bays.

---
### Key Concepts

| Term | Simple Meaning | Why it matters here |
|------|----------------|---------------------|
| **LoRaWAN** | Long-range, low-power wireless protocol | Airport parking lots are huge; Wi-Fi would need hundreds of access points and power cables |
| **Confirmed Uplink** | Gateway ACKs the packet; sensor retransmits if lost | Critical when a car *just* parked—you do not want the system thinking the slot is still free |
| **Unconfirmed Uplink** | Fire-and-forget; no ACK | Used for periodic heartbeats or low-priority updates to save battery |
| **1-bit Payload** | A single `1` or `0` in the data field | Keeps radio airtime tiny (sub-second), which is legally required and battery-friendly |
| **ChirpStack** | Open-source LoRaWAN Network Server + dashboard | GMR can self-host it instead of paying per-device cloud fees |

---

### 👤 Author

[@AdhvikaECE536](https://github.com/AdhvikaECE536)

---

### 🙏 Acknowledgments
Special thanks to GMR Airports Ltd for proving me the environment to learn, explore and grow. 
