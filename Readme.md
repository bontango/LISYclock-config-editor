# LISYclock Config Editor

A standalone browser application for creating and editing
[LISYclock](https://github.com/lisy) `config.txt` files.
No installation, no server — just open `LISYclock_config_editor.html` in Chrome or Edge.

## Requirements

- **Chrome or Edge** (Chromium-based) for full Open/Save support via the
  [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API).
- Other browsers fall back to a file-picker input and `<a download>` for saving.

## Quick Start

1. Open `LISYclock_config_editor.html` in Chrome or Edge.
2. Click **Open** to load an existing `config.txt`, or start with the defaults.
3. Edit the settings across the five tabs.
4. Click **Save** (overwrites the open file) or **Save As…** to write to a new location.

A ready-to-use sample config is provided in `config.txt`.

## Tabs

| Tab | What you configure |
|-----|--------------------|
| **General** | Display brightness, FTP server, timezone, weekday strings |
| **Events** | Timed events (TTS, MP3, batch files, display/LED on-off, NTP sync) |
| **Attract Mode** | Five LED groups with blink rates and optional random activation |
| **GI LEDs** | Permanently-on LED colors (LED number + RGB) |
| **TTS Settings** | WIT.ai token, voice, style, speed/pitch/gain, SFX character & environment |

## Config File Format

- Lines starting with `#` are **commented out** (disabled), not deleted.
- FTP block: no space after `#` — `#FTP_USER=lisy`
- Timezone: space after `#` — `# TIMEZONE="CET-1CEST,..."`
- Weekday strings must be **exactly 6 characters** (padded with spaces if shorter).
- `EVENT_TIME` value is an **unquoted number**; all other `EVENT_*` types use a **quoted string**.

See `config.txt` for a fully annotated example with all supported keys.

## Project Structure

```
LISYclock_config_editor.html   — complete application (single self-contained file, ~1150 lines)
config.txt   — reference / sample configuration
CLAUDE.md    — developer notes for Claude Code
```

## Development

No build step. Edit `LISYclock_config_editor.html` and refresh the browser to test.
