# Travel Router with Raspberry Pi 3B+

A portable, secure Wi-Fi router built using a custom OpenWRT firmware on a Raspberry Pi 3B+. Designed for travelers, researchers, and penetration testers needing secure, persistent connectivity over untrusted networks (hotel Wi-Fi, public hotspots, etc.).

---

## 🔧 Key Features

- **Custom-built OpenWRT firmware** (preconfigured)
- **No manual post-boot setup** — flash and go
- **Dual-interface Wi-Fi bridging** (one for uplink, one for broadcast)
- Planned: VPN client auto-connect, DNS filtering, captive portal bypass

---

## Hardware Requirements

- Raspberry Pi 3B+ (or any model with onboard Wi-Fi)
- USB Wi-Fi dongle (for second interface)
- 8GB+ microSD card
- Power source (portable battery or adapter)

---

## Why Custom Firmware?

OpenWRT allows us to:
- Eliminate post-boot configuration steps
- Persist all network rules across reboots
- Reconfigure easily with version-controlled builds
- Avoid command-line setup in the field

---

## Use Case: Public Wi-Fi Armor

Connect the Pi to any public/hotel Wi-Fi (via wlan1), and rebroadcast a secure private network (via wlan0):
- Safeguards against MITM attacks
- Prevents browser-based captive portal leaks
- Enables future VPN tunneling or DNS over HTTPS (DoH)

---

## Setup Instructions

[Check Setup Guide →](https://github.com/MrB1sw4s/Travel-Router-with-Pi/blob/main/processinbrief.md)

---

## Future Additions (Planned)

- WireGuard/OpenVPN auto-start with fallback
- Simple firewall layer for outgoing filtering
- DNS sinkhole + Pi-hole integration
- Optional Web UI for control

---

