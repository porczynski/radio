# MMN: unused Wi-Fi routers worldwide (estimate, 2026-11-27)

Order-of-magnitude estimate of how many working Wi-Fi routers sit unused in homes, and how many of them MMN could use without new firmware. Full model with assumptions, ranges, charts and sources: [spreadsheet (EN)](https://docs.google.com/spreadsheets/d/18IXAK-AL6SkNswFpWij04_VIuez-GZylh4jE8JyALT8/edit), [document (EN)](https://docs.google.com/document/d/1CwVSLFvLTLeaBx-r2Y7mXRBBJ5dMzFZTcmTYvWsgKVc/edit).

## Results (base case, with low-high range)

| Metric | Base | Range |
|---|---|---|
| Unused working Wi-Fi routers stored at home | ~520 M | 230-980 M |
| Supplied by an ISP | 56% | |
| Cannot be flashed with OpenWrt/DD-WRT | 87% (ISP gateways ~97%) | ~80-93% |
| Have a USB port | 44% | |
| Closed operator firmware | 54% | |
| WAN works only in the operator's network | 39% | |
| Usable as an offline access point without flashing | 93% | |
| Can link to another router by radio from the stock UI | 25% | |
| Internet providers worldwide | ~50,000 | 30,000-76,000 |
| Large operators supplying locked, branded gateways | ~500 | |

By technology: Wi-Fi 4 (802.11n) 37%, Wi-Fi 5 (802.11ac) 40%, Wi-Fi 6/6E 19%, Wi-Fi 1-3 (b/a/g) 4%, Wi-Fi 7 1%. Wi-Fi 1-3 are unofficial retroactive names. Largest manufacturers in the pool: TP-Link, Huawei, ZTE, Sagemcom, Technicolor/Vantiva.

## Method

Unused stock = shipments per production year (2003-2026) x share retired by age x share kept at home x share still working. Anchors: ITU 2025 (fixed broadband 20 per 100 people), MIIT (China 691 M), Digitimes Q1 2026 (38.3 M routers), Dell'Oro (>150 M PON ONTs 2024), Parks Associates (71% of US homes use an ISP router, 2025), OpenWrt 25.12 (>2,200 devices), UKE (2,133 fixed ISPs in Poland), BroadbandNow (2,877 US ISPs), WEEE Forum hoarding survey. Values between anchors are estimates.

## What it means for MMN

- A stock router works well as the local hotspot: the MMN mini computer plugs into a LAN port, logs in, sets SSID/channel/password and turns off DHCP.
- Radio links between nodes must come from the MMN mini computer (built-in or USB Wi-Fi with 802.11s/batman-adv) or Ethernet. Only ~25% of stock routers have client/repeater/WDS mode.
- About 19% have a scriptable interface; the rest need per-model web-UI drivers, so start with the 20-30 most common models per country.
- Accept only routers the user owns: leased ISP hubs may have to be returned.

## Biggest uncertainties

How many retired routers are kept rather than discarded (no survey; ~4x range), ISP gateway shipment totals (paywalled data), USB/WDS/API shares (no statistics), global ISP count (no register), and OpenWrt coverage by unit volume (the OpenWrt hardware table could not be downloaded here).
