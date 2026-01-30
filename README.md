# Network-Wide Ad Blocking with Pi-hole on Raspberry Pi Zero W

<img width="690" height="184" alt="pihole-logo" src="https://github.com/user-attachments/assets/940c1dde-e291-4899-869c-cd5093bd1076" />


## Overview
This project documents the deployment of a low-power, network-wide DNS
ad-blocking solution using Pi-hole on a Raspberry Pi Zero W.

The system is designed to function as an always-on infrastructure service,
integrated directly with the home router using Ethernet for improved stability
and reliability.

In addition to local network DNS filtering, this project includes secure
remote access using Tailscale, allowing Pi-hole management and DNS usage
from external devices via a private mesh VPN.

---

## Objectives
- Deploy Pi-hole on a Raspberry Pi Zero W
- Provide network-wide DNS-based ad and tracker blocking
- Integrate Pi-hole directly with the router via LAN DNS settings
- Use Ethernet instead of Wi-Fi for infrastructure reliability
- Enable secure remote access using Tailscale (VPN)

---

## Hardware & Software
**Hardware**
- Raspberry Pi Zero W
- USB-to-Ethernet adapter
- MicroSD card
- Home router with configurable DNS settings

**Software**
- Raspberry Pi OS Lite (Legacy, 32-bit)
- Pi-hole
- Tailscale (for secure remote access)

---

## Network Architecture
- Raspberry Pi functions as the primary DNS server
- Router distributes Pi-hole IP via DHCP
- All LAN clients resolve DNS through Pi-hole
- Optional remote clients connect via Tailscale VPN

![network diagram pi hole](https://github.com/user-attachments/assets/6457b539-99d1-432e-854a-c5ed37cca481)


---

## Implementation Summary
- Flashed Raspberry Pi OS Lite (Legacy, 32-bit) for ARMv6 compatibility
- Installed and configured Pi-hole as the network DNS resolver
- Transitioned from Wi-Fi to Ethernet for improved uptime
- Configured router DHCP/DNS settings to enforce Pi-hole usage
- Integrated Tailscale to enable secure remote access to Pi-hole

---

## Documentation
Detailed step-by-step documentation is provided in the `/docs` directory:

- `setup.md` – Operating system installation, Pi-hole setup, and router DNS configuration
- `networking.md` – DNS behavior, router integration, and client resolution
- `troubleshooting.md` – Common issues, limitations, and fixes
- `tailscale.md` – Tailscale installation and remote DNS usage

---

## Results
- Network-wide reduction of ads and trackers
- Improved visibility into DNS traffic
- Stable DNS resolution using wired connectivity
- Secure remote access via private VPN (Tailscale)

<img width="1342" height="742" alt="pi hole" src="https://github.com/user-attachments/assets/469387d9-eca1-4a4b-998c-4390f171b941" />


---

## Known Limitations
- DNS-level blocking cannot fully block ads served from the same domains
  as content (e.g., YouTube, Roku, some streaming services)
- Smart TVs and streaming devices may bypass or hardcode DNS behavior

---

## Lessons Learned
- Importance of OS compatibility on ARMv6 devices
- Benefits of Ethernet over Wi-Fi for infrastructure services
- Practical limitations of DNS-based filtering
- Value of private VPN access for remote network administration
