# Troubleshooting – Pi-hole on Raspberry Pi Zero W

This document covers common issues encountered during the setup and operation of Pi-hole on a Raspberry Pi Zero W, along with resolutions and best practices. It is intended to demonstrate **real-world troubleshooting experience** in a professional and portfolio-ready format.

---

## 1. Boot Issues and LED Codes

### Symptom: 7 Green Flashes on Boot

**Cause:** Kernel or OS incompatibility, often due to flashing the wrong OS version.

**Resolution:**

* Use **Raspberry Pi OS Lite (Legacy, 32-bit)** for ARMv6 devices like the Pi Zero W.
* Re-flash the SD card and ensure the image was fully written.
* Verify power supply is stable (5V, 2A recommended).

### Other Notes

* Unrecognized boot patterns usually indicate corrupted images or unsupported OS builds.
* Always use the official Raspberry Pi OS images for compatibility.

---

## 2. Pi-hole Web Interface Access

### Symptom: Cannot access Pi-hole admin page

**Cause:**

* IP misconfiguration
* Incorrect network interface selected during install
* Pi-hole service not running

**Resolution:**

* Confirm Pi-hole IP: `ip addr show` on Pi
* Verify FTL service is running: `sudo systemctl status pihole-FTL`
* Restart service if needed: `sudo systemctl restart pihole-FTL`

---

## 3. DNS Issues Over VPN (Tailscale)

### Symptom: No internet when connected to Tailscale

**Cause:** DNS is routed through Pi-hole, but network routing is incomplete.

**Resolution:**

* Configure Tailscale DNS to use Pi-hole’s Tailscale IP (starts with 100.x.x.x).
* Enable **Override local DNS** in Tailscale settings.
* Ensure Pi-hole is listening on all interfaces (`Settings > DNS > Interface listening behavior`).
* Restart services:

  ```bash
  sudo systemctl restart pihole-FTL
  sudo systemctl restart tailscaled
  ```

---

## 4. Router Integration Issues

### Symptom: Some devices not using Pi-hole

**Cause:** Secondary DNS or hardcoded DNS

**Resolution:**

* Remove secondary DNS entries from router DHCP
* Force devices to renew DHCP lease
* For devices with hardcoded DNS, consider network-level firewall rules or VPN routing through Pi-hole

---

## 5. Power and Stability Issues

* Use a stable 5V 2A power supply to prevent reboots and corruption.
* Ethernet over USB OTG improves stability for infrastructure services.
* Avoid using Wi-Fi for critical DNS server to reduce downtime.

---

## 6. Best Practices

* Regularly update Pi-hole and blocklists.
* Monitor Pi-hole logs to verify queries and blocked traffic.
* Document any changes for reproducibility and portfolio presentation.

---

This troubleshooting guide captures real-world issues and resolutions, demonstrating the ability to **identify, diagnose, and fix network and system-level problems professionally**.
