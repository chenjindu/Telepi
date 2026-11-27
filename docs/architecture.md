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

Telepi comes in two builds. The upstream radio is the same; the hotspots differ.

**Build 1 — Starter** (Telepi OS 2.0.1, or 2.1.0 and later)

| Radio | Hardware | Role |
|---|---|---|
| #1 | USB 3.0 adapter (MediaTek MT7921AU) | Upstream — connects to the source WiFi |
| #2 | MT7922 M.2 card on the PCIe to M.2 HAT | 5 GHz hotspot |
| #3 | Raspberry Pi 5 onboard WiFi | 2.4 GHz hotspot |

**Build 2 — Pro** (Telepi OS 2.1.0 and later)

| Radio | Hardware | Role |
|---|---|---|
| #1 | USB 3.0 adapter (MediaTek MT7921AU) | Upstream — connects to the source WiFi |
| #2 | AW7915-NPD / AW7916-NPD Mini PCIe card on the PCIe to MiniPCIe HAT | 5 GHz hotspot **and** 2.4 GHz hotspot |
| — | Raspberry Pi 5 onboard WiFi | Switched off |

In both builds the repeater and the dual-band hotspot run **simultaneously** —
no bandwidth halving, no mode switching. In Build 1 the hotspots come from ordinary WiFi
cards working as access points, and the 2.4 GHz hotspot from the onboard WiFi is weak.
In Build 2 a chip made for access points runs both hotspots.

Telepi OS 2.1.0 detects which build it is running on at every boot and sets up the hotspots
to match. See the [Hardware List](hardware.md) for the exact parts.

## The Ethernet Port

The Gigabit port detects what is plugged into it:

- a cable from an upstream router or modem → it becomes the **WAN** uplink (instead of WiFi)
- a computer or other device → it joins your **LAN**
