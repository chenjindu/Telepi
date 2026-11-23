# Changelog

All notable changes to Telepi OS are documented here.

## [2.0.0] — 2026-11-22

The product is now **Telepi**, and the system image is **Telepi OS**
(previously HelpRouter and HelpOS Router Edition). 2.0.0 focuses on being a portable router
and nothing else.

### Changed

- Renamed everything on the device: hotspot names are now `Telepi-5G-XXXX` / `Telepi-2.4G-XXXX`,
  the default password is `telepi.com`, and the web UI lives at `http://192.168.111.200:8080`
- Redesigned web UI: new look, network status cards (WAN / LAN / Wi-Fi 5G / Wi-Fi 2.4G) on the
  overview page, light and dark themes
- The connected upstream network is always listed first on the WAN page

### Removed

- SMB, FTP, Git, SVN, Web and File servers
- Remote access (`*.hc.ink` subdomain, DDNS, tunnel)
- VPN server and OpenVPN (the WireGuard **client** stays)
- Local Share, web terminal, backup & restore

### Upgrading

Devices running 1.x cannot be upgraded in place. Flash the 2.0.0 image — see the
[Flash Guide](docs/flash-guide.md).

## [1.0.0] — 2026-08-31

First public release (as HelpOS Router Edition; Raspberry Pi 5, arm64).

- WiFi repeater + dual-band (2.4G + 5G) hotspot, running simultaneously
- MAC clone
- DNS filtering
- VPN client (WireGuard)
- Full web UI, one-click upgrade
- Watchdogs for upstream WiFi reconnection and USB adapter recovery
- Also shipped file/code servers, remote access and a VPN server, all removed in 2.0.0
