# DM-32UV files: sources, checksums, what is where

Collected: 2026-11-27. Verify every file with `SHA256SUMS.txt` before use (`shasum -a 256 <file>` on macOS, `certutil -hashfile <file> SHA256` on Windows).

## Sources

| Folder | Contents | Origin |
|---|---|---|
| 01_official_Baofeng | CPS v1.60 (2026-06-08), Firmware pack 2025-08-22 (CPS 1.45 + FW L01.048), Firmware 046 (2025-03-27), User Manual 2025-02-10, Picture Tool, FCC certificate | baofengradio.com/pages/download, unmodified copies archived by M7OCM (github.com/M7OCM/DM-32UV, branch Official-Baofeng-downloads), fetched via github.com/rogerclarkmelbourne/DM32 mirror @ 4b21859 |
| 02_stock_firmware_ROW_DM32.01 | Stock firmware for the common ("ROW", ridged SK1/SK2) hardware: 032-049, L01.048, 01.02.046, Taiwan NRF/Fanti 049 | M7OCM, branch DM-32UV-Firmware-(most-common) |
| 03_stock_firmware_HR_Vocoder_DM32.00 | Stock firmware for the HR-Vocoder (smooth SK1/SK2) hardware: 034, 037, 046 | M7OCM, branch HR-Vocoder-DM-32UV-Firmware |
| 04_backup_and_flash_tools | OpenUV008 flasher (andynvkz), VK3KYY Python tools, OpenDMR dm32_dump.py backup/write tool, gzalo/dm32-uv docs | M7OCM; github.com/rogerclarkmelbourne/DM32/python; github.com/va3bpf/OpenDMR @ f48c7e3; github.com/gzalo/dm32-uv |
| 05_CPS_all_versions | Baofeng CPS 1.22-1.50 installers, fonts, default OEM codeplug, default adjust-mode file | M7OCM, branch CPS |
| 06_mods_use_at_own_risk | RA4FHE band-extension mods (046 / 049), inverted-screen "Light" 049, ALPU-check-free 049, VK3KYY CPS with adjust-mode password removed | M7OCM branch Mods (from infotex58.ru topic 1148); rogerclarkmelbourne/DM32/CPS_HACKED |
| 07_software_logo_record | Record tool, original boot logo, DMR-APRS image | M7OCM, branch DM-32UV-Software-(Record-&-PoP) |
| 08_OpenDMR_source_snapshot | Full OpenDMR source tree (no release binaries) | github.com/va3bpf/OpenDMR @ f48c7e3 (2026-11-27) |

Hashes of DM32.01.01.049, DM32.01.L01.048, DM32_NRF_049 and DM32_Fanti_049 match the independent list in github.com/Chicago-Offline/dm32-info (FIRMWARE.md).

**Warning on 06_mods:** extended-band firmware does not make the hardware work outside its design range, and transmitting outside your licensed bands is illegal. Keep for research/RX only. Never flash 01.* firmware onto HR-Vocoder (00.*) hardware or vice versa: it soft-bricks the radio (recovery: battery out, hold SK1+PTT, power on, reflash correct file).

## Not on Google Drive (too large for the Drive connector, max ~3 MB per file)

Available in the project Library (Claude project files) under `radio/files/2026-11-27_DM-32UV/`:

- 01_official_Baofeng/BAOFENG_DM-32UV_CPS_v1.60_20260608.zip (11.6 MB)
- 01_official_Baofeng/Baofeng_DM-32_UV_Firmware_20250822.zip (4.8 MB)
- 01_official_Baofeng/Baofeng_DM-32UV_User_Manual_20250210.pdf (4.7 MB)
- 01_official_Baofeng/Baofeng_DM-32UV_Picture_Tool.zip (7.2 MB)
- 05_CPS_all_versions/CPS DMR Radio Setup v1.22 ... v1.50.exe (11 files, 4.2-4.5 MB each)
- 06_mods_use_at_own_risk/DM32_CPS_HACKED.zip (4.3 MB)
- 08_OpenDMR_source_snapshot/OpenDMR_source_f48c7e3.zip (5.9 MB)

Direct download alternative: https://github.com/M7OCM/DM-32UV (each branch).

## Could not be downloaded (host blocked from the research environment)

| Item | Where to get it | Expected SHA256 of the .bin |
|---|---|---|
| OpenDM32 HotFix 2026-09-27 (latest OpenGD77 port) | infotex58.ru/forum/index.php?topic=1168.0, attachment 4616 (OpenDM32.zip) | 742653d5731cab98fba7f37c60bea24f5762fe63791add1a9d1142a316fb5f05 |
| OpenDM32_20260801_DCS_BS_Act | same topic, attachment 4614 | 6197c140d523f237a172aad51ccb1b9c7e21168087be5f3934b97e8d7498c556 |
| OpenGD77_DM32_20260711_addid_2018 | same topic, attachment 4602 | 2f5419552a9e365351f8b857dd528e07032ec96a4bb5b73ab1f6d764942fc5ba |
| OpenGD77_DM32_20260704 | same topic | 9a07f7d878e5bd0dd3f7a97b280cb455657e9474f77b54a0e7b2e48d4976dcc3 |
| OpenGD77 CPS with DM32/UV008 (E2026.05.26.01 official; E2026.07.13.01 on infotex58) | opengd77.com/downloads/PC_CPS/Experimental/ | check on site |
| grid.radio browser CPS / web flasher | https://grid.radio/opengd77/ (runs in Chrome/Edge, nothing to download) | n/a |
| OpenDMR release package (OpenDMR-<version>.zip, latest v0.8.2, 2026-11-22) | https://github.com/va3bpf/OpenDMR/releases | SHA256SUMS.txt in the release |
| OpenUV008 newer builds | infotex58.ru topic 1155 | n/a |
| CH340 / FTDI drivers | wch-ic.com (CH340), ftdichip.com (VCP) | vendor site |
