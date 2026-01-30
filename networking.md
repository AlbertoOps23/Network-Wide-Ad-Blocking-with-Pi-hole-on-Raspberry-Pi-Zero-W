# Networking Architecture – Pi-hole on Raspberry Pi Zero W

This document explains the **network design and DNS flow** for the Pi-hole deployment on a Raspberry Pi Zero W, including decisions regarding router integration, wired connectivity, and client behavior. It focuses on the architecture and design rationale rather than step-by-step installation.

---

## 1. Overview

The Pi-hole device functions as a **central DNS resolver and ad-blocker** for the home network. All LAN clients resolve domain names through the Pi-hole, enabling network-wide ad and tracker blocking while maintaining normal internet functionality.

Remote access and DNS resolution over a VPN is facilitated by Tailscale, allowing secure connections to the Pi-hole from external devices.

---

## 2. Network Topology

**High-level diagram (ASCII for simplicity):**

```
           +--------------------+
           |     Internet       |
           +---------+----------+
                     |
                Router/Gateway
                     |
    +----------------+----------------+
    |                                 |
  Wired                             Wi-Fi
    |                                 |
+--------+                     +-------------+
| Pi-Zero|                     | Client PCs, |
|  W     |                     | Phones,     |
| (Pi-   |                     | TVs, IoT    |
| hole)  |                     +-------------+
+--------+
```

* Router acts as the DHCP server and primary gateway.
* Pi-hole runs on the Pi Zero W with **Ethernet connectivity** for stability.
* Clients receive Pi-hole IP as their primary DNS via router DHCP.

---

## 3. DNS Resolution Flow

1. A client requests a domain (e.g., `example.com`).
2. The query is sent to the router.
3. The router forwards DNS requests to Pi-hole (enforced via DHCP settings).
4. Pi-hole checks the domain against blocklists.

   * If blocked: Pi-hole returns a null or safe response.
   * If allowed: Pi-hole forwards the query to an upstream DNS (e.g., Cloudflare, Google).
5. Pi-hole returns the resolved IP to the client.

This flow ensures **network-wide ad and tracker blocking** without requiring individual client configuration.

---

## 4. Router-Level DNS Enforcement

* The router’s DHCP configuration is modified to distribute the Pi-hole IP as the primary DNS.
* Secondary DNS entries are removed to prevent bypass.
* This ensures clients cannot use external DNS unless manually configured, enforcing centralized filtering.

### Notes:

* Some smart TVs and IoT devices may hardcode DNS or use encrypted DNS, which can bypass Pi-hole.
* Roku devices and certain streaming apps may not respect Pi-hole DNS, limiting ad-blocking effectiveness.

---

## 5. Wired Infrastructure Design

Ethernet connectivity was chosen for the Pi-hole for several reasons:

* Reduces DNS query latency and packet loss
* Provides stable uptime for network services
* Avoids Wi-Fi sleep or disconnect issues that can affect infrastructure devices
* Ensures predictable IP assignment for router DHCP and Tailscale VPN

---

## 6. Client Device Behavior

* **Phones and PCs**: Most queries follow the Pi-hole DNS; ads and trackers are blocked.
* **Smart TVs**: Some apps (YouTube, Roku channels) bypass Pi-hole due to hardcoded DNS or encrypted traffic.
* **IoT Devices**: Behavior varies; some respect DHCP-distributed DNS, others may not.

This illustrates the **limitations of DNS-level filtering** for certain devices and apps.

---

## 7. DNS-Based Blocking Limitations

* Cannot block ads served from the same domain as content without breaking functionality.
* Encrypted DNS (DoH) on clients bypasses Pi-hole unless forced through network-level controls or VPN.
* Some services require whitelisting for proper operation (Apple, Netflix, Roku, etc.).

This emphasizes the importance of understanding **real-world device behavior** in a network security context.

---

## 8. Notes on VPN DNS (Tailscale)

* Tailscale allows remote devices to use Pi-hole as their DNS server securely over a private mesh VPN.
* DNS queries from remote clients are tunneled through the Pi-hole, maintaining ad/tracker blocking outside the home network.
* Full configuration and setup of Tailscale is documented separately in `docs/tailscale.md`.

---

## Summary

This document provides a technical overview of the Pi-hole network architecture, DNS flow, client behavior, and wired infrastructure, supporting stable LAN operation and planned VPN integration.
