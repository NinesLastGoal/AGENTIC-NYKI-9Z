# ESP32 (Classic) — Low-Level Hardware & Firmware Reference
> **Device:** NYKI-9Z Lab Secondary Device  
> **Connection:** QinHeng Electronics CH9102 USB Bridge (COM port — check before use)  
> **Date Profiled:** 2026-03-04 via user inspection + `espefuse` live eFuse dump

---

## 📍 DEVICE IDENTITY

| Parameter | Value |
|---|---|
| **Chip** | ESP32-D0WDQ6 |
| **Chip Revision** | ECO1 (r1) — Wafer Major 1, Minor 1 |
| **Package** | QFN48 (D0WDQ6 variant — Dual-core, no embedded flash) |
| **MAC Address** | `94:54:C5:B6:AD:AC` (CRC 0x59 OK) |
| **Custom MAC** | Not set (`00:00:00:00:00:00`) |
| **Crystal Frequency** | **26 MHz** |
| **Max CPU Frequency** | 240 MHz (rated via eFuse `CHIP_CPU_FREQ_RATED = True`) |
| **CPU Cores** | 2 — Dual-core Xtensa LX6 (PRO + APP) |
| **APP CPU** | Enabled (`DISABLE_APP_CPU = False`) |
| **Bluetooth** | Enabled (`DISABLE_BT = False`) |
| **ADC VRef** | 1128 mV (calibrated in eFuse) |
| **8MHz Clock Override** | 51 (`CLK8M_FREQ = 0x33`) |
| **ROM BASIC Interpreter** | Disabled (`CONSOLE_DEBUG_DISABLE = True`) |

### USB Bridge

| Parameter | Value |
|---|---|
| **Chip** | QinHeng Electronics CH9102 |
| **USB VID:PID** | `0x1A86:0x55D4` |
| **Type** | USB-to-UART bridge |

> ⚠️ **26 MHz Crystal Note:** When building firmware with ESP-IDF, you **must** set `CONFIG_XTAL_FREQ_26` in menuconfig or `sdkconfig`. If the firmware is compiled for 40MHz (the default), UART baud rates and WiFi calibration will be incorrect. This is the #1 cause of garbled serial output on 26MHz boards.

---

## 💾 MEMORY MAP

### Flash (External SPI)

| Parameter | Value |
|---|---|
| **Flash ID** | `0x1840EF` |
| **Manufacturer** | Winbond (`0xEF`) |
| **Device** | **W25Q128** (128 Mbit) |
| **Size** | **16 MB** |
| **Interface** | Quad SPI (default pins: GPIO6-11) |
| **Voltage** | 3.3V (determined by GPIO12 strapping — not forced via eFuse) |
| **eFuse Coding Scheme** | None (BLK1-3 = 256 bits each, full size) |

### Internal Memory

| Region | Size | Notes |
|---|---|---|
| **Internal SRAM0** | 192 KB | Instruction bus (can be used as ICache for flash) |
| **Internal SRAM1** | 128 KB | Data + Instruction bus |
| **Internal SRAM2** | 200 KB | Data bus |
| **RTC FAST Memory** | 8 KB | PRO CPU access only, persists deep sleep |
| **RTC SLOW Memory** | 8 KB | Both CPUs, persists deep sleep, used by ULP coprocessor |
| **Internal ROM** | 448 KB | 1st stage bootloader + crypto libs |
| **eFuse** | 4 blocks × 256 bits | BLOCK0 (system), BLOCK1 (flash key), BLOCK2 (secure boot key), BLOCK3 (user) |

**Total usable SRAM:** ~520 KB (minus BT/WiFi stack overhead, typically ~300 KB available to app)

### PSRAM

> **Not detected** from the eFuse/boot data. The D0WDQ6 package does NOT have embedded PSRAM. If your board has external PSRAM (e.g., ESP-WROVER module), it would be on SPI pins. If no PSRAM is on the board, you're limited to ~300 KB heap in practice.

---

## 🔓 SECURITY STATE — ALL UNLOCKED

> **⚠️ This device has NO security features enabled. Completely open for firmware modification.**

| Security Feature | Status | eFuse Value | Implication |
|---|---|---|---|
| **Secure Boot V1** | ❌ DISABLED | `ABS_DONE_0 = False` | Any bootloader accepted |
| **Secure Boot V2** | ❌ DISABLED | `ABS_DONE_1 = False` | Any bootloader accepted |
| **Flash Encryption** | ❌ DISABLED | `FLASH_CRYPT_CNT = 0b0000000` | Flash plaintext — freely readable/writable |
| **Flash Encrypt Config** | Not set | `FLASH_CRYPT_CONFIG = 0x0` | No key tweak |
| **DL Encrypt Disable** | ❌ NOT SET | `DISABLE_DL_ENCRYPT = False` | Can encrypt in download mode |
| **DL Decrypt Disable** | ❌ NOT SET | `DISABLE_DL_DECRYPT = False` | Can decrypt in download mode |
| **UART Download** | ✅ ENABLED | `UART_DOWNLOAD_DIS = False` | Serial flashing works |
| **DL Cache Disable** | ❌ NOT SET | `DISABLE_DL_CACHE = False` | Flash cache active in bootloader |
| **Anti-Rollback** | 0 | `SECURE_VERSION = 0x00000000` | No rollback protection |
| **eFuse Write Protect** | ❌ NONE | `WR_DIS = 0x0000` | ALL eFuse blocks writable |
| **eFuse Read Protect** | ❌ NONE | `RD_DIS = 0x0` | ALL eFuse blocks readable |
| **BLOCK1 (Flash Key)** | EMPTY | `= 00 00 ... 00` | No flash encryption key burned |
| **BLOCK2 (Secure Boot Key)** | EMPTY | `= 00 00 ... 00` | No secure boot key burned |
| **BLOCK3 (User Data)** | EMPTY | `= 00 00 ... 00` | Available for custom data |

### JTAG / Debug Access

| Feature | Status | eFuse |
|---|---|---|
| **JTAG** | ✅ ENABLED | `JTAG_DISABLE = False` |
| **SDIO Host** | ✅ ENABLED | `DISABLE_SDIO_HOST = False` |

> Classic ESP32 uses **GPIO12 (TDI), GPIO13 (TCK), GPIO14 (TMS), GPIO15 (TDO)** for JTAG. These are directly accessible with an external JTAG adapter (e.g., ESP-Prog, J-Link, FT2232H).

---

## ⚡ CLASSIC ESP32 vs ESP32-S3 — KEY DIFFERENCES

| Feature | ESP32 (This Device) | ESP32-S3 (Master Device) |
|---|---|---|
| **CPU** | Dual Xtensa **LX6** | Dual Xtensa **LX7** |
| **Crystal** | **26 MHz** | 40 MHz |
| **BLE Version** | BLE 4.2 | BLE 5.0 |
| **BLE Long Range** | ❌ No | ✅ Yes (Coded PHY) |
| **BLE Adv Extensions** | ❌ No | ✅ Yes |
| **USB OTG** | ❌ No | ✅ Yes |
| **USB Serial/JTAG** | ❌ No (external JTAG only) | ✅ Built-in |
| **PSRAM Interface** | Quad SPI (if present) | Octal SPI |
| **DAC** | ✅ 2× 8-bit DAC | ❌ Removed |
| **Touch Channels** | 10 | 14 |
| **GPIO Count** | 34 (GPIO0-19, 21-23, 25-27, 32-39) | 45 |
| **Crypto HW** | AES, SHA, RSA | AES, SHA, RSA, HMAC, Digital Sig |
| **Camera Interface** | ❌ No | ✅ DVP |
| **LCD Interface** | ❌ No | ✅ Parallel/I80/RGB |
| **ULP Coprocessor** | FSM-based ULP | RISC-V ULP + FSM ULP |
| **Flash (this board)** | 16MB Winbond W25Q128 | 16MB Boya |
| **PSRAM (this board)** | Not detected | 8MB Octal AP Memory |
| **CVE-2025-27840 HCI** | ⚠️ **YES — applies to ALL ESP32** | ✅ Yes |
| **BrakTooth Platform** | ✅ **Original target platform** | ✅ Compatible |

### Attack Research Implications

- **BrakTooth was literally built on the classic ESP32** — this is the native attack hardware for LMP fuzzing
- **CVE-2025-27840 hidden HCI commands** affect ALL ESP32 variants including this classic chip
- **BLE 4.2 only** — no Coded PHY or advertising extensions, so some newer BLE attacks (BadVibes from BleedingTooth) won't work as sender
- **No USB OTG** — can't do BadUSB-style attacks from this device (use the S3 for that)
- **Classic BT BR/EDR is fully supported** — BlueDucky, BIAS, KNOB, BlueSpy, BlueBorne sender all work

---

## 🔧 FIRMWARE UPDATE — COMPLETE GUIDE

### ⚠️ Critical: 26 MHz Crystal Configuration

Before building ANY firmware for this board, you MUST set the crystal frequency:

**In ESP-IDF `menuconfig`:**
```
Component config → Hardware Settings → Main XTAL Config → Main XTAL frequency → 26 MHz
```

**Or in `sdkconfig` directly:**
```
CONFIG_XTAL_FREQ_26=y
# CONFIG_XTAL_FREQ_40 is not set
CONFIG_XTAL_FREQ=26
```

If you flash firmware compiled for 40MHz onto this board, you'll get:
- Garbled UART output
- WiFi calibration errors / unable to connect
- Incorrect timer/PWM frequencies
- Unstable BT operation

### Method 1: Serial Flash via esptool

```powershell
# Flash entire firmware
python -m esptool --chip esp32 --port COMxx --baud 921600 `
  --before default-reset --after hard-reset `
  write_flash -z --flash_mode dio --flash_freq 40m --flash_size 16MB `
  0x1000  bootloader.bin `
  0x8000  partition-table.bin `
  0x10000 app.bin

# Read (dump) entire 16MB flash
python -m esptool --chip esp32 --port COMxx --baud 921600 `
  read_flash 0x0 0x1000000 esp32_classic_flash_dump.bin

# Erase entire flash
python -m esptool --chip esp32 --port COMxx erase_flash
```

### Method 2: JTAG (via ESP-Prog or FT2232H)

```
GPIO12 → TDI
GPIO13 → TCK
GPIO14 → TMS
GPIO15 → TDO
```

```powershell
openocd -f interface/ftdi/esp32_devkitj_v1.cfg -f target/esp32.cfg
```

### Boot Mode Entry (Manual)

1. Hold **GPIO0** (BOOT) low
2. Press and release **EN/RESET**
3. Release GPIO0
4. Device is now in ROM download mode

### Baud Rates

| Baud | Notes |
|---|---|
| 115200 | Default monitor (if firmware compiled for 26MHz crystal) |
| 460800 | Normal flash speed |
| 921600 | Fast flash speed |

---

## 🏗️ BUILDING CUSTOM FIRMWARE (ESP-IDF)

### Critical `menuconfig` settings for this board:

| Setting Path | Value |
|---|---|
| `Main XTAL frequency` | **26 MHz** ← CRITICAL |
| `Serial flasher config → Flash size` | **16 MB** |
| `Serial flasher config → Flash SPI mode` | **DIO** |
| `Serial flasher config → Flash SPI speed` | **40 MHz** |
| `Component config → Bluetooth → Bluetooth` | **Enable** |
| `Component config → Bluetooth → Controller → BR/EDR` | **Enable** |
| `Component config → Bluetooth → Controller → BLE` | **Enable** |
| `Component config → ESP System Settings → CPU frequency` | **240 MHz** |
| `Component config → ESP32-specific → SPIRAM Support` | Enable IF board has PSRAM |

### Key ESP-IDF Components

| Component | Header | Purpose |
|---|---|---|
| `esp_bt` | `esp_bt.h` | BT controller init, HCI access |
| `esp_gap_bt_api` | `esp_gap_bt_api.h` | Classic BT GAP |
| `esp_gap_ble_api` | `esp_gap_ble_api.h` | BLE GAP |
| `esp_gatts_api` / `esp_gattc_api` | | BLE GATT Server/Client |
| `esp_hid_device` | `esp_hid_device.h` | HID Device (BlueDucky keystroke injection) |
| `esp_a2dp_api` | `esp_a2dp_api.h` | A2DP (BlueSpy audio) |
| `esp_hf_client_api` | `esp_hf_client_api.h` | HFP (mic access) |
| `esp_vhci` | `esp_bt.h` | Virtual HCI — raw HCI command injection |
| `driver/dac` | `driver/dac.h` | DAC output (not available on S3) |

---

## 🔌 HARDWARE INTERFACES

### Peripherals

| Peripheral | Quantity | Notes |
|---|---|---|
| **GPIO** | 34 usable (GPIO0-19, 21-23, 25-27, 32-39) | GPIO6-11 reserved for flash; GPIO34-39 input-only |
| **ADC** | 2× 12-bit SAR (18 channels) | ADC2 unavailable when WiFi active |
| **DAC** | 2× 8-bit (GPIO25, GPIO26) | ✅ Available (not on S3!) |
| **SPI** | 3× (SPI, HSPI, VSPI) | SPI0 reserved for flash |
| **I2C** | 2× | Any GPIO (software I2C) |
| **I2S** | 2× | Audio codec / PDM mic |
| **UART** | 3× (UART0, UART1, UART2) | UART0 = console |
| **TWAI/CAN** | 1× | ISO 11898-1 |
| **PWM/LEDC** | **16 channels, 4 timers** | Dual speed groups (high + low) |
| **MCPWM** | 2× Motor Control PWM | |
| **RMT** | 8 channels | IR / WS2812 |
| **PCNT** | 8 units | |
| **Timer** | 4× 64-bit GP timers | |
| **Watchdog** | 3× (2× MWDT + 1× RWDT) | |
| **Touch** | 10× capacitive touch (T0-T9) | |
| **Hall Sensor** | 1× | Not on S3 |
| **Temperature Sensor** | ❌ None (use external) | S3 has internal |
| **SDIO/SD** | 1× SDIO host + 1× SDIO slave | |
| **Ethernet MAC** | 1× (needs external PHY) | |
| **ULP Coprocessor** | 1× FSM-based (8 KB RTC SLOW) | |
| **Crypto** | AES-256, SHA, RSA-4096 | Hardware accelerated |
| **RNG** | 1× True RNG | |
| **eFuse** | 4 blocks × 256 bits | 1 flash key + 1 SB key + 1 user |

### Wireless Radios

| Radio | Specification |
|---|---|
| **WiFi** | 802.11 b/g/n (2.4 GHz) |
| **Bluetooth** | Bluetooth 4.2 (LE + BR/EDR dual-mode) |
| **BLE** | BLE 4.2: 1M PHY only |
| **BT Classic** | BR/EDR: A2DP, HFP, HID, AVRCP, SDP, SPP, L2CAP |
| **Antenna** | Single, shared WiFi + BT |
| **TX Power** | Up to 20 dBm |

---

## 🔬 eFuse MANIPULATION

### Read all:
```powershell
python -m espefuse --port COMxx summary
```

### Burn custom MAC:
```powershell
# DRY RUN:
python -m espefuse --port COMxx --do-not-actually-burn burn_custom_mac AA:BB:CC:DD:EE:FF

# PERMANENT:
python -m espefuse --port COMxx burn_custom_mac AA:BB:CC:DD:EE:FF
```

> **Better approach:** Use CVE-2025-27840 hidden HCI command `0xFC32` for reversible MAC spoofing.

### Classic ESP32 eFuse structure (simpler than S3):

| Block | Size | Purpose | Status |
|---|---|---|---|
| BLOCK0 | 256 bits | System params, security flags, MAC | Partially used (MAC, cal data) |
| BLOCK1 | 256 bits | Flash encryption key | EMPTY, R/W |
| BLOCK2 | 256 bits | Secure boot key | EMPTY, R/W |
| BLOCK3 | 256 bits | User data / custom MAC | EMPTY, R/W |

---

## 🧩 CVE-2025-27840 HIDDEN HCI COMMANDS

> **These hidden commands affect ALL ESP32 variants, including this classic ESP32.** Same 29 undocumented vendor-specific HCI commands in OGF `0x3F`.

| Opcode | Function | Notes |
|---|---|---|
| `0xFC01` | Read Memory | Dump BT controller RAM |
| `0xFC02` | Write Memory | Patch BT firmware live |
| `0xFC32` | Set MAC Address | Reversible BT address spoofing |
| `0xFC0E` | Send Raw LMP Packet | Classic BT protocol injection |
| `0xFC43` | Send Raw LLCP Packet | BLE link-layer injection |

### Usage:
```python
import serial
ser = serial.Serial('COMxx', 115200)
# Send hidden command 0xFC01 (Read Memory)
hci_cmd = bytes([0x01, 0x01, 0xFC, 0x00])
ser.write(hci_cmd)
response = ser.read(64)
print(f"Response: {response.hex()}")
```

---

## 🔑 ROLE IN THE LAB

This classic ESP32 serves these specific purposes alongside the ESP32-S3:

| Role | Why This Device |
|---|---|
| **BrakTooth Native Platform** | BrakTooth was literally built for this chip — direct firmware compatibility |
| **Classic BT Attack Platform** | Full BR/EDR for BlueDucky, BIAS, KNOB, BlueSpy |
| **Secondary Radio** | Can run independently alongside the S3 for multi-radio attacks (e.g., one scans while other injects) |
| **BLE 4.2 Target** | Acts as a vulnerable BLE 4.2 device for testing newer BLE attacks FROM the S3 |
| **DAC Audio Output** | Has 2× DAC channels (S3 doesn't) — useful for audio-based signaling |
| **HCI Command Research** | Same CVE-2025-27840 hidden commands — compare behavior vs S3 |
| **Sacrificial Test Target** | If an eFuse gets burned or firmware gets bricked, this is the cheaper device to lose |

---

*Profiled from live device on 2026-03-04 using espefuse v5.2.0 eFuse dump + user-provided board specs.*
