# LISYclock Config Editor – Bedienungsanleitung

**Version 1.11** | Sprache: Deutsch

---

## Inhaltsverzeichnis

1. [Überblick](#1-überblick)
2. [Voraussetzungen & Start](#2-voraussetzungen--start)
3. [Dateioperationen](#3-dateioperationen)
4. [Tab: Clock](#4-tab-clock)
5. [Tab: General](#5-tab-general)
6. [Tab: Events](#6-tab-events)
7. [Tab: GI LEDs](#7-tab-gi-leds)
8. [Tab: Attract Mode](#8-tab-attract-mode)
9. [Tab: TTS Settings](#9-tab-tts-settings)
10. [Tab: Update](#10-tab-update)
11. [Tab: Debug](#11-tab-debug)
12. [config.txt – Format & Aufbau](#12-configtxt--format--aufbau)
13. [Tipps & Hinweise](#13-tipps--hinweise)

---

## 1. Überblick

Der **LISYclock Config Editor** ist ein Werkzeug zum komfortablen Bearbeiten der `config.txt`-Konfigurationsdatei des **LISYclock**.

- Standalone-Anwendung: eine einzige `LISYclock_config_editor.html`-Datei, kein Server, kein Build-Schritt
- Einfach `LISYclock_config_editor.html` im Browser öffnen und loslegen
- Unterstützt direkten Upload/Download zur LISYclock-Hardware über IP (HTTP) oder USB

![Gesamtansicht des Editors mit geöffneter Konfigurationsdatei](pics/Full%20view%20of%20the%20editor%20with%20an%20open%20configuration%20file.jpg)

---

## 2. Voraussetzungen & Start

### Browser

| Browser | Unterstützung |
|---------|---------------|
| **Chrome / Edge** (empfohlen) | Volle Unterstützung inkl. File System Access API (nativer Öffnen/Speichern-Dialog) und Web Serial API (USB-Verbindung) |
| Firefox / Safari | Für Datei-Bearbeitung nutzbar, nutzt jedoch `<input type=file>` / Download-Link als Fallback; USB-Modus nicht unterstützt |

### Starten

1. `LISYclock_config_editor.html` aus dem Projektverzeichnis **lokal** im Browser öffnen (Doppelklick oder `Datei → Öffnen`)
2. Die Anwendung startet sofort mit einer leeren Standardkonfiguration

> **Wichtig:** Für die direkte Kommunikation mit der LISYclock über IP (Tab „Clock") muss die Datei über `file://` geöffnet werden – **nicht** über HTTPS. Browser blockieren HTTP-Anfragen von HTTPS-Seiten aus Sicherheitsgründen. Der USB-Modus funktioniert unabhängig davon, wie die Datei geöffnet wurde.

---

## 3. Dateioperationen

Die Schaltflächen in der **Navigationsleiste** (oben) steuern alle Dateioperationen:

| Schaltfläche | Funktion |
|---|---|
| **Öffnen** | Öffnet eine vorhandene `config.txt` vom lokalen Dateisystem |
| **Speichern** | Schreibt die aktuelle Konfiguration in die geöffnete Datei zurück (nur aktiv, wenn eine Datei geöffnet ist) |
| **Speichern unter…** | Lässt eine neue Zieldatei wählen und speichert darin |
| **Neue Konfiguration** | Setzt alle Felder auf Standardwerte zurück (Achtung: ungespeicherte Änderungen gehen verloren) |

Der **aktuelle Dateiname** wird in der Mitte der Navigationsleiste angezeigt. Solange keine Datei geöffnet wurde, erscheint dort „— keine Datei geöffnet —".

---

## 4. Tab: Clock

Dieser Tab ermöglicht die direkte Kommunikation mit der LISYclock-Hardware. Wähle zwischen **IP-Modus** (WLAN) und **USB-Modus** (USB-Kabel).

### Verbindungsmodus

Mit den Radiobuttons oben im Tab den Verbindungstyp auswählen:

| Modus | Beschreibung |
|-------|-------------|
| **IP-Modus** | Verbindung zur Uhr über das lokale Netzwerk per IP-Adresse (erfordert WLAN) |
| **USB-Modus** | Direkte Verbindung zur Uhr per USB-Kabel (Web Serial API, nur Chrome/Edge) |

### IP-Modus

Trage die IP-Adresse deiner LISYclock ein (z. B. `192.168.1.42`). Die Adresse wird im Browser gespeichert und beim nächsten Öffnen automatisch vorausgefüllt.

Ein Klick auf **Connect Clock** prüft die Erreichbarkeit der Uhr:

| Anzeige | Bedeutung |
|---------|-----------|
| Grau | Noch kein Test durchgeführt |
| Grün | Verbindung erfolgreich |
| Rot | Verbindung fehlgeschlagen |

Bei erfolgreicher Verbindung werden **Firmware-Version** und **API-Version** angezeigt. Stimmt die API-Version nicht mit der erwarteten Version überein, erscheint eine **Warnung** (die Funktionen können trotzdem eingeschränkt nutzbar sein).

### USB-Modus

| Einstellung | Beschreibung |
|-------------|-------------|
| **Baud Rate** | Serielle Übertragungsrate; Standard: 115200 |
| **Reset Delay** | Wartezeit nach dem Verbinden, bevor der Handshake gesendet wird; Standard: 1500 ms |

Klick auf **Connect USB** baut die Verbindung auf. Der Status-Punkt zeigt den Zustand an:

| Anzeige | Bedeutung |
|---------|-----------|
| Grau | Nicht verbunden |
| Gelb (blinkend) | Handshake läuft – wartet auf ESP32-Reset |
| Grün | Verbunden |

> **Hinweis:** Die Uhr startet einmal neu, wenn **Connect USB** geklickt wird. Das ist normal – der Neustart löst den USB-Handshake aus. Warten bis der Status *Connected* anzeigt.

### WLAN-Verbindungstest (nur USB-Modus)

Im USB-Modus kann mit **Test WiFi Connection** die WLAN-Zugangsdaten aus dem General-Tab zur Uhr gesendet und die Verbindung geprüft werden. Der Status-Punkt zeigt das Ergebnis an.

### Konfiguration

| Schaltfläche | Funktion |
|---|---|
| **Upload Config to Clock** | Überträgt die aktuelle (im Editor angezeigte) Konfiguration zur Uhr |
| **Download Config from Clock** | Lädt die `config.txt` von der Uhr herunter und öffnet sie im Editor |

### Uhrzeit stellen

Ein Klick auf **Set Clock Time from PC** überträgt die aktuelle Uhrzeit und das Datum des PCs zur Uhr.

### Reboot

Ein Klick auf **Reboot Clock** startet die LISYclock neu. Ein Bestätigungsdialog verhindert versehentliche Neustarts.

### Datei hochladen

Beliebige Datei (z. B. MP3, Batch) auf die SD-Karte der Uhr übertragen.

### Dateiverwaltung auf der SD-Karte

- Mit **Refresh** die aktuelle Dateiliste von der SD-Karte laden
  - Die Liste zeigt Name, Größe und Änderungs**datum** jeder Datei
  - Einzelne Dateien können **heruntergeladen**, **umbenannt** oder **gelöscht** werden

### Protokoll-Log (nur USB-Modus)

Im USB-Modus erscheint ein Protokoll-Log-Panel, das die gesamte serielle Kommunikation zwischen Editor und Uhr anzeigt.

### HTTPS-Warnung

Wenn der Editor über eine HTTPS-URL geöffnet wird, blockiert der Browser alle HTTP-Anfragen zur Uhr (IP-Modus). Lösung: `LISYclock_config_editor.html` **lokal** über `file://` öffnen.

---

## 5. Tab: General

Allgemeine Einstellungen der LISYclock.

![General-Tab](pics/General%20tab.jpg)

### WLAN-Einstellungen

| Feld | Beschreibung |
|------|-------------|
| Aktiviert (Checkbox) | WLAN ein-/ausschalten (`WIFI_ENABLE=yes` / `WIFI_ENABLE=no`) |
| SSID | Name des WLAN-Netzwerks |
| Passwort | WLAN-Passwort (Anzeige per Show/Hide-Schaltfläche umschaltbar) |

> WLAN-Zugangsdaten werden in der `config.txt` gespeichert. Mit **Test WiFi Connection** im Clock-Tab (USB-Modus) lässt sich prüfen, ob die Zugangsdaten funktionieren.

### Display-Helligkeit

Wert von **0** (dunkel) bis **7** (maximale Helligkeit). Entspricht dem Konfigurationsschlüssel `DISP_BRIGHT`.

### FTP-Server

| Feld | Beschreibung |
|------|-------------|
| Aktiviert (Checkbox) | FTP-Server ein-/ausschalten (`#FTP_USER=…` wenn deaktiviert) |
| Benutzername | FTP-Login-Name |
| Passwort | FTP-Passwort |

### Zeitzone

| Feld | Beschreibung |
|------|-------------|
| Aktiviert (Checkbox) | Zeitzonen-Konfiguration ein-/ausschalten |
| POSIX-String | Zeitzone im POSIX-Format |

Beispiel für Mitteleuropa mit Sommerzeit:
```
CET-1CEST,M3.5.0,M10.5.0/3
```

### Wochentags-Strings

Wenn aktiviert, zeigt die Uhr lokalisierte Wochentagsnamen an. Jedes der 7 Felder (Sonntag–Samstag) darf **maximal 6 Zeichen** lang sein. Kürzere Einträge werden automatisch mit Leerzeichen auf 6 Zeichen aufgefüllt.

Beispiel (Deutsch): `So`, `Mo`, `Di`, `Mi`, `Do`, `Fr`, `Sa`

---

## 6. Tab: Events

Events definieren zeitgesteuerte Aktionen der LISYclock.

![Events-Tab mit mehreren Einträgen](pics/Events%20tab%20with%20multiple%20entries.jpg)

### Event-Typen

| Typ | Beschreibung | Wert-Feld |
|-----|-------------|-----------|
| **TTS** | Text per Text-to-Speech ausgeben | Freitext (z. B. „Guten Morgen!") |
| **MP3** | MP3-Datei von der SD-Karte abspielen | Dateiname (z. B. `alarm.mp3`) |
| **BATCH** | Batch-Datei von der SD-Karte ausführen | Dateiname (z. B. `script.bat`) |
| **DISPLAY** | Alle Displays ein- oder ausschalten | Dropdown: `on` / `off` |
| **SYNC_TIME** | Uhrzeit per NTP synchronisieren | Dropdown: 1–10 Wiederholungsversuche |
| **SAY_TIME** | Aktuelle Uhrzeit per TTS ansagen | Dropdown: `german` / `english` / `italian` |
| **GI_LEDS** | Alle GI-LEDs ein- oder ausschalten | Dropdown: `on` / `off` |
| **ATTRACT_LEDS** | Alle Attract-LEDs ein- oder ausschalten | Dropdown: `on` / `off` |

### Zeitformat

Die **Zeit** eines Events wird als `HH:MM:W` oder `HH:MM-TT.MM.JJJJ` angegeben. `*` steht in jedem Feld für „beliebig".

#### Wochentag-Modus: `HH:MM:W`

| Feld | Bedeutung |
|------|-----------|
| `HH` | Stunde (0–23), `*` = jede Stunde |
| `MM` | Minute (0–59), `*` = jede Minute |
| `W` | Wochentag: 0 = Sonntag, 1 = Montag, … 6 = Samstag, `*` = täglich |

#### Datum-Modus: `HH:MM-TT.MM.JJJJ`

Aktiv, wenn im Wochentag-Feld `*` (Datum) ausgewählt ist. Das Datum-Feld erscheint dann zusätzlich.

| Feld | Bedeutung |
|------|-----------|
| `TT` | Tag (1–31), `*` = jeden Tag |
| `MM` | Monat (1–12), `*` = jeden Monat |
| `JJJJ` | Jahr (vierstellig), `*` = jedes Jahr |

#### Beispiele

| Zeit | Bedeutung |
|------|-----------|
| `7:30:1` | Jeden Montag um 07:30 Uhr |
| `*:*:*` | Jede Minute, jeden Tag |
| `8:0:*` | Täglich um 08:00 Uhr |
| `12:0-24.12.*` | Jeden 24. Dezember um 12:00 Uhr |
| `*:0-*.*.*` | Jede volle Stunde, jeden Tag |

### Bedienung

- **+ Add Event** (Button oben rechts): Fügt einen neuen Event-Eintrag am Ende der Liste hinzu
- **✕** (rechts neben einem Event): Entfernt diesen Event

---

## 7. Tab: GI LEDs

Definiert die Startfarben der GI-LEDs (General Illumination).

![GI LEDs-Tab](pics/GI%20LEDs%20tab.jpg)

### Felder je LED-Zeile

| Feld | Beschreibung |
|------|-------------|
| LED | LED-Nummer (1–64) |
| R / G / B | Farbwert je Kanal (0–255) |
| Farbwähler | Öffnet einen nativen Farb-Picker |
| Vorschau-Punkt | Zeigt die aktuelle Farbe als kleinen farbigen Kreis an |

Farbwähler und RGB-Felder sind miteinander synchronisiert: Eine Änderung im Picker aktualisiert die RGB-Felder und umgekehrt.

### Bedienung

- **+ Add LED**: Neuen LED-Eintrag am Ende der Liste anfügen
- **✕**: Diesen LED-Eintrag entfernen

---

## 8. Tab: Attract Mode

Konfiguriert die Attract-Mode-Beleuchtung (Lauflichter im Standby) in **5 Gruppen** (AT1–AT5).

![Attract Mode-Tab mit aufgeklappter Gruppe AT1](pics/Attract%20Mode%20tab%20with%20group%20AT1%20expanded.jpg)

Jede Gruppe ist als **aufklappbares Accordion-Panel** dargestellt.

### Einstellungen je Gruppe

| Feld | Beschreibung |
|------|-------------|
| **Blinkrate** | Blinkintervall in Millisekunden |
| **Zufällige Aktivierung** | Checkbox: Gruppe zufällig aktivieren |
| **Rand** | Zufallsschwellwert (0–255); je höher, desto seltener wird die Gruppe aktiviert |
| **LED-Liste** | Liste der LEDs in dieser Gruppe (wie GI LEDs: Nr., R, G, B, Farbwähler) |

- **+ Add LED**: Neuen LED-Eintrag zur Gruppe hinzufügen
- **✕**: LED-Eintrag aus der Gruppe entfernen

---

## 9. Tab: TTS Settings

Einstellungen für die Text-to-Speech-Funktion (verwendet den WIT.ai-Dienst).

![TTS Settings-Tab](pics/TTS%20Settings%20tab.jpg)

| Feld | Mögliche Werte | Standardwert |
|------|---------------|-------------|
| **WIT.ai Token** | Freitext (API-Schlüssel von wit.ai) | — |
| **Voice** | 17 Stimmen (US EN, UK EN, CA EN), z. B. `wit$Cooper` | `wit$Cooper` |
| **Style** | `default` / `soft` / `formal` / `fast` / `projected` | `default` |
| **Speed** | 0–200 | 80 |
| **Pitch** | 0–200 | 80 |
| **Gain** | 0–100 | 30 |
| **SFX Char** | `none` / `chipmunk` / `monster` / `daemon` / `robot` / `alien` | `none` |
| **SFX Env** | `none` / `reverb` / `room` / `cathedral` / `radio` / `phone` | `none` |

> Den WIT.ai API-Token erhältst du nach kostenloser Registrierung auf [wit.ai](https://wit.ai).

---

## 10. Tab: Update

Der Update-Tab bietet drei Möglichkeiten, die Firmware der LISYclock zu aktualisieren.

### Lokale Firmware-Datei hochladen (nur IP-Modus)

1. `.bin`-Firmware-Datei mit dem Datei-Auswahl-Button auswählen
2. **Update & Reboot** klicken – die Datei wird als `update.bin` auf die SD-Karte übertragen und die Uhr startet automatisch neu, um das Update einzuspielen

### Oder vom lisy.dev-Server

1. Zuerst im Clock-Tab (IP-Modus) mit der Uhr verbinden, damit der Editor die Hardware-Version erkennt
2. **Fetch versions** klicken – ein Dropdown erscheint mit allen passenden Firmware-Dateien von lisy.dev
3. Die gewünschte Version aus dem Dropdown auswählen
4. **Update & Reboot** klicken – die Datei wird von lisy.dev heruntergeladen und zur Uhr übertragen

> Wenn eine Server-Version ausgewählt ist, hat das Dropdown Vorrang vor einer lokal gewählten Datei.

### Flash via USB – Vollinstallation (nur USB-Modus)

Klick auf **Flash via USB** schreibt das vollständige Firmware-Paket (Bootloader, Firmware-Binary, Partitionstabelle und OTA-Daten) direkt per USB auf den ESP32. Keine WLAN-Verbindung erforderlich.

> Nur Chrome/Edge. Während des Flashens wird ein Fortschritts-Log unter der Schaltfläche angezeigt. Das USB-Kabel nicht trennen, bis der Vorgang abgeschlossen ist.

---

## 11. Tab: Debug

Der Debug-Tab bietet eine reine USB-Seriell-Konsole für fortgeschrittene Benutzer und Entwickler.

### Verbindung

**Baud Rate** (Standard: 115200) und **Reset Delay** (Standard: 1500 ms) auswählen, dann **CONNECT** klicken. Der Status-Punkt zeigt den Zustand an:

| Zustand | Bedeutung |
|---------|-----------|
| Rot | Nicht verbunden |
| Gelb (blinkend) | Handshake läuft – wartet auf ESP32-Reset |
| Grün (blinkend) | Verbunden |

DTR- und RTS-Signalzustände werden oben rechts angezeigt.

### Optionen

| Option | Beschreibung |
|--------|-------------|
| **Handshake** | `0x55` → `OK:READY`-Handshake-Protokoll ein-/ausschalten. Deaktivieren für direktes Rohmonitoring |
| **Boot Logs** | ESP32-Bootlog-Zeilen ein-/ausblenden |
| **Auto-Scroll** | Automatisch zur neuesten Ausgabe scrollen |
| **Timestamp** | Jeder empfangenen Zeile einen Zeitstempel hinzufügen |

### Zeilenende

Das an gesendete Befehle angehängte Zeilenende auswählen: `NL (\n)`, `CR+NL`, `CR` oder `None`.

### Konsole

Die Konsole zeigt alle seriellen Ausgaben des ESP32 farbkodiert an:
- Grün: empfangene Daten
- Orange: Systemmeldungen
- Rot: Fehler
- Dunkelgrau: Boot-Meldungen (wenn Boot Logs aktiviert)

---

## 12. config.txt – Format & Aufbau

### Grundformat

```
KEY=value
```

- Jede Zeile enthält einen Schlüssel und einen Wert, getrennt durch `=`
- Eine `#` am **Zeilenanfang** deaktiviert die Zeile (ohne sie zu löschen)
- Groß-/Kleinschreibung der Schlüsselnamen ist im Editor normalisiert

### Besonderheiten

| Regel | Beschreibung |
|-------|-------------|
| FTP-Block | Kein Leerzeichen nach `#`: `#FTP_USER=lisy` |
| Zeitzone | Leerzeichen nach `#`: `# TIMEZONE="CET-1CEST,…"` |
| Wochentags-Strings | Genau 6 Zeichen, mit Leerzeichen aufgefüllt: `Mo    ` |
| `EVENT_SYNC_TIME` | Wert **ohne** Anführungszeichen: `EVENT_SYNC_TIME=2:0:*,10` |
| `EVENT_SAY_TIME` | Wert **mit** Anführungszeichen: `EVENT_SAY_TIME=14:12:*,"german"` |
| Alle anderen `EVENT_*` | Wert **mit** Anführungszeichen: `EVENT_TTS=8:0:*,"Guten Morgen!"` |

### Beispiel-Ausschnitt

```
WIFI_ENABLE=yes
WIFI_SSID="MeinNetzwerk"
WIFI_PWD="MeinPasswort"
DISP_BRIGHT=5
#FTP_USER=lisy
#FTP_PWD=lisy
# TIMEZONE="CET-1CEST,M3.5.0,M10.5.0/3"
EVENT_SYNC_TIME=2:0:*,10
EVENT_SAY_TIME=14:12:*,"german"
EVENT_TTS=8:0:*,"Guten Morgen!"
```

---

## 13. Tipps & Hinweise

- **Chrome oder Edge verwenden:** Nur diese Browser unterstützen die File System Access API für native Öffnen/Speichern-Dialoge sowie die Web Serial API für den USB-Modus. In Firefox und Safari erscheinen stattdessen Standard-Datei-Picker bzw. Download-Links; der USB-Modus ist dort nicht verfügbar.

- **Lokal öffnen für IP-Verbindung:** Wenn die Kommunikation mit der LISYclock im IP-Modus nicht funktioniert, prüfe ob `LISYclock_config_editor.html` über `file://` (lokal) und nicht über `https://` geöffnet ist.

- **FTP als Alternative:** Die fertige `config.txt` kann auch per FTP auf die SD-Karte der Uhr übertragen werden (wenn der FTP-Server in der Konfiguration aktiviert ist).

- **Unbekannte Schlüssel:** Wenn der Editor beim Laden unbekannte Schlüssel erkennt, ignoriert er sie und zeigt ein Warnbanner mit den betroffenen Zeilen an. Sie werden beim Speichern nicht übernommen. Falls du benutzerdefinierte Erweiterungen in der `config.txt` hast, sichere diese vorher separat.

- **Keine Undo-Funktion:** Der Editor hat keine Rückgängig-Funktion. Bei versehentlichen Änderungen hilft es, die Datei neu zu öffnen (ohne vorher zu speichern).

- **USB-Verbindung wird geteilt:** Eine im Clock-Tab hergestellte USB-Verbindung wird automatisch mit dem Debug-Tab geteilt und umgekehrt. Es gibt immer nur eine gleichzeitige USB-Verbindung.

---

*Erstellt für LISYclock Config Editor v1.11*
