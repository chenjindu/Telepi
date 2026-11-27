# Hardware List (BOM)

The complete parts list. All items are off-the-shelf and available worldwide
(Amazon links on [telepi.com/products](https://telepi.com/products)).
Equivalent parts with the same specs also work — these are the exact models we build and test with.

| # | Photo | Part | Notes |
|---|-------|------|-------|
| 1 | <img src="images/hw_01.png" width="120" alt="Raspberry Pi 5"> | **Raspberry Pi 5** | 2GB RAM or more |
| 2 | <img src="images/hw_02.png" width="120" alt="27W USB-C PSU"> | **27W USB-C power supply** | Official Raspberry Pi 27W PSU recommended — the Pi 5 is picky about power |
| 3 | <img src="images/hw_03.png" width="120" alt="GeeekPi case"> | **GeeekPi aluminum case for Pi 5** | With active cooler; supports PCIe peripheral boards (X1000/X1001/X1003/N04/N05) |
| 4 | <img src="images/hw_04.png" width="120" alt="Waveshare M.2 HAT"> | **Waveshare PCIe to M.2 E-Key HAT** | For Raspberry Pi 5; supports NGFF (M.2 E-Key) wireless NICs, USB Bluetooth; HAT+ standard |
| 5 | <img src="images/hw_05.png" width="120" alt="MT7922 WiFi card"> | **MT7922 Wi-Fi 6E network card** | M.2 E-Key; drives the 5G hotspot (the 2.4G hotspot uses the Pi 5 onboard WiFi) |
| 6 | <img src="images/hw_06.png" width="120" alt="Bingfu antennas"> | **Bingfu dual-band antennas (2-pack)** | 2.4/5/5.8 GHz 3dBi MIMO, RP-SMA male; connect to the MT7922 |
| 7 | <img src="images/hw_07.png" width="120" alt="MT7921AU USB WiFi adapter"> | **MT7921AU USB 3.0 WiFi adapter** | WiFi 6E (AXE3000, tri-band); the upstream/repeater radio. See [USB adapter models](#usb-adapter-models) below |
| 8 | <img src="images/hw_08.png" width="120" alt="microSD / USB SSD"> | **microSD card or USB SSD** | 16 GB or larger; SSD gives better longevity under 24/7 use |

## M.2 Card Options

Telepi OS 2.1.0 and later detects the M.2 card on every boot and sets up the hotspots to match.
The same image works with either card:

| M.2 card | 5G hotspot | 2.4G hotspot |
|---|---|---|
| **MT7921 / MT7922** (the card in the list above) | M.2 card | Pi 5 onboard WiFi |
| **AsiaRF AW7915-NPD** (MediaTek MT7915) | M.2 card, WiFi 6 | M.2 card, WiFi 6 |

M.2 cards with the MT7925 chip are not supported: the card was unstable as a hotspot in our tests.

## USB Adapter Models

The upstream radio is the one part where you have a choice of brands. What matters is the
chip: a **MediaTek MT7921AU** (Linux driver `mt7921u`, included in Telepi OS).

| Brand | Model | Chip |
|---|---|---|
| Panda Wireless | PAU0F AXE3000 Tri Band | MT7921AU |
| ALFA Network | AWUS036AXML | MT7921AU |
| EDUP | USB 3.0 WiFi 6E Adapter AX3000M | MT7921AU |
| NETGEAR | Nighthawk A8000 | MT7921AU |
| TP-Link | Archer TXE50UH AXE3000 | MT7921AU |
| BrosTrend | AXE3000 | MT7921AU |

Telepi is built and tested with the Panda. The others use the same chip and are expected to
work the same way. Check the exact model name before buying: similar names can hide a
different chip.

## Assembly Notes

1. Mount the **active cooler** on the Pi 5 first, then the **PCIe HAT** on top.
2. Insert the **MT7922** card into the HAT's M.2 slot and connect its two antenna
   pigtails; screw the **Bingfu antennas** onto the RP-SMA connectors through the case.
3. Plug the **USB WiFi adapter** into one of the **blue USB 3.0 ports** (USB 2.0 ports will
   throttle it badly).
4. Insert the flashed microSD / connect the SSD, close the case, connect power. Done.

## Minimal Build (reduced feature set)

No PCIe HAT / MT7922 / USB adapter at hand? Telepi OS also boots on a **bare Raspberry Pi 5**.
The onboard WiFi provides the 2.4G hotspot, and DNS filtering and VPN work as usual — but there
is no 5G hotspot, and without the USB adapter there is no radio left for the upstream, so plug
a wired Ethernet uplink into the Gigabit port. This is not the configuration we build and test
with; the full parts list above is what gives you *simultaneous* repeater + dual-band hotspot.

## Power Consumption

Typical draw is **5–15W** depending on load — comparable to a phone charger.
It runs comfortably from a USB-C power bank that supports PD, which is what makes it
a practical travel companion.
