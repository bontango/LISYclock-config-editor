# LISYclock Config Editor – User Manual

**Version 1.07** | Language: English

---

## Table of Contents

1. [Overview](#1-overview)
2. [Requirements & Getting Started](#2-requirements--getting-started)
3. [File Operations](#3-file-operations)
4. [Tab: Clock](#4-tab-clock)
5. [Tab: General](#5-tab-general)
6. [Tab: Events](#6-tab-events)
7. [Tab: GI LEDs](#7-tab-gi-leds)
8. [Tab: Attract Mode](#8-tab-attract-mode)
9. [Tab: TTS Settings](#9-tab-tts-settings)
10. [config.txt – Format & Structure](#10-configtxt--format--structure)
11. [Tips & Notes](#11-tips--notes)

---

## 1. Overview

The **LISYclock Config Editor** is a tool for conveniently editing the `config.txt` configuration file of the **LISYclock**.

- Standalone application: a single `index.html` file, no server, no build step
- Simply open `index.html` in your browser and get started
- Supports direct upload/download to the LISYclock hardware via HTTP

![Full view of the editor with an open configuration file](pics/Full%20view%20of%20the%20editor%20with%20an%20open%20configuration%20file.jpg)

---

## 2. Requirements & Getting Started

### Browser

| Browser | Support |
|---------|---------|
| **Chrome / Edge** (recommended) | Full support including File System Access API (native open/save dialog) |
| Firefox / Safari | Functional, but uses `<input type=file>` / download link as fallback |

### Starting

1. Open `index.html` from the project directory **locally** in your browser (double-click or `File → Open`)
2. The application starts immediately with an empty default configuration

> **Important:** For direct communication with the LISYclock (tab "Clock"), the file must be opened via `file://` – **not** via HTTPS. Background: browsers block HTTP requests (to the clock) from HTTPS pages for security reasons.

---

## 3. File Operations

The buttons in the **navigation bar** (top) control all file operations:

| Button | Function |
|--------|----------|
| **Open** | Opens an existing `config.txt` from the local file system |
| **Save** | Writes the current configuration back to the opened file (only active when a file is open) |
| **Save As…** | Lets you choose a new target file and saves to it |
| **New Configuration** | Resets all fields to default values (caution: unsaved changes will be lost) |

The **current filename** is displayed in the center of the navigation bar. As long as no file has been opened, "(no file)" appears there.

---

## 4. Tab: Clock

This tab enables direct communication with the LISYclock hardware over the local network.

### IP Address

Enter the IP address of your LISYclock (e.g. `192.168.1.42`). The address is saved in the browser and automatically pre-filled the next time you open the editor.

### Connection Test

Clicking **Test Connection** checks whether the clock is reachable:

| Display | Meaning |
|---------|---------|
| Grey | No test performed yet |
| Green | Connection successful |
| Red | Connection failed |

On successful connection, the **firmware version** and **API version** are shown. If the API version does not match the expected version, a **warning** is displayed (functions may still be partially usable).

### Configuration

| Button | Function |
|--------|----------|
| **Upload Config** | Transfers the current (editor-displayed) configuration to the clock |
| **Download Config** | Downloads the `config.txt` from the clock and opens it in the editor |

### Reboot

Clicking **Reboot** restarts the LISYclock. A confirmation dialog prevents accidental restarts.

### Firmware Update

**Option A – Local file:**
1. Select a `.bin` file using the file selection button
2. Click **Upload & Reboot** – the file is transferred and the clock restarts automatically

**Option B – From lisy.dev server:**
1. Connect to the clock first (so the editor knows whether v1 or v2 firmware is running)
2. Click **Fetch versions** – a dropdown appears with all matching firmware files from lisy.dev
3. Select the desired version from the dropdown
4. Click **Upload & Reboot** – the file is downloaded from lisy.dev and transferred to the clock

> A progress indicator is shown during the upload. Do not close the browser tab.
> When selecting from the server, the server dropdown takes priority over a locally selected file.

### File Management on the SD Card

- **Upload file:** Transfer any file (e.g. MP3, batch) to the clock's SD card
- **File list:** Use **Refresh** to load the current file list from the SD card
  - The list shows name, size, and modification **date** for each file
  - Individual files can be **downloaded** or **renamed**

### HTTPS Warning

If the editor is opened via an HTTPS URL, the browser blocks all HTTP requests to the clock. Solution: open `index.html` **locally** via `file://`.

---

## 5. Tab: General

General settings for the LISYclock.

![General tab](pics/General%20tab.jpg)

### Display Brightness

Value from **0** (dark) to **7** (maximum brightness). Corresponds to the configuration key `DISP_BRIGHT`.

### FTP Server

| Field | Description |
|-------|-------------|
| Enabled (checkbox) | Enable/disable FTP server (`#FTP_USER=…` when disabled) |
| Username | FTP login name |
| Password | FTP password |

### Timezone

| Field | Description |
|-------|-------------|
| Enabled (checkbox) | Enable/disable timezone configuration |
| POSIX string | Timezone in POSIX format |

Example for Central Europe with daylight saving time:
```
CET-1CEST,M3.5.0,M10.5.0/3
```

### Weekday Strings

When enabled, the clock displays localized weekday names. Each of the 7 fields (Sunday–Saturday) may be **at most 6 characters** long. Shorter entries are automatically padded with spaces to 6 characters.

Example (English): `Sun`, `Mon`, `Tue`, `Wed`, `Thu`, `Fri`, `Sat`

---

## 6. Tab: Events

Events define time-controlled actions of the LISYclock.

![Events tab with multiple entries](pics/Events%20tab%20with%20multiple%20entries.jpg)

### Event Types

| Type | Description | Value Field |
|------|-------------|-------------|
| **TTS** | Output text via Text-to-Speech | Free text (e.g. "Good morning!") |
| **MP3** | Play an MP3 file from the SD card | Filename (e.g. `alarm.mp3`) |
| **BATCH** | Execute a batch file from the SD card | Filename (e.g. `script.bat`) |
| **DISPLAY** | Turn all displays on or off | Dropdown: `on` / `off` |
| **TIME** | Synchronize time via NTP | Dropdown: 1–10 retry attempts |
| **GI_LEDS** | Turn all GI LEDs on or off | Dropdown: `on` / `off` |
| **ATTRACT_LEDS** | Turn all Attract LEDs on or off | Dropdown: `on` / `off` |

### Time Format

The **time** of an event is specified as `HH:MM:W` or `HH:MM-DD.MM.YYYY`. `*` stands for "any" in each field.

#### Weekday Mode: `HH:MM:W`

| Field | Meaning |
|-------|---------|
| `HH` | Hour (0–23), `*` = every hour |
| `MM` | Minute (0–59), `*` = every minute |
| `W` | Weekday: 0 = Sunday, 1 = Monday, … 6 = Saturday, `*` = daily |

#### Date Mode: `HH:MM-TT.MM.JJJJ`

Active when `*` (date) is selected in the weekday field. The date field then appears additionally.

| Field | Meaning |
|-------|---------|
| `TT` | Day (1–31), `*` = every day |
| `MM` | Month (1–12), `*` = every month |
| `JJJJ` | Year (four digits), `*` = every year |

#### Examples

| Time | Meaning |
|------|---------|
| `7:30:1` | Every Monday at 07:30 |
| `*:*:*` | Every minute, every day |
| `8:0:*` | Daily at 08:00 |
| `12:0-24.12.*` | Every December 24th at 12:00 |
| `*:0-*.*.*` | Every full hour, every day |

### Controls

- **+ Add Event** (bottom button): Adds a new event entry at the end of the list
- **✕** (to the right of an event): Removes that event

---

## 7. Tab: GI LEDs

Defines the start colors of the GI LEDs (General Illumination).

![GI LEDs tab](pics/GI%20LEDs%20tab.jpg)

### Fields per LED Row

| Field | Description |
|-------|-------------|
| LED | LED number (1–64) |
| R / G / B | Color value per channel (0–255) |
| Color picker | Opens a native color picker |
| Preview dot | Shows the current color as a small colored circle |

The color picker and RGB fields are synchronized: a change in the picker updates the RGB fields and vice versa.

### Controls

- **+ Add LED**: Append a new LED entry at the end of the list
- **✕**: Remove this LED entry

---

## 8. Tab: Attract Mode

Configures the Attract Mode lighting (running lights in standby) in **5 groups** (AT1–AT5).

![Attract Mode tab with group AT1 expanded](pics/Attract%20Mode%20tab%20with%20group%20AT1%20expanded.jpg)

Each group is displayed as a **collapsible accordion panel**.

### Settings per Group

| Field | Description |
|-------|-------------|
| **Blink rate** | Blink interval in milliseconds |
| **Random activation** | Checkbox: activate group randomly |
| **Rand** | Random threshold (0–255); the higher the value, the less frequently the group is activated |
| **LED list** | List of LEDs in this group (like GI LEDs: number, R, G, B, color picker) |

- **+ Add LED**: Add a new LED entry to the group
- **✕**: Remove an LED entry from the group

---

## 9. Tab: TTS Settings

Settings for the Text-to-Speech function (uses the WIT.ai service).

![TTS Settings tab](pics/TTS%20Settings%20tab.jpg)

| Field | Possible Values | Default |
|-------|----------------|---------|
| **WIT.ai Token** | Free text (API key from wit.ai) | — |
| **Voice** | 17 voices (US EN, UK EN, CA EN), e.g. `wit$Cooper` | `wit$Cooper` |
| **Style** | `default` / `soft` / `formal` / `fast` / `projected` | `default` |
| **Speed** | 0–200 | 80 |
| **Pitch** | 0–200 | 80 |
| **Gain** | 0–100 | 30 |
| **SFX Char** | `none` / `chipmunk` / `monster` / `daemon` / `robot` / `alien` | `none` |
| **SFX Env** | `none` / `reverb` / `room` / `cathedral` / `radio` / `phone` | `none` |

> You obtain the WIT.ai API token after free registration at [wit.ai](https://wit.ai).

---

## 10. config.txt – Format & Structure

### Basic Format

```
KEY=value
```

- Each line contains a key and a value, separated by `=`
- A `#` at the **start of a line** disables the line (without deleting it)
- Capitalization of key names is normalized in the editor

### Special Rules

| Rule | Description |
|------|-------------|
| FTP block | No space after `#`: `#FTP_USER=lisy` |
| Timezone | Space after `#`: `# TIMEZONE="CET-1CEST,…"` |
| Weekday strings | Exactly 6 characters, padded with spaces: `Mon   ` |
| `EVENT_TIME` | Value **without** quotes: `EVENT_TIME=730` |
| All other `EVENT_*` | Value **with** quotes: `EVENT_TYPE="TTS"` |

### Example Excerpt

```
DISP_BRIGHT=5
#FTP_USER=lisy
#FTP_PWD=lisy
# TIMEZONE="CET-1CEST,M3.5.0,M10.5.0/3"
DAYS_SUN=So
DAYS_MON=Mo
EVENT_TIME=730
EVENT_TYPE="TTS"
EVENT_VALUE="Guten Morgen!"
```

---

## 11. Tips & Notes

- **Use Chrome or Edge:** Only these browsers support the File System Access API for native open/save dialogs. In Firefox and Safari, standard file pickers or download links appear instead.

- **Open locally for Clock access:** If communication with the LISYclock does not work, check whether `index.html` is opened via `file://` (locally) and not via `https://`.

- **FTP as alternative:** The finished `config.txt` can also be transferred to the clock's SD card via FTP (if the FTP server is enabled in the configuration).

- **Unknown keys:** The editor ignores unknown keys when loading – they are not retained when saving. If you have custom extensions in `config.txt`, back them up separately beforehand.

- **No undo function:** The editor has no undo function. If you make accidental changes, reopening the file (without saving first) will restore the previous state.

---

*Created for LISYclock Config Editor v1.07*
