# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A standalone browser app (no server, no build step) for editing LISYclock `config.txt` files (compatible with firmware v1.43 / v2.43). The entire application is a single self-contained `LISYclock_config_editor.html` (~1095 lines). Open it directly in a browser — no local server needed.

## Development

No build tools, no package manager. Edit `LISYclock_config_editor.html` directly and refresh the browser to test.

**Testing manually:** Open `LISYclock_config_editor.html` in a Chromium-based browser (File System Access API requires Chrome/Edge for the Save/Open dialogs; other browsers fall back to `<input type=file>` / `<a download>`).

A reference `config.txt` (with all supported keys) is in the project root.

## Architecture

All logic lives in the `<script>` block of `LISYclock_config_editor.html`, organized in clearly labeled sections:

| Section | Lines | Purpose |
|---------|-------|---------|
| STATE | ~259–301 | `state` object + `resetState()` |
| CONFIG PARSER | ~307–398 | `parseConfig(text)` — reads config.txt into `state` |
| CONFIG GENERATOR | ~404–535 | `generateConfig()` — serializes `state` back to config.txt format |
| FILE I/O | ~541–648 | Open/Save/SaveAs/New using File System Access API with fallback |
| RENDER: TTS | ~668–707 | Populates TTS tab; wires input listeners |
| RENDER: GENERAL | ~713–799 | Populates General tab (FTP, timezone, weekday fields) |
| LED ROW HELPER | ~805–878 | `createLedRow(ledObj, onRemove)` — shared by GI LEDs and Attract tabs |
| RENDER: GI LEDs | ~884–900 | Renders `state.gi_leds` array |
| RENDER: ATTRACT MODE | ~906–997 | Renders Bootstrap accordion for AT1–AT5 groups |
| RENDER: EVENTS | ~1003–1071 | Renders `state.events` array |
| RENDER ALL | ~1077–1083 | `renderAll()` calls all render functions |
| INIT | ~1089–1091 | `resetState()` → `buildDaysFields()` → `renderAll()` |

## Data Flow

```
config.txt text  →  parseConfig()  →  state  →  renderAll()  →  DOM
user edits DOM   →  event listeners mutate state in-place
Save button      →  generateConfig()  →  state  →  config.txt text
```

Render functions are **destructive** (they do `innerHTML = ''` and rebuild from `state`). DOM input listeners mutate the relevant `state` property directly in-place — there is no two-way binding framework.

## Config File Format Quirks

- Lines starting with `#` are commented out (disabled), not deleted
- FTP block: no space after `#` → `#FTP_USER=lisy`
- Timezone: space after `#` → `# TIMEZONE="..."`
- Weekday values must be padded to exactly 6 chars (`pad6()` helper)
- `EVENT_TIME` value is an unquoted number; all other `EVENT_*` types use a quoted string
- Key names in the file use mixed case (e.g. `TTS_SFXChar`, `TTS_Voice`) but the parser normalizes to uppercase for matching
- `_unknown` field in state exists as a placeholder; unknown config lines are silently skipped (round-trip fidelity is not a goal for unknown keys)

## Koordination mit dem Partnerprojekt

Die Firmware befindet sich in `../LISYclock/` (relativ zu diesem Verzeichnis).

**API-Vertrag:** [`API.md`](API.md) ist die Referenzkopie der HTTP-API für diesen Editor. Die autoritative Kopie liegt in `../LISYclock/API.md`. Sync via `../sync_api.ps1`.

**Erwartete API-Version:** `2` (definiert als `EXPECTED_API_VERSION` in `LISYclock_config_editor.html`)

**Regeln:**
- Wenn du einen neuen Endpunkt im Editor nutzen möchtest, muss er zuerst in der Firmware (`../LISYclock/main/httpserver.c`) implementiert und in `../LISYclock/API.md` dokumentiert werden.
- Der Editor prüft beim Verbindungstest (`clockTestConnection`) ob `api_version` im `/status`-Response mit `EXPECTED_API_VERSION` übereinstimmt und zeigt eine Warnung bei Mismatch.
- Default-Port ist **8080** (Port 80 ist intern von wifi-manager belegt).

## State Structure

```js
state = {
  tts: { wit_token, voice, style, speed, pitch, gain, sfx_char, sfx_env },
  events: [{ type, time, value }],           // type = 'TTS'|'MP3'|'BATCH'|'DISPLAY'|'TIME'|'GI_LEDS'|'ATTRACT_LEDS'
  general: {
    disp_bright,
    ftp_enabled, ftp_user, ftp_pwd,
    timezone_enabled, timezone,
    days_enabled, days: { sun, mon, tue, wed, thu, fri, sat }
  },
  gi_leds: [{ led, r, g, b }],
  attract: [5x { blink_rate, rand_enabled, rand, leds: [{ led, r, g, b }] }],
  _fileHandle: null   // FileSystemFileHandle for Save (non-null after Open/SaveAs)
}
```
