# NYKI-9Z Pentest Device — System Architecture
> Rev 1.0 — FINAL
> Lab: NYKI-9Z Security Research
> Hardware: LilyGO T-Display (ESP32-D0WDQ6 r1) + ESP32-S3 DevKit (QFN56, screenless)
> Last Verified: 2026-03-04 against esptool + espefuse + Espressif silicon docs

---

## Part 0 — Verified Hardware Baseline

Both devices have been profiled and cross-checked against Espressif silicon documentation.
No assumptions — only confirmed values are used in this document.

### Device 1 — LilyGO T-Display (Radio Engine + Scope Display)

| Parameter | Confirmed Value |
|---|---|
| Chip | ESP32-D0WDQ6 revision ECO1 (r1) |
| MAC | 94:54:C5:B6:AD:AC |
| Flash | 16MB Winbond W25Q128 |
| Crystal | 26 MHz |
| CPU | Dual-core Xtensa LX6, 240 MHz |
| USB Bridge | QinHeng CH9102, 921600 baud |
| Screen | 135×240 ST7789V, 1.14" |
| Buttons | GPIO 0 (BOOT/A), GPIO 35 (B) |
| Bluetooth | BR/EDR (Classic) + BLE 4.2 — full dual-mode simultaneous |
| WiFi | 802.11 b/g/n |
| CVE-2025-27840 | Confirmed present — original D0WD die only |
| Hidden HCI | 0xFC0E (raw LMP) · 0xFC43 (raw LLCP) · 0xFC32 (MAC spoof) |

### Device 2 — ESP32-S3 DevKit (Brain + BLE5 Engine + WiFi Server)

| Parameter | Confirmed Value |
|---|---|
| Chip | ESP32-S3 v0.2, QFN56 package |
| MAC | 30:ED:A0:B7:5A:80 |
| Flash | 16MB Boya BX25Q128 |
| PSRAM | 8MB Octal (AP Memory Gen3, 80MHz, 3.3V) ← confirmed from boot log |
| Crystal | 40 MHz |
| CPU | Dual-core Xtensa LX7, 240 MHz |
| USB Bridge | QinHeng CH343 |
| Screen | None |
| Bluetooth | BLE 5.0 ONLY — no BR/EDR whatsoever |
| WiFi | 802.11 b/g/n |
| CVE-2025-27840 | Not present — S3 silicon does not have these commands |
| Security | All eFuse security disabled — full R/W/debug access |
| Partition: factory | 3,968 KB app slot |
| Partition: spiffs | 12,288 KB (12MB) filesystem — 11,287 KB currently free |

> Warning: One correction to your S3 spec document:
> The "Wireless Radios" table and "Hidden HCI Commands" section in the spec doc
> were manually authored and contain errors. The S3 is BLE 5.0 only — no BR/EDR,
> no A2DP, no HFP, no HID Classic, no AVRCP, no BNEP, no SDP Classic.
> CVE-2025-27840 hidden commands (0xFC0E, 0xFC32, 0xFC43) do not exist on S3.
> Those sections should be removed from that reference document before it is used
> during firmware development to avoid building against a false capability map.

---

## Part 1 — Role Division

The split is defined entirely by silicon. Neither chip is being asked to do
anything outside its confirmed hardware capability.

```
┌─────────────────────────────────────────────────────────────────┐
│  RADIO ENGINE + SCOPE DISPLAY                                        │
│  LilyGO T-Display — ESP32-D0WDQ6 r1                                 │
│  Screen: 135×240 ST7789V (1.14")                                     │
├─────────────────────────────────────────────────────────────────┤
│  RADIO:                                                              │
│  - Bluetooth Classic BR/EDR — full profile support                   │
│    (HID, A2DP, HFP, AVRCP, SDP, BNEP, L2CAP, LMP)                  │
│  - BLE 4.2 — central + peripheral simultaneously                     │
│  - CVE-2025-27840 hidden HCI (0xFC0E, 0xFC32, 0xFC43)               │
│  - Dual-mode BT+BLE active simultaneously                            │
│                                                                      │
│  OWNS ALL BR/EDR ATTACKS:                                            │
│  - BlueDucky (01) — HID keystroke injection                          │
│  - BlueSpy (07) — HFP mic activation                                 │
│  - BrakTooth (06) — LMP baseband fuzzing (native platform)           │
│  - PerfektBlue (08) — AVRCP malformed PDU via Component D            │
│  - BlueBorne (22) — SDP info leak + BNEP overflow via Components D/E │
│  - KNOB probe (19) — LMP key entropy downgrade via Layer B           │
│  - BIAS probe (20) — device impersonation via Layer B + 0xFC32       │
│  - BLUFFS probe (03) — session key downgrade via Layer B             │
│  - BlueFrag (21) — L2CAP zero-click via Layer A                      │
│  - BleedingTooth (23) — Linux kernel RCE via Layer A                 │
│  - Android Zero-Click (09) — L2CAP via Layer A                       │
│  - BLE MITM peripheral side (for GATTacker split-role)               │
│  - Blue's Clues (10) — BD_ADDR exfil from non-discoverable devices   │
│                                                                      │
│  SCREEN ROLE — two modes, button B toggles:                          │
│                                                                      │
│  MODE 1 — STATUS (default when idle)                                 │
│  ┌─────────────────────────────────┐                                 │
│  │ NYKI-9Z              BAT: 87%  │                                  │
│  │ ──────────────────────────────  │                                 │
│  │ BT: SCANNING    BLE: IDLE      │                                  │
│  │ TARGETS: 7      WiFi: AP UP    │                                  │
│  │ LAST: BlueDucky 12:04:33       │                                  │
│  └─────────────────────────────────┘                                 │
│                                                                      │
│  MODE 2 — ATTACK SCOPE (auto on attack launch, button B to hold)    │
│  ┌─────────────────────────────────┐                                 │
│  │ [KNOB PROBE]        00:00:14   │                                  │
│  │ ──────────────────────────────  │                                 │
│  │ >CMD FC0E LMP_enc_key_size_req │                                  │
│  │ <EVT 0E04 LMP_accepted         │                                  │
│  │ KEY ENTROPY: 1 BYTE            │                                  │
│  │ !! VULNERABLE !!               │                                  │
│  └─────────────────────────────────┘                                 │
│                                                                      │
└─────────────────────────────────────────────────────────────────┘
                           │
              4-wire UART bus @ 921600 baud
              T-Display GPIO17 TX ──► S3 GPIO18 RX
              T-Display GPIO21 RX ◄── S3 GPIO17 TX
              GND ──────────────────── GND
              (optional: GPIO22 for hardware flow control)
                           │
┌─────────────────────────────────────────────────────────────────┐
│  BRAIN + BLE5 ENGINE + WiFi OPERATOR CONSOLE                         │
│  ESP32-S3 DevKit — QFN56, no screen                                  │
│  8MB PSRAM + 16MB Flash (12MB SPIFFS)                                │
├─────────────────────────────────────────────────────────────────┤
│  RADIO:                                                              │
│  - BLE 5.0 — 1M/2M PHY, Coded PHY (Long Range), Extended Adv        │
│  - WiFi 802.11 b/g/n — AP mode serving operator web console         │
│  - NO BR/EDR — any Classic BT command is routed to T-Display        │
│                                                                      │
│  OWNS ALL BLE5 ATTACKS:                                              │
│  - WhisperPair (02) — Fast Pair GATT force-pair                      │
│  - SweynTooth (05) — BLE LL malformed PDU via NimBLE low-level       │
│  - BLESA (25) — BLE reconnection spoofing                            │
│  - BLEEDINGBIT (24) — TI chip BLE adv overflow beacon crafting       │
│  - GATTacker MITM central side — connects to real BLE target         │
│                                                                      │
│  SYSTEM ROLE:                                                        │
│  - Master orchestrator — all attacks launched from web UI via S3     │
│  - Routes BR/EDR attack commands to T-Display over UART              │
│  - Aggregates logs from both radios                                  │
│  - Stores session data to 12MB SPIFFS (thousands of sessions)        │
│  - 8MB PSRAM: scan database, packet buffers, web UI assets in RAM    │
│  - Serves single-page web app to operator phone at 192.168.4.1       │
│  - OTA firmware distribution for both chips over WiFi                │
│                                                                      │
└─────────────────────────────────────────────────────────────────┘
                           │
                      WiFi AP mode
                      SSID: NYKI-9Z (hidden recommended)
                      192.168.4.1:80 — web console
                      192.168.4.1:81 — WebSocket (live log)
                           │
                ┌──────────▼──────────┐
                │  Operator's Phone   │
                │  Browser — no app   │
                │  Full UI + logs     │
                │  Attack control     │
                └─────────────────────┘
```

---

## Part 2 — Memory Architecture (Accurate)

### ESP32-S3 (Brain)

With 8MB PSRAM confirmed, the memory story is excellent:

| Resource | Location | Capacity |
|---|---|---|
| BLE scan results (live ring buffer) | PSRAM | 500+ entries comfortably |
| BR/EDR scan results (relayed from T-Display) | PSRAM | 500+ entries |
| Packet capture buffers | PSRAM | Up to 4MB allocated |
| Web UI assets (HTML/CSS/JS) | PSRAM (copied from SPIFFS at boot) | ~100KB |
| WebSocket send buffers | PSRAM | 3 clients × 64KB each |
| UART bridge buffer (T-Display → S3) | PSRAM | 64KB ring buffer |
| Session logs (persistent, current session) | PSRAM ring + SPIFFS flush | PSRAM: 1MB live; SPIFFS: unlimited |
| Historical session logs | SPIFFS (12MB) | ~100+ full sessions before rotation |
| OTA firmware staging | SPIFFS | ~2MB per chip image |
| Attack profiles / target database | SPIFFS | Thousands of entries |

**Total available heap at runtime (estimated):**
- Internal SRAM: ~307KB (254 + 21 + 32KB from boot log)
- PSRAM pool: ~8,160KB (8MB minus 32KB DMA reserve)
- Total: ~8,467KB usable — this is a genuinely capable platform

### ESP32-D0WDQ6 (T-Display / Radio Engine)

The T-Display has no PSRAM. Internal SRAM is 520KB total on the original ESP32,
with BT dual-mode + WiFi stack consuming ~180KB, leaving ~300KB free for the application.

| Resource | Location | Capacity |
|---|---|---|
| LMP PDU construction buffers | SRAM (stack allocated) | 32 bytes, ephemeral |
| HCI ACL bypass send buffer | SRAM | 1KB |
| BLE scan result staging (before UART flush) | SRAM | 10 entries max |
| BR/EDR connection state | SRAM | Per Bluedroid internals |
| UART TX buffer (to S3) | SRAM | 4KB ring buffer |
| Screen framebuffer | SRAM (DMA) | ~135×240×2 = ~64KB |
| Attack execution state machine | SRAM | <4KB |

**Rule:** T-Display holds nothing long-term. All results flush to S3 over UART
immediately. T-Display is stateless between attacks by design — keeps memory clean
for the next operation.

---

## Part 3 — Inter-Chip UART Bus

### Physical Wiring

```
T-Display (ESP32)        ESP32-S3 DevKit
─────────────────        ───────────────
GPIO17 (UART2 TX) ──────► GPIO18 (UART1 RX)
GPIO21 (UART2 RX) ◄────── GPIO17 (UART1 TX)
GND               ──────── GND
```

> GPIO conflict check (T-Display):
> Screen uses: GPIO4, 5, 16, 18, 19, 23. Buttons: GPIO0, 35.
> GPIO17 and GPIO21 are free and confirmed safe.

> GPIO conflict check (S3):
> GPIO26–32: reserved for SPI flash — do not use.
> GPIO33–37: reserved for Octal PSRAM (confirmed present) — do not use.
> GPIO17 and GPIO18: free, not strapping pins, safe.

### Frame Protocol

Every message on the bus uses a lightweight framed structure to prevent
partial-packet corruption and allow easy parsing on both ends:

```
┌──────┬──────┬──────┬────────────────┬──────┐
│ 0xAA │ LEN  │ TYPE │    PAYLOAD     │ CRC8 │
│  1B  │  1B  │  1B  │   0–252 bytes  │  1B  │
└──────┴──────┴──────┴────────────────┴──────┘

TYPE values:
  0x01  COMMAND     S3 → T-Display   Launch/abort attack, set params
  0x02  RESULT      T-Display → S3   Attack outcome (VULN/NOT_VULN/ERROR)
  0x03  LOG_LINE    T-Display → S3   Human-readable log string (UTF-8)
  0x04  SCAN_ENTRY  T-Display → S3   Discovered device (MAC + name + RSSI + profiles)
  0x05  STATUS      Bidirectional    Heartbeat + battery level + current state
  0x06  HCI_EVENT   T-Display → S3   Raw HCI event bytes (for scope display passthrough)
```

Implementation is ~200 lines of C shared between both firmware projects.
This is the first code to write — nothing else works without it.

### Latency Budget

| Operation | UART cost | Available window | Verdict |
|---|---|---|---|
| S3 sends attack command to T-Display | ~87µs (10B frame) | Human timescale | Fine |
| T-Display sends scan entry to S3 | ~555µs (64B frame) | Human timescale | Fine |
| BLE GATT MITM relay (GATTacker) | ~87µs per PDU | 7.5ms–4s conn interval | Comfortable |
| LMP MitM intercept+substitute | ~174µs (2 passes) | 1.25ms connection event | Tight |

**MitM timing solution:** All LMP MitM decision logic executes locally on the
T-Display, pre-configured by the S3 before the attack launches. No UART
round-trips during a live connection event. The S3 sets parameters; the T-Display
executes autonomously. This is correct architecture regardless of bus speed.

---

## Part 4 — Web Operator Console

The S3 serves a single HTML file from SPIFFS at boot. Assets are copied to
PSRAM for fast serving. WebSocket on port 81 provides real-time log streaming.

### Technology Stack

| Component | Choice | Reason |
|---|---|---|
| HTTP server | ESPAsyncWebServer | Non-blocking, WebSocket native, well-tested on S3 |
| WebSocket | AsyncWebSocket (built into above) | Real-time log push without polling |
| Frontend | Single HTML file, vanilla JS, no framework | No build step, no dependencies, <50KB total |
| File storage | SPIFFS on 16MB flash | Survives power cycles, large capacity |
| Authentication | WPA2 AP password + optional HTTP basic auth | Prevents casual access in the field |

### UI Layout (wireframe)

```
┌──────────────────────────────────────────────────────────────┐
│  NYKI-9Z  LIVE                              BAT:87% 12:04  │
├──────────┬───────────────────────────────────────────────────┤
│  SCAN    │                                                    │
│  TARGETS │  ┌─ SCAN RESULTS ──────────────────────────────┐  │
│  ATTACKS │  │  [SCAN BLE 5.0]  [SCAN BR/EDR]  [STOP]   │  │
│  LOG     │  │                                              │  │
│  CONFIG  │  │  94:54:C5:AA:BB:CC  iPhone 14   BT  -67dBm │  │
│          │  │  11:22:33:44:55:66  JBL Flip 6  BT  -72dBm │  │
│          │  │  DE:AD:BE:EF:00:01  Unknown BLE  LE  -81dBm │  │
│          │  │  30:ED:A0:B7:5A:AA  AirPods Pro  LE  -75dBm │  │
│          │  └──────────────────────────────────────────────┘  │
├──────────┴───────────────────────────────────────────────────┤
│  TARGET: JBL Flip 6 | 11:22:33:44:55:66 | BR/EDR+BLE        │
│  Profiles detected: A2DP · HFP · AVRCP · GATT               │
│  ─────────────────────────────────────────────────────────── │
│  ATTACKS AVAILABLE FOR THIS TARGET:                          │
│  [BlueSpy]  [KNOB Probe]  [BIAS Probe]               │
│  [BlueDucky]  [AVRCP Fuzz]  [BlueBorne SDP]          │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│  ACTIVE: KNOB Probe → JBL Flip 6              00:00:14   │
│  ─────────────────────────────────────────────────────────── │
│  Phase: LMP Encryption Key Negotiation                        │
│  Proposed entropy: 1 byte (minimum possible)                  │
│  Remote response: LMP_accepted ← Vulnerable                   │
│  Result: TARGET ACCEPTS 1-BYTE ENCRYPTION KEY                 │
│                                                [ABORT]    │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│  SESSION LOG                            [DOWNLOAD .txt]   │
│  ─────────────────────────────────────────────────────────── │
│  12:04:11 [BT]  Connected to 11:22:33:44:55:66              │
│  12:04:11 [HCI] CMD FC0E LMP_encryption_key_size_req key=1  │
│  12:04:12 [HCI] EVT 0E04 LMP_accepted                       │
│  12:04:12 [RES] KNOB: TARGET VULNERABLE — accepts 1-byte key │
│  12:04:13 [BT]  Disconnected cleanly                         │
│  (live — new lines append in real time via WebSocket)      │
└──────────────────────────────────────────────────────────────┘
```

---

## Part 5 — T-Display Scope Screen (Attack Telemetry)

The T-Display screen serves one purpose: showing what is happening at the
radio layer during an active attack. It is not a menu system. It is closer
to an oscilloscope readout. The operator glances at it during an attack to
confirm the state machine is progressing correctly.

### Two Modes

**MODE 1 — Status (idle default):**
Driven by a FreeRTOS task polling the BT state at 2Hz. Displays:
- Device name + battery voltage (read from ADC)
- BT state (SCANNING / CONNECTED / IDLE) + BLE state
- Target count from last scan
- WiFi AP status (relayed from S3 via UART STATUS frames)
- Last attack name + timestamp

**MODE 2 — Scope (auto-activates on attack launch):**
Driven by HCI event callbacks — no polling. Every HCI command sent and
every HCI event received is formatted to a short string and pushed into
a 6-line circular buffer. Screen redraws on each new entry.
Button B holds/unfreezes the display.

### Screen GPIO (confirmed, do not use for anything else)

| GPIO | Function |
|---|---|
| 4 | TFT_CS (chip select) |
| 5 | TFT_DC (data/command) |
| 16 | TFT_RST (reset) |
| 18 | TFT_SCLK (SPI clock) |
| 19 | TFT_MOSI (SPI data) |
| 23 | TFT backlight |
| 0 | Button A (BOOT — hold for bootloader) |
| 35 | Button B (scope toggle) |

**Free GPIOs confirmed safe for bus and future peripherals:**
GPIO 17, 21, 22, 25, 26, 27, 32, 33, 34 (input only), 36 (input only), 39 (input only)

---

## Part 6 — Physical Form Factor

```
┌─────────────────────────────────────┐  ← FRONT (user-facing)
│         LilyGO T-Display            │
│    ┌───────────────────────────┐    │
│    │  1.14" ST7789V scope      │    │
│    │  135 × 240 px             │    │
│    └───────────────────────────┘    │
│    [BTN A]              [BTN B]     │
│    (bootloader)         (mode toggle)│
└───────────────┬─────────────────────┘
                │  4× M2 standoffs, 10–12mm
                │  4-wire UART harness routed internally
                │  Shared GND
┌───────────────┴─────────────────────┐  ← BACK (not user-facing)
│         ESP32-S3 DevKit             │
│         (bare PCB, no screen)       │
│                                     │
│  ←← ANTENNA MUST OVERHANG EDGE ←←  │
│  (5mm overhang clears T-Display     │
│   ground plane — critical for RF)   │
│                                     │
│  USB-C port: route to edge via      │
│  right-angle adapter for access     │
└─────────────────────────────────────┘
```

Dimensions (assembled): ~51mm × 25mm × 20mm
Weight (no battery): ~25g estimated
Battery: 3.7V LiPo, JST 1.25mm
  → T-Display has onboard TP4054 charger
  → S3 board: verify if charge circuit present on battery pin
  → If not: inline TP4056 module (1cm², ~$0.50)
Runtime at 2000mAh:
  → Both radios active, attacking: ~6.5 hours
  → Passive BLE scan only: ~18–20 hours

---

## Part 7 — Partition Table Changes Required

### S3 (Current — needs updating before OTA works)

Current partition table has no OTA slots. Before Phase 8 (OTA), reflash
the S3 with this table:

```
# partitions_s3.csv
# Name,     Type, SubType,  Offset,   Size,    Flags
nvs,         data, nvs,      0x9000,   0x5000,
otadata,     data, ota,      0xE000,   0x2000,
phy_init,    data, phy,      0x10000,  0x1000,
ota_0,       app,  ota_0,    0x20000,  0x1E0000,
ota_1,       app,  ota_1,    0x200000, 0x1E0000,
spiffs,      data, spiffs,   0x400000, 0xC00000,
```

### T-Display (add OTA + SPIFFS)

```
# partitions_tdisplay.csv
# Name,     Type, SubType,  Offset,   Size,
nvs,         data, nvs,      0x9000,   0x5000,
otadata,     data, ota,      0xE000,   0x2000,
phy_init,    data, phy,      0x10000,  0x1000,
ota_0,       app,  ota_0,    0x20000,  0x180000,
ota_1,       app,  ota_1,    0x1A0000, 0x180000,
spiffs,      data, spiffs,   0x320000, 0xCE0000,
```

---

## Part 8 — Known Gaps (Honest, Unchanged)

**Gap 1 — Full BR/EDR LMP MitM (KNOB/BIAS/BLUFFS full session decrypt):**
Requires a second BR/EDR radio. Two original ESP32s could fill this role via
the dual-unit relay architecture described in the firmware architecture document.
This is a v2 addition. Initiator-side probes are fully operational in v1.

**Gap 2 — Passive BLE promiscuous sniffing:**
Neither chip exposes a promiscuous BLE mode without deep controller firmware patches.
This device is an active attacker. Passive capture requires external hardware
(Ubertooth One, nRF52840 dongle with sniffer firmware). Different tool, different phase.

**Gap 3 — BLE 5.0 enhanced PHY SweynTooth variants:**
The T-Display's BLE controller is 4.2 only. The S3 has BLE 5.0 but no 0xFC43
equivalent. SweynTooth on the S3 is limited to what NimBLE's HCI API exposes
natively, which covers the majority of vectors but not the most timing-sensitive ones.

---

## Part 9 — Build Sequence

```
PHASE 0 — FOUNDATIONS
  Wire the 4-pin UART bus
  Implement framed binary bus protocol on both chips
  Milestone: S3 sends PING, T-Display returns PONG,
             both print to their respective outputs

PHASE 1 — T-DISPLAY SCOPE DISPLAY
  TFT_eSPI initialisation + two-mode renderer
  STATUS mode: hardcoded placeholder data
  SCOPE mode: echo incoming UART LOG_LINE frames to screen
  Milestone: bus log lines appear on T-Display screen live

PHASE 2 — S3 WEB CONSOLE SHELL
  ESPAsyncWebServer + SPIFFS serving
  WiFi AP mode, static IP 192.168.4.1
  WebSocket log stream
  Scan list and attack selector (populated with dummy data)
  Milestone: phone browser connects, sees placeholder UI,
             log lines scroll in real time

PHASE 3 — FIRST REAL ATTACK (BlueDucky — validates full pipeline)
  T-Display: implement HID peripheral + unauthenticated pair
  S3 web UI: wire "Launch BlueDucky" button to UART COMMAND frame
  Milestone: tap button on phone → UART → T-Display → BT HID injection
             → scope screen shows HCI events → result appears in web log
  This single phase proves the entire hardware + comms stack end to end.

PHASE 4 — LAYER A (HCI ACL Bypass Patch)
  Patch hci_layer.cc / btm_acl.cc on T-Display firmware
  New API: hci_send_raw_acl(handle, flags, data, len)
  Milestone: send malformed L2CAP packet → BlueFrag crash on test target
  Unlocks: BlueFrag (21) · BleedingTooth (23) · Android Zero-Click (09)

PHASE 5 — LAYER B (LMP PDU Library)
  New module: lmp_inject.c wrapping 0xFC0E with structured PDU builders
  HCI vendor event callback for LMP response parsing
  Milestone: KNOB probe connects to headphone, proposes 1-byte key,
             receives and logs LMP_accepted or LMP_not_accepted
  Unlocks: KNOB probe (19) · BIAS probe (20) · BLUFFS probe (03)

PHASE 6 — S3 BLE5 ATTACKS
  SweynTooth: NimBLE low-level LL PDU crafting
  WhisperPair: Fast Pair GATT scan + unauthenticated pair attempt
  BLESA: BLE reconnection spoofing (peripheral impersonation)
  GATTacker: S3 as BLE central to real device,
             T-Display BLE as spoofed peripheral to phone
  Milestone: WhisperPair successfully force-pairs with a Fast Pair device

PHASE 7 — COMPONENTS D + E (Profile Escape Hatches + BNEP)
  Component D: avrc_bld_ct.c patch → PerfektBlue AVRCP fuzzing
  Component D: sdp_discovery.c patch → BlueBorne SDP info leak
  Component E: bnep_attack.c mini-implementation → BlueBorne BNEP overflow
  Milestone: BlueBorne SDP info leak returns memory addresses from target

PHASE 8 — WiFi LOGGING + OTA
  Reflash both chips with OTA partition tables
  S3: implement esp_ota_ops.h update handler
  T-Display: receive OTA image from S3 over UART, apply to its own OTA slot
  Session log download button in web UI
  Milestone: update T-Display firmware from phone browser without USB cable
```

---

*All research is conducted in a controlled lab environment for authorized security testing and educational purposes only.*
