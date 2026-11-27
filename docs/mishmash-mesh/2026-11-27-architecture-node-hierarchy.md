# MMN – Architecture and node hierarchy (summary)

2026-11-27 · Status: proposal for discussion. Full documents (EN first, PL second) are on Google Drive: "MMN – Architecture and node hierarchy" (EN/MishMash Mesh/2026-11-27) and "MMN – Architektura i hierarchia węzłów" (PL/MishMash Mesh/2026-11-27).

## Starting point

Most home routers cannot be flashed and have no SSH, API, bridge or client mode. The router market study (2026-11-27) estimates about 520 million unused working routers, about 87% not flashable (about 97% of ISP gateways), about 93% usable as an offline access point without flashing, and only about 25% with client, repeater or WDS modes in the stock interface. MMN must work with routers that can only be configured through their normal web interface.

## Proposed model

- **The stock router is only an access point** that users connect to (SSID `MMN-...`).
- **The MMN microcomputer next to it is the brain**: it configures the router locally over Ethernet through the web UI (per-model drivers, headless browser; start with the 20 to 30 most common models per country), and links to neighbouring nodes with its own WiFi radio (built-in or USB adapter) or cables.
- A stock router cannot form a mesh on its own: no mesh protocol in stock firmware, and WDS/repeater modes are vendor-specific. The mesh is formed by the microcomputers (batman-adv over 802.11s locally, Yggdrasil or Reticulum as the global overlay).
- The technician station is the first neighbour, confirms new nodes by signing their certificates offline, and helps in the field. It does not need to configure routers over the air.
- Islands (home, building, estate, town, region) work fully offline. Bridges between islands start as WireGuard tunnels over the internet that carry only MMN traffic (no internet exit for users), later independent volunteer links (5 GHz point-to-point, LoRa, amateur radio).

## Key security and legal points

- Router credentials stay on the local microcomputer and never go to mishmashmesh.net.
- Old routers are treated as untrusted: end-to-end encryption above them, WPS and remote admin off, no WAN.
- Phase 1 uses only spare routers without internet, so ISP terms against sharing a connection do not apply. Users must state they own the device (ISP routers are often leased).
- Providing internet access to the public as a service may fall under the EECC / Polish Electronic Communications Law (UKE registration). Internet exits are the main legal risk. To be checked with a lawyer.

## Recommended naming (no master/slave)

| Role | Code / docs (EN / PL) | Optional public layer |
|---|---|---|
| MMN technical server | Core / Rdzeń | Lighthouse / Latarnia |
| Technician station | Anchor / Kotwica | Harbour / Port |
| Node (microcomputer + router) | Node / Węzeł | Buoy / Boja |
| Stock router (AP only) | Spot / Punkt | Beacon / Pława |
| Microcomputer with MMN OS | Box | Pilot |
| Gateway to other technologies | Gate / Bramka | Lock / Śluza |
| Link between islands | Bridge / Most | Ferry / Prom |
| Local network | Island / Wyspa | Island / Wyspa |
| Group of islands | Region | Archipelago / Archipelag |

## Phases

0. One pilot island, Core in the cloud.
1. Islands bridged over the internet (MMN traffic only).
2. Independent volunteer bridges.
3. Federated Core with an offline root key.
