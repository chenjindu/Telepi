# Changelog

All notable changes to Telepi OS are documented here.

## [2.1.0] — 2026-11-27

One image now fits every supported WiFi card. Telepi OS looks at the hardware on each boot
and sets up the hotspots to match, so there is no longer a separate image per card.

### Added

- **AW7915-NPD support.** This M.2 card (MediaTek MT7915) runs both hotspots, 5G and 2.4G,
  with WiFi 6. The Pi's onboard WiFi stays off in this setup
- **Hardware detection on every boot.** Swap the M.2 card, or move the microSD card to a
  Telepi built with a different card, and the hotspots come up with settings for that card.
  Hotspot names and passwords are kept
- The WAN, Wi-Fi 5G and Wi-Fi 2.4G pages show the adapter behind each of them: a picture,
  the model and its temperature. Known USB adapters are shown by brand
- The WAN page says so when no USB adapter is plugged in, and carries on by itself once
  you plug one in

### Changed

- **Changing the LAN subnet now restarts the router.** Reconnect to the same WiFi and your
  devices get addresses in the new subnet. The old step of confirming at the new address
  within two minutes is gone
- Faster boot: about 10 seconds with the AW7915-NPD (2.0.1 took about 15 seconds with the
  MT7922)
- Built on Raspberry Pi OS Lite 2026-10-06, kernel 6.18.50
- The image file name no longer ends in `_7922` and now carries the version:
  `2026-11-27-telepi-os-2.1.0-arm64.img.xz`
- The WAN page shows the last list of networks at once and rescans only when the list is old
- The kernel package is held back from `apt upgrade`, because the WiFi drivers in the
  image are built for this exact kernel
- Hotspot events (clients joining and leaving, the hotspot starting and stopping) are in
  the system log: `journalctl -u telepi-5g` and `journalctl -u telepi-24g`

### Fixed

- The first boot after flashing could stay on the splash screen for good and needed a
  power cycle (seen on a 128 GB card, while the system partition was being expanded).
  It now finishes by itself

### Supported WiFi hardware

| M.2 card (hotspots) | 5G hotspot | 2.4G hotspot | Status |
| --- | --- | --- | --- |
| MT7921 / MT7922 | M.2 card | Pi 5 onboard WiFi | Supported |
| AW7915-NPD | M.2 card, WiFi 6 | M.2 card, WiFi 6 | Supported, new in 2.1.0 |
| AW7916-NPD | — | — | Driver included, not tested yet |
| MT7925 (M.2) | — | — | Not supported: unstable as a hotspot in our tests |

The upstream radio is still a USB adapter with the MT7921AU chip. Drivers for the USB
version of the MT7925 and for MT7610U / MT7612U adapters are in the image, but these
adapters have not been tested.

### Upgrading

Flash the 2.1.0 image — see the
[Flash Guide](https://github.com/chenjindu/Telepi/blob/main/docs/flash-guide.md).

## [2.0.1] — 2026-11-25

### Improved

- More robust WiFi drivers: the image ships an updated mt76 driver set, for better
  long-running stability of the upstream connection and both hotspots

### Changed

- Built on a newer Raspberry Pi OS base image (kernel 6.12.75)
- System logs are kept across reboots
- Web UI 3.0.1: the About page shows the Telepi OS version

### Upgrading

Flash the 2.0.1 image — see the
[Flash Guide](https://github.com/chenjindu/Telepi/blob/main/docs/flash-guide.md).

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
