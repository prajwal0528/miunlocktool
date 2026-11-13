# Xiaomi Unlock Tool (Mi Unlock Tool Revamp)

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-GPL--3.0--only-blue.svg?style=for-the-badge" alt="License: GPL-3.0-only"></a>
  <a href="https://github.com/AsInsideOut/miunlocktool/releases/latest"><img src="https://img.shields.io/badge/Release-v8.2%20Stable-success.svg?style=for-the-badge" alt="Latest Release"></a>
  <img src="https://img.shields.io/badge/Python-3.10%2B-3776AB.svg?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.10+">
  <img src="https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS%20%7C%20Android-informational.svg?style=for-the-badge" alt="Platforms">
  <a href="https://miunlock.su"><img src="https://img.shields.io/badge/Website-miunlock.su-orange.svg?style=for-the-badge" alt="Official Website"></a>
  <a href="https://t.me/miunlocktoolrevamp"><img src="https://img.shields.io/badge/Telegram-@miunlocktoolrevamp-2CA5E0.svg?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram Community"></a>
</p>

<p align="center">
  <strong>The most advanced open-source utility for bypassing Xiaomi HyperOS & MIUI bootloader unlocking quotas with sub-millisecond NTP precision.</strong>
</p>

<p align="center">
  <a href="#english"><b>English</b></a> • <a href="#русский"><b>Русский</b></a>
</p>

---

<a name="english"></a>
## English

### Overview

**Xiaomi Unlock Tool** is an open-source, community-driven utility designed to automate the bootloader unlock authorization process on Xiaomi, Redmi, and POCO smartphones running **Xiaomi HyperOS (1, 2, and 3)** as well as **MIUI 14**.

Starting with HyperOS, Xiaomi implemented strict global daily quotas (~1,500–2,000 devices worldwide) that reset exactly at **00:00:00 Beijing Time (UTC+8)**. Manually clicking the unlock button in the Xiaomi Community app almost always results in *"Limit reached"*.

Xiaomi Unlock Tool solves this by combining:
1. **Stratum-1 NTP time synchronization** across multiple global servers (Google, Cloudflare, pool.ntp.org).
2. **Dynamic round-trip latency compensation** measuring ping directly against Xiaomi's authentication gateways (`sgp-api.buy.mi.com`).
3. **Automated sub-millisecond submission triggers** that land your request precisely as the daily quota window opens.

---

### Key Features

* **Sub-Millisecond Precision Engine:** Synchronizes against global atomic NTP clocks and auto-calibrates network round-trip time right at `23:59:59.xxx` Beijing time.
* **Safe & Direct Protocol:** Communicates directly with Xiaomi's official endpoints over HTTPS. No middleman servers, no data logging, no credentials stored.
* **Modern Desktop GUI:** Crafted with CustomTkinter (Material Design 3 style), featuring dark/light themes, custom accent colors, and real-time logs.
* **Headless CLI Version:** Standalone Python CLI (`sourcecode/cli/miunlock_cli.py`) for servers, VPS, Linux, macOS, and automated cron setups.
* **Native Android App (In Development):** A Kotlin-based native app (`sourcecode/android`) allowing direct quota application from your phone without a PC.
* **Flexible Authentication:** Supports automatic cookie extraction from Chromium-based browsers, manual token input, or instant QR-code login.
* **Multi-Language Interface:** Full native support for 10+ languages (English, Russian, Spanish, Chinese, Turkish, Portuguese, Hindi, etc.).
* **Regional Timing Profiles:** Pre-configured latency presets for Europe, Asia, and the Americas, plus up to 15 custom timing triggers.

---

### Supported Devices & Firmware

| Firmware Region | Code | Support Status | Notes |
| :--- | :---: | :---: | :--- |
| **Global** | `MI` | Supported | Official global firmware |
| **Europe** | `EEA` / `EU` | Supported | European Union devices |
| **Russia** | `RU` | Supported | Russian regional firmware |
| **India** | `IN` | Supported | Indian devices |
| **Indonesia** | `ID` | Supported | Indonesian devices |
| **Turkey / Taiwan** | `TR` / `TW` | Supported | Turkish & Taiwanese regional firmware |
| **China** | `CN` | Not Supported | Requires Mainland China Mi Community Lv.5 status |

> [!IMPORTANT]
> **Account Prerequisites:**
> 1. Your Xiaomi (Mi) account must be **older than 30 days**.
> 2. In the Xiaomi Community app / web profile, your region **MUST be set to Global**.
> 3. After the tool secures your quota, account binding in *Developer Options -> Mi Unlock Status* **must be performed strictly over mobile data (SIM card)** with Wi-Fi switched off.

---

### Installation & Quick Start

#### Option A: Windows (Pre-compiled Portable EXE)
1. Download the latest `Xiaomi Unlock Tool 8.1.zip` from [Releases](https://github.com/AsInsideOut/miunlocktool/releases/latest) or our official website [miunlock.su](https://miunlock.su).
2. Extract the ZIP archive to any folder.
3. Launch `Xiaomi Unlock Tool 8.1.exe`. No installation or Python setup needed.

#### Option B: Linux / macOS / Run from Python Source
1. Ensure Python **3.10 or newer** is installed:
   ```bash
   python --version
   ```
2. Clone this repository:
   ```bash
   git clone https://github.com/AsInsideOut/miunlocktool.git
   cd miunlocktool
   ```
3. Install required Python packages:
   ```bash
   pip install customtkinter packaging ntplib darkdetect pytz urllib3 icmplib requests pygame-ce
   ```
4. Run the GUI application:
   ```bash
   python sourcecode/pc/8.1.py
   ```
   Or run the lightweight Terminal CLI version:
   ```bash
   python sourcecode/cli/miunlock_cli.py
   ```

---

### Step-by-Step Instructions

1. **Launch the tool** and review the initial welcome dialogue.
2. Open **"Instructions and Script settings"** -> **"Getting Cookies"** and select your browser (or use the built-in QR Code login).
3. Retrieve your `new_bbs_token` and paste it into the main token field.
4. Click **"Start application"**.
5. **Timeline of operation (Beijing Time GMT+8):**
   * **23:58:00** — Precision clock resynchronization via NTP.
   * **23:59:40** — Real-time latency (ping) calculation to Xiaomi's servers.
   * **23:59:58 – 23:59:59.xxx** — Timed authorization requests are dispatched.
6. At **00:01:00 Beijing Time**, proceed to your smartphone's *Settings -> Additional settings -> Developer options -> Mi Unlock status* and tap **"Add account and device"** (using mobile SIM data).

---

### Common Issues & Solutions

| Error Message | Cause & Recommended Action |
| :--- | :--- |
| **"Couldn't add. Please go to Mi Community to apply..."** | Daily quota full. The server limit (~1,500/day) was exhausted before your request landed. Re-run the tool tomorrow with adjusted timing offsets. |
| **"System is being upgraded. Please try again later"** | Known Xiaomi firmware glitch (frequent on Redmi 13 / Note 13 series). Wait for the next HyperOS OTA or retry in a few days. |
| **"The account is not authorized"** | Account is younger than 30 days or Community region is not set to Global. |
| **"Wait timer (168 / 360 hours)"** | **Success!** Your account is approved and bound. You only need to wait out the mandatory security timer before running the official PC unlocker. |

---

### License

This project is licensed under the **GNU General Public License v3.0 only** (`GPL-3.0-only`).  
See the [LICENSE](LICENSE) file for the full legal text.

* **Summary of Terms:**
  * **Freedom:** You may run, study, modify, and redistribute this software.
  * **Copyleft (Section 6):** All forks, modifications, and derivative distributions **MUST** be open-sourced under the exact same `GPL-3.0-only` license. Closed-source binary redistribution or proprietary repackaging is strictly prohibited.
  * **Attribution (Sections 4, 5a, 5d):** Any derivative work **must preserve** all original copyright notices (`Copyright (C) 2026 AsInsideOut`), state that it was modified, and maintain visible legal notices in the UI / CLI.
  * **No Warranty:** Provided "AS IS", without warranty of any kind.

```text
SPDX-License-Identifier: GPL-3.0-only
Copyright (C) 2026 AsInsideOut
```

---

### Credits & Acknowledgments

* **[offici5l](https://github.com/offici5l/)** — Creator of the original Python-based *Xiaomi Flash Unlock* concept and pioneer in community unlock research.
* **[AsInsideOut](https://github.com/AsInsideOut)** — Complete project revamp, custom NTP timing engine, CustomTkinter modern UI, CLI edition, and cross-platform ecosystem.

---

### Contacts & Community

* **Official Website:** [https://miunlock.su](https://miunlock.su) (Mirror: [https://miunlocktool.ru](https://miunlocktool.ru))
* **Telegram Channel & Chat:** [@miunlocktoolrevamp](https://t.me/miunlocktoolrevamp)
* **4PDA Discussion Topic:** [4PDA Xiaomi Unlock Tool Forum](https://4pda.to/forum/index.php?showtopic=721838&view=findpost&p=145201115)
* **Reddit:** [u/AsInsideOut](https://reddit.com/u/AsInsideOut)
* **Support Email:** `support@miunlock.su`

#### Support the Project
* **Crypto (USDT / TON):** `UQBCkvXTF-NiT7P8g0G-4oiAdsU81rMWJNYBTxDrbl76FabG`
* **Bitcoin (BTC):** `bc1qzzt00sk0zdxgskw8xx8cjh4jl69wl7fvlshhsh`
* **DonationAlerts:** [https://www.donationalerts.com/r/asinsideout](https://www.donationalerts.com/r/asinsideout)
* **DaLink:** [https://dalink.to/asinsideout](https://dalink.to/asinsideout)

---
