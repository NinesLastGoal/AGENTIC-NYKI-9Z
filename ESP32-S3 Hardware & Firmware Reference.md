# ESP32-S3 Hardware & Firmware Reference

> **Device read via `esptool v5.2.0` on COM8 — 2026-03-05**

---

## 1. Chip Identification

| Field | Value |
|---|---|
| **Chip Type** | ESP32-S3 (QFN56) |
| **Silicon Revision** | v0.2 |
| **Features** | Wi-Fi, Bluetooth 5 (LE), Dual Core + LP Core, 240 MHz, Embedded Flash |
| **MAC Address** | `30:ED:A0:B7:5A:80` |
| **Custom MAC** | Not set (`00:00:00:00:00:00`) |
| **USB Mode** | USB-Serial/JTAG (built-in) |
| **Serial Port** | COM8 |

---

## 2. Flash Memory

| Field | Value |
|---|---|
| **Manufacturer ID** | `0x68` |
| **Device ID** | `0x4018` |
| **Detected Size** | **16 MB** (128 Mbit) |
| **Flash Type (eFuse)** | Quad I/O (4 data lines) |
| **Flash Voltage (eFuse)** | 3.3V |
| **Flash Status Register** | `0x600200` |

---

## 3. Partition Table

Read from flash at offset `0x8000`:

| Name | Type | SubType | Offset | Size |
|---|---|---|---|---|
| `nvs` | data | nvs | `0x00009000` | 24 KB |
| `phy_init` | data | phy | `0x0000F000` | 4 KB |
| `factory` | app | factory | `0x00010000` | 4,032 KB (~3.9 MB) |
| `spiffs` | data | spiffs | `0x00400000` | 12,288 KB (12 MB) |

### Partition Layout Notes

- **Single app slot** — no OTA partitions present. Firmware updates require serial flash.
- **Factory app** occupies ~3.9 MB starting at `0x10000`.
- **SPIFFS** partition is large (12 MB) — used for file storage (web assets, config, logs, etc.).
- **NVS** (Non-Volatile Storage) is 24 KB — stores key-value pairs (Wi-Fi credentials, calibration, settings).
- **PHY init** is 4 KB — RF calibration data for Wi-Fi/BT.

---

## 4. Security & Boot Configuration (eFuse)

All security features are **disabled / not configured**. The chip is fully open for development.

| eFuse | Value | Meaning |
|---|---|---|
| **Secure Boot** | `False` | Not enabled |
| **Flash Encryption** | `Disable` (SPI_BOOT_CRYPT_CNT = `0b000`) | Not enabled |
| **Secure Boot Key Revoke 0-2** | `False` | No keys revoked |
| **Secure Version** | `0` | Anti-rollback not configured |
| **Download Mode Disabled** | `False` | Serial download mode available |
| **Force Download Disabled** | `False` | Force-download via GPIO still works |
| **Manual Encrypt in Download** | `False` | Not disabled |
| **Secure UART Download** | `False` | Not enabled |

> [!TIP]
> This chip has no security lockdowns applied — you can freely flash, erase, and debug via serial.

---

## 5. JTAG & Debug Configuration (eFuse)

| eFuse | Value | Meaning |
|---|---|---|
| **DIS_USB_JTAG** | `False` | USB-to-JTAG switching is enabled |
| **DIS_PAD_JTAG** | `False` | JTAG via GPIO pads is enabled |
| **DIS_USB_SERIAL_JTAG** | `False` | USB Serial/JTAG device is enabled |
| **STRAP_JTAG_SEL** | `False` | GPIO3 strapping for JTAG source selection not enabled |
| **USB_PHY_SEL** | Internal PHY → USB Device; External PHY → USB OTG | Default configuration |

> [!NOTE]
> JTAG is fully accessible via both USB and GPIO pads. No debug fuses have been burned.

---

## 6. USB Configuration (eFuse)

| eFuse | Value | Meaning |
|---|---|---|
| **DIS_USB_OTG** | `False` | USB OTG function available |
| **USB_EXCHG_PINS** | `False` | D+/D- not swapped |
| **USB_EXT_PHY_ENABLE** | `False` | External USB PHY not used |
| **DIS_USB_SERIAL_JTAG_ROM_PRINT** | `Enable` | ROM boot messages print over USB |
| **DIS_USB_SERIAL_JTAG_DOWNLOAD_MODE** | `False` | UART download via USB is allowed |
| **DIS_USB_OTG_DOWNLOAD_MODE** | `False` | Download via USB OTG is allowed |

---

## 7. Power Configuration (eFuse)

| eFuse | Value | Meaning |
|---|---|---|
| **VDD_SPI_XPD** | `True` | SPI flash regulator is powered up |
| **VDD_SPI_TIEH** | `True` (VDD3P3_RTC_IO) | VDD_SPI connected to 3.3V rail |
| **VDD_SPI_FORCE** | `True` | eFuse configuration forced for VDD_SPI |

Flash voltage is hard-set to **3.3V** via eFuse.

---

## 8. Miscellaneous eFuse Settings

| eFuse | Value | Meaning |
|---|---|---|
| **WDT_DELAY_SEL** | 40000 | RTC watchdog timeout threshold (slow clock cycles) |
| **DIS_DOWNLOAD_ICACHE** | `False` | I-Cache available in download mode |
| **DIS_DOWNLOAD_DCACHE** | `False` | D-Cache available in download mode |
| **SPI Pad Config** | All `0` | Default SPI pin mapping (no custom routing) |
| **Key Blocks (0-5)** | All zeroed, Purpose = `USER` | No encryption/secure boot keys burned |

---

## 9. Quick Reference for Firmware Development

### Essential esptool Commands

```bash
# Read chip info
python -m esptool --port COM8 chip-id

# Read flash details
python -m esptool --port COM8 flash-id

# Read MAC address
python -m esptool --port COM8 read-mac

# Erase entire flash
python -m esptool --port COM8 erase-flash

# Flash firmware (typical ESP-IDF build)
python -m esptool --port COM8 --baud 460800 write-flash -z 0x0 bootloader.bin 0x8000 partition-table.bin 0x10000 firmware.bin

# Read partition table from flash
python -m esptool --port COM8 read-flash 0x8000 0xC00 partition_table.bin

# Dump full eFuse state
python -m espefuse --port COM8 summary
```

### Key Addresses

| Region | Address | Notes |
|---|---|---|
| Bootloader | `0x0000` | Second-stage bootloader |
| Partition Table | `0x8000` | 3 KB max |
| App (factory) | `0x10000` | Main application firmware |
| SPIFFS | `0x400000` | Filesystem storage |

### Development Constraints

- **No OTA** — single factory partition; updates require USB serial flashing
- **No Secure Boot / Flash Encryption** — chip is wide open, no restrictions
- **16 MB flash** — ~3.9 MB for app code, ~12 MB for SPIFFS, plenty of room
- **240 MHz dual-core** Xtensa LX7 with low-power core available
- **USB-Serial/JTAG** built-in — no external UART adapter needed (use the correct USB-C port)
