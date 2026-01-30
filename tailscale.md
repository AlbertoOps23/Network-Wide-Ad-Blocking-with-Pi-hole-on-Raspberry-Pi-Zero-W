# Secure Remote Access with Tailscale

This document describes how **Tailscale** is used to provide secure remote access to the Pi-hole instance running on a Raspberry Pi Zero W. The focus is on enabling encrypted connectivity and DNS resolution over a private mesh VPN without exposing services to the public internet.

---

## 1. Purpose and Design Rationale

Tailscale was selected to:

* Provide secure remote access to the Pi-hole admin interface
* Allow DNS filtering when devices are outside the local network
* Avoid port forwarding or public-facing services
* Use modern, encrypted, identity-based networking (WireGuard-based)

This approach aligns with security best practices for home and small-network infrastructure.

---

## 2. Tailscale Installation

Tailscale is installed directly on the Raspberry Pi hosting Pi-hole.

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

After installation, authenticate the device:

```bash
sudo tailscale up
```

This registers the Raspberry Pi with the Tailscale network and assigns it a private VPN IP address (typically in the `100.x.x.x` range).

---

## 3. DNS Integration with Pi-hole

To ensure DNS queries from remote devices are filtered by Pi-hole:

1. Identify the Pi-hole Tailscale IP address:

   ```bash
   tailscale ip -4
   ```
2. In the Tailscale admin console, set the Pi-hole Tailscale IP as the DNS server.
3. Enable **Override local DNS** so connected clients use Pi-hole for name resolution.

This configuration ensures consistent DNS filtering whether devices are on the LAN or connected remotely.

---

## 4. Pi-hole Listening Configuration

For DNS over VPN to function correctly, Pi-hole must listen on the appropriate interfaces.

* Navigate to **Settings → DNS** in the Pi-hole admin interface
* Set **Interface listening behavior** to:

  * *Listen on all interfaces* (or *Listen on all interfaces, permit all origins*)

This allows Pi-hole to respond to DNS queries arriving over the Tailscale interface.

---

## 5. Verifying Connectivity

* Connect a client device to the Tailscale VPN
* Confirm DNS resolution using Pi-hole:

  * Check real-time queries in the Pi-hole dashboard
* Verify internet access while connected remotely

Successful queries indicate correct VPN routing and DNS enforcement.

---

## 6. Security Considerations

* No inbound ports are exposed on the router
* Access is restricted to authenticated Tailscale devices
* Traffic is encrypted end-to-end
* Pi-hole remains isolated from the public internet

This setup provides a secure and maintainable solution for remote DNS filtering and administration.

---

## Summary

Tailscale enables secure, encrypted remote access to the Pi-hole DNS infrastructure without requiring public exposure. Combined with Pi-hole, it provides consistent ad and tracker blocking both on and off the local network while adhering to modern security principles.
