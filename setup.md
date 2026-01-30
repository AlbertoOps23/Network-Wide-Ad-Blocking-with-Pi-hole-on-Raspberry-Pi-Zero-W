# Setup Guide – Pi-hole on Raspberry Pi Zero W

This document provides a step-by-step walkthrough of how Pi-hole was installed and integrated into the local network using a Raspberry Pi Zero W. The focus is on a reliable, reproducible setup using Ethernet connectivity and router-level DNS configuration.

---

## 1. Operating System Selection

The Raspberry Pi Zero W uses an ARMv6 CPU, which limits operating system compatibility. For this reason, the following OS was selected:

* **Raspberry Pi OS Lite (Legacy, 32-bit)**

This version provides:

* ARMv6 support
* Minimal footprint (no desktop environment)
* Long-term stability for headless infrastructure services

---

## 2. Flashing the SD Card

1. Download *Raspberry Pi OS Lite (Legacy, 32-bit)* from the official Raspberry Pi website.
2. Flash the image to a microSD card using **Raspberry Pi Imager** or a similar tool.
3. Insert the SD card into the Raspberry Pi Zero W.

---

## 3. Initial Boot and Power

* Power the Raspberry Pi using a stable 5V power source.
* Allow the device to boot fully (initial boot may take several minutes).
* Verify activity via the onboard LED indicators.

> Note: A repeating LED blink pattern during boot may indicate an incompatible OS image or SD card issue.

---

## 4. Network Connectivity (Ethernet)

Although the Raspberry Pi Zero W supports Wi-Fi, Ethernet was chosen for increased reliability.

1. Connect a **USB-to-Ethernet adapter** to the Pi using a USB OTG cable.
2. Connect the Ethernet adapter directly to the router.
3. Power-cycle the Pi to ensure the Ethernet interface initializes correctly.
4. Determine the Pi’s IP address from the router’s DHCP client list.

---

## 5. System Update

After accessing the Pi via SSH or local console, update the system:

```bash
sudo apt update && sudo apt upgrade -y
```

This ensures all packages and dependencies are current before installing Pi-hole.

---

## 6. Pi-hole Installation

Install Pi-hole using the official automated installer:

```bash
curl -sSL https://install.pi-hole.net | bash
```

During installation:

* Select the Ethernet interface
* Choose an upstream DNS provider
* Accept default blocklists
* Enable the web admin interface

Once installation completes, note the Pi-hole IP address and admin password.

---

## 7. Router DNS Configuration

To enforce network-wide ad blocking, the router was configured to use Pi-hole as the primary DNS server.

1. Log in to the router’s admin interface.
2. Locate the **LAN / DHCP / DNS** settings.
3. Set the **primary DNS server** to the Pi-hole’s IP address.
4. Save and apply the configuration.
5. Renew DHCP leases or reboot client devices as needed.

This ensures all connected devices resolve DNS queries through Pi-hole.

---

## 8. Verifying Operation

* Access the Pi-hole admin dashboard at:

  ```
  http://<pi-hole-ip>/admin
  ```
* Confirm queries are appearing in real time.
* Verify ads and trackers are being blocked on client devices.

---

## 9. Notes on Remote Access

Secure remote access to Pi-hole is achieved using **Tailscale**. The configuration and usage of Tailscale are documented separately.

See:

* `tailscale.md`

---

## Summary

At this stage, Pi-hole is fully operational as a network-wide DNS resolver using a Raspberry Pi Zero W with wired connectivity. All LAN traffic is filtered through Pi-hole via router-level DNS enforcement, providing centralized ad and tracker blocking.
