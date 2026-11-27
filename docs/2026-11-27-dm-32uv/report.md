# Baofeng DM-32UV: firmware change, accessories, repeaters, alternatives

Date: 2026-11-27 · Project: Radio · Status: version 1 · Polish version: PL / 2026-11-27 DM-32UV

> Flashing is at your own risk. Rule number one: **make a full backup of both flash chips first.** The data flash holds this unit's own RX/TX calibration and ALPU key, which cannot be recovered from another radio.

---

## 1. Short answer

- **Yes, OpenGD77 can be installed on the DM-32UV.** The port was called OpenGD77_DM32 and since August 2026 **OpenDM32** (author RA4FHE / Koshak, based on andynvkz's Zastone UV008 port). Roger Clark VK3KYY (OpenGD77 lead) added "DM32/UV008" support to the OpenGD77 CPS.
- **grid.radio/opengd77/** is a browser-based OpenGD77 CPS (Web Serial). Its page lists **DM-32 / UV008** as supported with no driver needed and has a firmware section that can flash OpenGD77 or Baofeng firmware. Works in **Chrome, Edge or Brave** only (not Firefox or Safari), so flashing works from a Mac too.
- **A second, newer project exists: OpenDMR** (va3bpf), written from scratch on FreeRTOS with full source. Adds DMR, FM, **P25, NXDN, M17, YSF**, AM/SSB receive, APRS, GPS, an MMDVM hotspot mode. Licence: non-commercial. Near-daily releases (v0.8.2 on 2026-11-22).
- Both alternatives are still in development. The OpenDM32 author calls it demo-grade with bugs.

**Recommendation:** make the full backup, stay on stock 049 first while you learn the radio, then test OpenDMR or OpenDM32 via grid.radio. Always keep the original firmware and the flash backup.

---

## 2. Identify your unit first (brick risk)

There are at least two firmware lines; flashing the wrong one soft-bricks the radio. All units report model `DP570UV`, so the model string tells you nothing.

| Check | "ROW" line (most common) | "HR Vocoder" line (Asia/Taiwan) |
|---|---|---|
| Side keys SK1/SK2 | **raised horizontal ridges** | smooth |
| Firmware version (menu) | `DM32.01.*` or `DM32.NRF.*` | `DM32.00.*` or `UV32.*` |
| Stock firmware | 032…049 | 034, 037, 046 only |
| OpenDM32 / OpenDMR | tested on ROW | not confirmed, **do not flash** |

Recovery from a wrong flash: remove the battery, hold **SK1 + PTT**, power on and flash the correct file.

Sources: [M7OCM/DM-32UV](https://github.com/M7OCM/DM-32UV), [Chicago-Offline/dm32-info FIRMWARE.md](https://github.com/Chicago-Offline/dm32-info/blob/main/FIRMWARE.md)

---

## 3. What you need

| Item | Notes |
|---|---|
| K1 (Kenwood 2-pin) USB programming cable | The bundled CH340 cable works with the Baofeng CPS. **FTDI (e.g. BTECH PC03) is more reliable.** Push the plug fully in; the socket is tight. If you get errors, try a cable with another chip (CH340 / FTDI / CP2102). |
| Computer | Baofeng CPS and OpenUV008 are **Windows-only**. On a Mac: grid.radio in Chrome or the Python tools (OpenDMR / VK3KYY) with `pyserial`. A Windows VM also works. |
| Python 3 + `pip install pyserial pillow` | For backup and flashing scripts. |
| CH340 / FTDI driver | Usually built into Windows / recent macOS. |
| Charged battery | The whole job (mostly backup) takes about 40 minutes. |
| Target firmware | OpenDM32 (infotex58.ru topic 1168) or OpenDMR (GitHub Releases). See section 6. |
| Amateur licence | Needed to transmit on amateur bands (section 9). |

---

## 4. Downloaded files

All programs and firmware are in **Files (firmware & software) / 2026-11-27 DM-32UV** on Drive (shared by both language versions). Origins, SHA256 checksums and the list of missing files: the "README – sources, checksums, missing files" doc in that folder.

| Folder | Contents |
|---|---|
| 01_official_Baofeng | CPS v1.60, firmware pack 2025-08-22, firmware 046, manual, Picture Tool, FCC certificate |
| 02_stock_firmware_ROW_DM32.01 | stock firmware for the common hardware: 032…049, L01.048, NRF/Fanti 049 |
| 03_stock_firmware_HR_Vocoder_DM32.00 | stock firmware for HR Vocoder hardware: 034, 037, 046 |
| 04_backup_and_flash_tools | OpenUV008, VK3KYY scripts, OpenDMR `dm32_dump.py`, gzalo docs |
| 05_CPS_all_versions | Baofeng CPS 1.22–1.50, fonts, default codeplug |
| 06_mods_use_at_own_risk | RA4FHE band mods, "Light" 049, CPS without calibration password (research only) |
| 07_software_logo_record | Record tool, original logo |
| 08_OpenDMR_source_snapshot | full OpenDMR source |

**Not on Drive (files over ~3 MB exceed my Drive connection's limit):** CPS v1.60, firmware pack 2025-08-22, the PDF manual, Picture Tool, CPS installers 1.22–1.50, hacked CPS and the OpenDMR source. They are in the Claude project Library (`radio/files/2026-11-27_DM-32UV/`) and in the [M7OCM/DM-32UV](https://github.com/M7OCM/DM-32UV) archive.

**Could not be downloaded (host blocked in my environment):** OpenGD77/OpenDM32 firmware (infotex58.ru topic 1168), OpenGD77 CPS (opengd77.com), the OpenDMR release package (GitHub Releases). Expected SHA256 values are in the README. Latest OpenDM32: `OpenDM32.bin` (HotFix 2026-09-27) `742653d5…5f05`.

---

## 5. Step by step: backup (mandatory)

| Chip | Contents | Time |
|---|---|---|
| Program flash 25Q08 / 25Q16 (1–2 MB) | bootloader, firmware | ~1 min |
| Data flash 25Q128 (16 MB) | codeplug, fonts, voice, **calibration**, ALPU key | ~13 min |

1. Baofeng CPS: **Read** from radio → save the codeplug. Also export channels and contacts to CSV.
2. Flash backup (pick one method):
   - **OpenDMR `dm32_dump.py`** (Windows/Mac/Linux): radio off, cable in, run the command, switch the radio on within 30 s without pressing any key.
     ```
     py -u tools\backup\dm32_dump.py COM6 program backup\program-1.bin
     py -u tools\backup\dm32_dump.py COM6 program backup\program-2.bin
     py -u tools\backup\dm32_dump.py COM6 data    backup\data-1.bin
     py -u tools\backup\dm32_dump.py COM6 data    backup\data-2.bin
     fc.exe /b backup\program-1.bin backup\program-2.bin
     fc.exe /b backup\data-1.bin backup\data-2.bin
     ```
     On a Mac the port is e.g. `/dev/cu.usbserial-XXXX`; compare with `cmp file1 file2`.
   - **OpenUV008 Flasher** (Windows): CommPort tab → port, radio off, **Read**, switch on when prompted. Repeat for 25Q16 and 25Q128.
   - **VK3KYY scripts**: `py C7000_read_progmem.py COM6 program.bin`, `py dm32_read_Q128.py …`.
3. Read each chip **twice** and compare. Only two identical reads make a reliable backup.
4. Label the backup folder with the **radio's serial number** and keep two copies (Drive + USB stick).

---

## 6. Step by step: flashing

### Option A: OpenGD77 / OpenDM32 via grid.radio (simplest, works on Mac)

1. Open https://grid.radio/opengd77/ in **Chrome/Edge/Brave**.
2. Choose radio type **DM32 / UV008**.
3. Update mode: radio off, hold **PTT + SK1**, power on. **Green LED = update mode.** Connect the cable.
4. In the Firmware section select `OpenDM32.bin` (verify SHA256) and wait until done.
5. Write the **default codeplug**, then your own channels.

### Option B: OpenGD77 CPS on Windows

OpenGD77 CPS (build with DM32/UV008, e.g. `E2026.05.26.01`; the newer `E2026.07.13.01` is on infotex58.ru only) → **Radio Type: DM32/UV008** → **Extras → Firmware loader** → PTT+SK1 at power-on → select `.bin` → write default codeplug. This matches the "Install OpenGD77 on Baofeng DM-32_EN.pdf" already in your Drive.

### Option C: OpenDMR

1. Download `OpenDMR-<version>.zip` from Releases and verify SHA256.
2. Radio off, run
   `py -u tools\backup\dm32_dump.py COM6 program opendmr.bin --write --start 0xC000`
   and switch on within 30 s. The bootloader (below 0xC000) is never touched.
3. Codeplug: `opendmr-cps.html` (Chrome/Edge) or OpenGD77 CPS.
4. Later updates: `tools/debug/fw_update.py`.

### Going back to stock

- Update mode (**PTT + SK1**) and flash the stock `.bin` from 02_stock_firmware_ROW_DM32.01 (ROW: `DM32.01.01.049.bin`) via Baofeng CPS or grid.radio, **or**
- write your own program backup: `dm32_dump.py COM6 program backup\program-1.bin --write`.
- Then Baofeng CPS → Write your saved codeplug. Reset: write a CPS file with "allow reset", power on holding **SK1 + SK2**.

### Stock versions: what to watch

| Version | Recording | CSV contacts | Notes |
|---|---|---|---|
| 047 | – | 50k | on radios sold late 2025 |
| L01.048 | **none** | **150k** | "test" firmware |
| 049 | yes | 50k | latest in the archive; AES constant-IV bug verified fixed on NRF 049 |

---

## 7. Third-party accessories and antennas

The DM-32UV uses the **K1 (Kenwood 2-pin)** accessory jack, so a huge range of mics, earpieces and cables from Kenwood/Baofeng/Retevis/Abbree/Midland fits. Check the antenna connector before buying (Baofengs usually have an SMA-female socket on the radio and an SMA-male antenna; otherwise you need an adapter).

| Category | Popular picks (forum-recommended) | Notes |
|---|---|---|
| Handheld antenna | **Nagoya NA-771 / NA-701**, **Abbree AR-771** (tactical, ~48 cm), **Signal Stick**, **Diamond SRH77CA**, Diamond SRH805S (stubby) | NA-771 is a classic, but fakes are common; buy from trusted sellers |
| Mobile antenna | Nagoya UT-72 / Diamond NR-770, mag mount + SMA-to-PL/BNC adapter | better a coax lead than an antenna on the radio inside a car |
| Base antenna | Diamond X30 / X50, Comet GP-1 / GP-3, J-Pole | biggest single improvement for repeaters and hotspots |
| Programming cable | **BTECH PC03 (FTDI)**, Retevis/Baofeng K-plug CH340 | FTDI recommended for flashing |
| Battery | 3800 mAh USB-C for the DM-32 series | check compatibility; wrong batteries have been shipped |
| Mic / earpiece | Baofeng BS-UV25, Retevis/Abbree K1 mics, acoustic-tube kits | K1 standard |
| DMR hotspot | **Pi-Star / WPSD** on Raspberry Pi Zero 2 W + **MMDVM_HS_Hat / Jumbospot (duplex)**; ready-made **OpenSPOT 4**, **SharkRF**, Zumspot | use Brandmeister without a repeater in range |
| SDR for learning | RTL-SDR v4 + SDR++ / DSD-FME | monitor and decode DMR |
| Other | rugged case, 12 V car charger, dummy load for tests | |

Sources: [miklor.com DM32 review](https://www.miklor.com/COM/Review_DM32.php), [Baofeng accessories](https://www.baofengradio.com/collections/accessories), [Baofeng: Programming the DM-32UV part 1](https://www.baofengradio.com/blogs/news/programming-the-baofeng-dm-32uv-part-one-preparations)

---

## 8. Forums, communities and Discord

| Place | What's there |
|---|---|
| [infotex58.ru topic 1148](http://infotex58.ru/forum/index.php?topic=1148.0) | Main DM-32 thread (RU), origin of mods and new firmware |
| [infotex58.ru topic 1168](http://infotex58.ru/forum/index.php?topic=1168.0) | **OpenDM32** (OpenGD77 port), downloads |
| [infotex58.ru topic 1155](http://infotex58.ru/forum/index.php?topic=1155.0) | Memory dump tool |
| [RadioReference: Baofeng UV32/DM32](https://forums.radioreference.com/threads/baofeng-uv32-dm32.484874/) | Largest English thread |
| [RadioReference: OpenDM32 software](https://forums.radioreference.com/threads/opendm32-software.495804/) | Alternative firmware discussion |
| [GitHub M7OCM Discussions](https://github.com/M7OCM/DM-32UV/discussions) | Firmware archive and discussion |
| [gzalo/dm32-uv](https://github.com/gzalo/dm32-uv) | Bootloader docs, fonts, schematics |
| [Chicago-Offline/dm32-info](https://github.com/Chicago-Offline/dm32-info) | Best-documented notes (firmware, hashes, flashing from macOS) |
| [opengd77.org](https://opengd77.org/) | OpenGD77 forum, incl. grid.radio |
| Reddit: r/DMR, r/baofeng, r/amateurradio | Q&A |
| Facebook group "Baofeng DM-32UV" | Files, mods |
| Poland: [sierraecho.pl](https://sierraecho.pl/opengd77-w-nowej-odslonie/), [przemienniki.net](https://przemienniki.net) | Polish write-ups and repeater list |

**Discord:** I found no verified Discord server dedicated to the DM-32UV or OpenGD77. Active servers worth checking (invites are on the projects' official sites): **Meshtastic** (meshtastic.org), **MeshCore** (meshcore.co.uk), **Pi-Star / WPSD** hotspot community, **M17 Project** (m17project.org), the r/amateurradio community server. Reticulum mostly uses Matrix. I am not listing invite links because I could not verify them.

---

## 9. Repeaters and relay systems, and how to use them with the DM-32

| System | What it is | How to use with the DM-32UV |
|---|---|---|
| **DMR repeaters (SR…)** | Local UHF/VHF repeaters, in Poland mostly Brandmeister | Set RX/TX with offset, **Color Code**, **timeslot**, talkgroups. Lists: [przemienniki.net](https://przemienniki.net), [brandmeister.network](https://brandmeister.network) |
| **Brandmeister** | Global DMR network over the internet | TG **260** Poland (TS1), **2601–2609** districts SP1–SP9, **91** worldwide, **9** local, **9990** echo test (private call), **4000** unlink |
| **TGIF, FreeDMR, DMR+, XLX** | Alternative DMR networks and reflectors | Usually via a hotspot (network selection in Pi-Star/WPSD) |
| **MMDVM hotspot (Pi-Star/WPSD, OpenSPOT)** | Your own 10 mW mini-repeater on the internet | Simplex or duplex channel on 430–440 MHz, CC1, TS2 (simplex). Works at home with no repeater in range |
| **APRS** | Positions and short messages on 144.800 MHz + APRS-IS | Stock DM-32: GPS and DMR position. OpenDMR: **APRS beacons** (incl. satellites) |
| **EchoLink / AllStarLink** | Analogue FM over the internet (VoIP) | Through a local FM repeater/node with EchoLink; DM-32 in FM |
| **DVSwitch / DMRGateway / MMDVM_Bridge** | Cross-mode bridges DMR ↔ YSF ↔ D-STAR ↔ P25 ↔ NXDN ↔ AllStar | The basis for your own gateway: talk from the DM-32 to other systems |
| **M17 reflectors** | Open digital mode (Codec 2) | Only with OpenDMR firmware |
| **Satellites** | FM satellites; QO-100 (2.4/10 GHz) | DM-32 cannot do QO-100; OpenDMR predicts satellite passes |

**Getting started:**
1. Get the operator certificate (UKE exam in Poland, category A or C) and a callsign.
2. Register a **DMR ID** at [radioid.net](https://radioid.net) (requires a callsign).
3. In the CPS enter your ID, add the nearest SR repeater from przemienniki.net, group contacts 9, 91, 260, 2601–2609 and private 9990.
4. Test: private call 9990 (Parrot) plays your own audio back.

**Polish law (important):** transmitting on amateur bands requires a certificate and licence (callsign). The DM-32UV is **not type-approved for PMR446**, so using it on PMR channels is not legal (PMR446 means 0.5 W and a fixed antenna). CB 27 MHz needs a different radio.

---

## 10. Alternative radios

| Radio | Modes | Why | Alternative firmware |
|---|---|---|---|
| **AnyTone AT-D878UVII Plus** | DMR, FM, APRS, Bluetooth | most popular mature DMR HT | – |
| **AnyTone AT-D168UV** | DMR, FM, APRS | newer, cheaper | – |
| **TYT MD-UV390 / MD-380** | DMR, FM | classic | **OpenRTX**, md380tools |
| **Radioddity GD-77 / Baofeng DM-1801 / RD-5R** | DMR, FM | cheap, officially supported by OpenGD77 | **OpenGD77** |
| **Retevis RT3S** | DMR, FM | MD-UV380 equivalent | OpenRTX |
| **Connect Systems CS7000 (M17)** | DMR, M17, FM | M17 from factory | OpenRTX |
| **Quansheng UV-K5 / UV-K6** | FM, AM/SSB RX | very cheap, big community | **egzo, F4HWN (armel)** |
| **Yaesu FT5D / FTM-300** | C4FM, FM, APRS | System Fusion, WIRES-X | – |
| **Icom ID-52 Plus** | D-STAR, FM | D-STAR, terminal mode | – |
| **LoRa modules (Heltec V3/V4, LilyGO T-Beam/T-Deck, RAK WisBlock, Seeed T1000-E)** | LoRa 868 MHz | Meshtastic / MeshCore, licence-free | Meshtastic, MeshCore, RNode |

---

## 11. Systems bridging amateur radio, internet and mesh

| System | Medium | What it bridges | Status |
|---|---|---|---|
| **Meshtastic** | LoRa 868/433 MHz | text, position, telemetry; **MQTT**, **APRS** (aprstastic), **Matrix** (meshtastic-matrix-relay) gateways | very large community |
| **MeshCore** | LoRa | text; dedicated repeaters, room servers; MQTT gateways | growing fast, scales better than flooding |
| **Reticulum (RNS) + LXMF / Sideband / NomadNet** | LoRa (RNode), WiFi, Ethernet, packet radio, HF modems, I2P, TCP | any medium, end-to-end crypto, serverless routing | mature stack, closest to "all technologies" |
| **MeshForge / MeshAnchor** | MQTT | Meshtastic ↔ Reticulum ↔ MeshCore | early (2026) |
| **Mesh-Client** | app | Meshtastic + MeshCore + Reticulum in one client | early |
| **AREDN** | WiFi on amateur bands (2.4/3.4/5.8 GHz) | IP network for hams: VoIP, cameras, chat | mature |
| **HAMNET** | 5 GHz PtP links between repeater sites | European amateur IP backbone | active incl. DE, AT, PL |
| **Winlink + VARA / Pactor** | HF/VHF | e-mail over radio, RMS gateways to the internet | emergency-comms standard |
| **JS8Call** | HF | store-and-forward text | active |
| **APRS / APRS-IS** | VHF + internet | positions, messages, SMS/e-mail gateways | mature |
| **EchoLink / AllStarLink / DVSwitch** | radio + VoIP | voice bridges FM, DMR, YSF, D-STAR, P25, SIP (Asterisk) | mature |
| **Zello gateway** | radio ↔ app | CB/FM radio ↔ phone app | popular outside ham |

Sources: [MeshForge](https://github.com/nursedude/meshforge), [MeshCore RFC Reticulum #1736](https://github.com/meshcore-dev/MeshCore/discussions/1736), [Mesh-Client](https://github.com/Colorado-Mesh/mesh-client), [aprstastic](https://gitee.com/radiolee/aprstastic), [Meshcore Repeater MQTT Gateway](https://github.com/jmead/Meshcore-Repeater-MQTT-Gateway), [Meshtastic vs MeshCore vs Reticulum](https://simeononsecurity.com/articles/meshtastic-vs-meshcore-vs-reticulum-comparison-guide/)

---

## 12. Shopping and preparation list

1. **FTDI K1 cable** (e.g. BTECH PC03). If you have the bundled CH340, start with that.
2. **Chrome/Edge** (grid.radio) or a **Windows** PC/VM (Baofeng CPS), **Python 3 + pyserial**.
3. **Charged battery**, optionally a spare 3800 mAh USB-C.
4. **Better antenna** (NA-771 / AR-771 / Signal Stick) after checking the connector.
5. Optional **hotspot** (Raspberry Pi Zero 2 W + MMDVM_HS duplex hat + WPSD) and a base antenna.
6. **UKE operator certificate + callsign + DMR ID** (radioid.net) before you transmit.

**Order:** check SK1/SK2 and firmware version → CPS codeplug backup → 2× backup of both flash chips → save to Drive with the serial number → flash OpenDM32 (grid.radio) or OpenDMR → default codeplug → your channels. Going back: PTT+SK1 and the stock `.bin` from 02_stock_firmware_ROW_DM32.01.

---

## Sources

- [Install OpenGD77 on Baofeng DM-32_EN.pdf (your Drive)](https://drive.google.com/file/d/1CwFaVCXYHuIuXXvOd0USH-iQM_xWVDS9/view)
- [M7OCM/DM-32UV – CPS and firmware archive](https://github.com/M7OCM/DM-32UV)
- [rogerclarkmelbourne/DM32 – VK3KYY Python tools](https://github.com/rogerclarkmelbourne/DM32)
- [va3bpf/OpenDMR](https://github.com/va3bpf/OpenDMR) · [installation guide](https://github.com/va3bpf/OpenDMR/blob/develop/docs/installation.en.md)
- [Chicago-Offline/dm32-info](https://github.com/Chicago-Offline/dm32-info)
- [grid.radio OpenGD77 CPS](https://grid.radio/opengd77/) · [opengd77.org: The Grid Radio Browser-Based CPS](https://opengd77.org/viewtopic.php?t=55) · [sierraecho.pl: OpenGD77 w nowej odsłonie](https://sierraecho.pl/opengd77-w-nowej-odslonie/)
- [Reddit r/DMR: Baofeng DM32 and OpenGD77 firmware](https://www.reddit.com/r/DMR/comments/1w2rlrb/baofeng_dm32_and_opengd77_firmware/) · [xreflector.es: OpenGD77 supports the DM-32](https://radio.xreflector.es/index.php/2026/07/06/opengd77-ya-incorpora-soporte-para-el-baofeng-dm-32/)
- [Baofeng DM-32UV product page](https://www.baofengradio.com/products/dm-32uv)
- [przemienniki.net SR5OK (example TG configuration)](https://przemienniki.net/przemienniki/70cm/SR5OK?setlang=en)
