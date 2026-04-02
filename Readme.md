> **This repository has been archived.**
> The LISYclock Config Editor has moved to the [LISYclock monorepo](https://github.com/bontango/LISYclock) under `config_editor/`.
> Please use that repository for all future development and issues.

---

# LISYclock Config Editor *(archived)*

A standalone browser application for creating and editing
[LISYclock](https://github.com/bontango/LISYclock) `config.txt` files.
No installation, no server — just open `LISYclock_config_editor.html` in Chrome or Edge.

## Requirements

- **Chrome or Edge** (Chromium-based) for full Open/Save support via the
  [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API)
  and USB connection support via the
  [Web Serial API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API).
- Other browsers fall back to a file-picker input and `<a download>` for saving; USB mode is not available.

## Quick Start

1. Open `LISYclock_config_editor.html` in Chrome or Edge.
2. Click **Open** to load an existing `config.txt`, or start with the defaults.
3. Edit the settings across the tabs.
4. Click **Save** (overwrites the open file) or **Save As…** to write to a new location.

A ready-to-use sample config is provided in `config.txt`.

## Tabs

| Tab | What you configure |
|-----|--------------------|
| **Clock** | Connect to the LISYclock via IP (WiFi) or USB; upload/download config; manage SD card files; set time; reboot |
| **General** | WiFi credentials, display brightness, FTP server, timezone, weekday strings |
| **Events** | Timed events (TTS, MP3, batch files, display/LED on-off, NTP sync) |
| **Attract Mode** | Five LED groups with blink rates and optional random activation |
| **GI LEDs** | Permanently-on LED colors (LED number + RGB) |
| **TTS Settings** | WIT.ai token, voice, style, speed/pitch/gain, SFX character & environment |
| **Update** | Firmware update via IP (local file or lisy.dev server); full USB flash (bootloader + firmware + partition table + OTA data) |
| **Debug** | Raw USB serial console for developers |

## Clock Connection Modes

The Clock tab supports two connection modes:

- **IP Mode** — connects to the clock over the local network using its IP address (HTTP on port 8080). Requires the HTML file to be opened via `file://`, not HTTPS.
- **USB Mode** — connects directly via USB cable using the Web Serial API (Chrome/Edge only). Enables WiFi credential testing and is required for USB flashing.

## Config File Format

- Lines starting with `#` are **commented out** (disabled), not deleted.
- WiFi: `WIFI_ENABLE=yes`, `WIFI_SSID="…"`, `WIFI_PWD="…"`
- FTP block: no space after `#` — `#FTP_USER=lisy`
- Timezone: space after `#` — `# TIMEZONE="CET-1CEST,..."`
- Weekday strings must be **exactly 6 characters** (padded with spaces if shorter).
- `EVENT_SYNC_TIME` value is an **unquoted number**; all other `EVENT_*` types use a **quoted string**.

See `config.txt` for a fully annotated example with all supported keys.

## Development

No build step. Edit `LISYclock_config_editor.html` and refresh the browser to test.

**Active development has moved to [github.com/bontango/LISYclock](https://github.com/bontango/LISYclock).**
