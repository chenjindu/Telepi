# Architecture

How Telepi sits in your network, and how Telepi OS is put together.

## Network Topology

Telepi sits between the untrusted upstream network and your own devices.
Upstream can be any WiFi (hotel, Airbnb, office, landlord, your ISP's router) or a wired
Ethernet connection — your side of the network never changes.

```
                      ┌──────────────────────────────────────────────┐
                      │                   Telepi                     │
   Upstream WiFi ────►│  WiFi radio #1 (upstream / repeater)         │
        or            │                                              │
   Wired Ethernet ───►│  Gigabit Ethernet (upstream, optional)       │
                      │                                              │
                      │  WiFi radio #2 ── 5 GHz hotspot ────┐        │
                      │  WiFi radio #3 ── 2.4 GHz hotspot ──┤        │
                      └─────────────────────────────────────┼────────┘
                                                            │
                              Your private network (192.168.111.0/24)
                                                            │
                    ┌───────────┬───────────┬───────────────┤
                 Laptop      Phone       Tablet      Other devices
```

Key properties:

- **Your network is always the same** — the same WiFi names, password and LAN subnet wherever
  the router goes. At a new place only Telepi has to join the upstream network.
- **NAT isolation** — the upstream network only ever sees one device (Telepi). With MAC clone,
  even networks with single-device restrictions work.
- **Your LAN keeps working offline** — devices behind Telepi still see each other when the
  upstream is down.

## Software Stack

Telepi OS is based on **Raspberry Pi OS Lite** (Debian, arm64) with everything
pre-installed and pre-wired:

```
┌─────────────────────────────────────────────────────┐
│  Web UI                                             │  ← http://192.168.111.200:8080
├─────────────────────────────────────────────────────┤
│  Network layer                                      │
│  WAN (WiFi repeater / Ethernet / MAC clone)         │
│  Wi-Fi (5G + 2.4G hotspots), LAN with DHCP          │
│  DNS server with filtering                          │
│  VPN client (WireGuard)                             │
├─────────────────────────────────────────────────────┤
│  Telepi OS base (Raspberry Pi OS Lite, arm64)       │
│  Watchdogs: WiFi reconnection, USB adapter recovery │
│  SSH server                                         │
└─────────────────────────────────────────────────────┘
```

Notes:

- **Full SSH access** to the underlying Linux system is included — it's your device.
- Built-in **watchdogs** recover automatically from upstream WiFi drops and USB WiFi adapter
  failures, so the router keeps itself alive unattended.
- Management is entirely web-based.

## Radio Layout

| Radio | Hardware | Role |
|---|---|---|
| #1 | USB 3.0 adapter (ALFA AWUS036AXML) | Upstream — connects to the source WiFi |
| #2 | M.2 card (MT7922) via PCIe HAT | 5 GHz hotspot |
| #3 | Raspberry Pi 5 onboard WiFi | 2.4 GHz hotspot |

With this layout the repeater and the dual-band hotspot run **simultaneously** —
no bandwidth halving, no mode switching. See the [Hardware List](hardware.md) for exact parts.

## The Ethernet Port

The Gigabit port detects what is plugged into it:

- a cable from an upstream router or modem → it becomes the **WAN** uplink (instead of WiFi)
- a computer or other device → it joins your **LAN**
